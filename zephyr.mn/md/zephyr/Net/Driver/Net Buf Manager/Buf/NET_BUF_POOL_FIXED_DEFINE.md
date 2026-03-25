```c
macro
NET_BUF_POOL_FIXED_DEFINE(_name, _count, _data_size, _ud_size, _destroy)
  _NET_BUF_ARRAY_DEFINE(_name, _count, _ud_size);
  static uint8_t __noinit net_buf_data_##_name[_count][_data_size] __net_buf_align;
  static const struct net_buf_pool_fixed net_buf_fixed_##_name = {
  	.data_pool = (uint8_t *)net_buf_data_##_name,
  };
  static const struct net_buf_data_alloc net_buf_fixed_alloc_##_name = {
  	.cb = &net_buf_fixed_cb,
  	.alloc_data = (void *)&net_buf_fixed_##_name,
  	.max_alloc_size = _data_size,
  };
  static STRUCT_SECTION_ITERABLE(net_buf_pool, _name) =
  	NET_BUF_POOL_INITIALIZER(_name, &net_buf_fixed_alloc_##_name,
  				 _net_buf_##_name, _count, _ud_size,
  				 _destroy)
 

Expansion Preview
struct _net_buf_tx_bufs {
	uint8_t b[sizeof(struct net_buf)];
	uint8_t ud[4];
};

_Static_assert((4 <= (0xff)), "");
_Static_assert((__builtin_offsetof (struct net_buf, user_data) == __builtin_offsetof (struct _net_buf_tx_bufs, ud)),
               "" "Invalid offset");
_Static_assert((__alignof__(struct net_buf) == __alignof__(struct _net_buf_tx_bufs)), "" "Invalid alignment");
_Static_assert(
	(sizeof(struct _net_buf_tx_bufs) == ((((unsigned long) (sizeof(struct net_buf) + 4) + (
		                                       (unsigned long) (__alignof__(struct net_buf)) - 1)) / (unsigned long) (
		                                      __alignof__(struct net_buf))) * (unsigned long) (__alignof__(struct
		                                     net_buf)))), "" "Size cannot be determined");
static struct _net_buf_tx_bufs _net_buf_tx_bufs[36] __attribute__((section("." "noinit" "." "net_pkt.c" "." "83997462"))
);
static uint8_t __attribute__((section("." "noinit" "." "net_pkt.c" "." "83997463"))) net_buf_data_tx_bufs[36][128];
static const struct net_buf_pool_fixed net_buf_fixed_tx_bufs = {.data_pool = (uint8_t *) net_buf_data_tx_bufs,};
static const struct net_buf_data_alloc net_buf_fixed_alloc_tx_bufs = {
	.cb = &net_buf_fixed_cb, .alloc_data = (void *) &net_buf_fixed_tx_bufs, .max_alloc_size = 128,
};
static struct net_buf_pool tx_bufs __attribute__((section("." "_net_buf_pool" "." "static" "." "tx_bufs_")))
		__attribute__((__used__)) = {
			.free = {
				._queue = {
					.data_q = {((void *) 0), ((void *) 0)}, .lock = {},
					.wait_q = {{{(&(&tx_bufs.free._queue.wait_q)->waitq)}, {(&(&tx_bufs.free._queue.wait_q)->waitq)}}},
					.poll_events = {{(&tx_bufs.free._queue.poll_events)}, {(&tx_bufs.free._queue.poll_events)}},
				}
			},
			.lock = {}, .buf_count = 36, .uninit_count = 36, .user_data_size = 4, .destroy = ((void *) 0),
			.alloc = &net_buf_fixed_alloc_tx_bufs, .__bufs = (struct net_buf *) _net_buf_tx_bufs,
		};
```

Define a new pool for buffers based on fixed-size data
@endcond
Defines a net_buf_pool struct and the necessary memory storage (array of structs) for the needed amount of buffers. After this, the buffers can be accessed from the pool through net_buf_alloc. The pool is defined as a static variable, so if it needs to be exported outside the current module this needs to happen with the help of a separate pointer rather than an extern declaration.
The data payload of the buffers will be allocated from a byte array of fixed sized chunks. This kind of pool does not support blocking on the data allocation, so the timeout passed to net_buf_alloc will be always treated as K_NO_WAIT when trying to allocate the data. This means that allocation failures, i.e. NULL returns, must always be handled cleanly.
If provided with a custom destroy callback, this callback is responsible for eventually calling net_buf_destroy() to complete the process of returning the buffer to the pool.
Params:
`_name — Name of the pool variable.
`_count — Number of buffers in the pool.
`_data_size — Maximum data payload per buffer.
`_ud_size — User data space to reserve per buffer.
`_destroy — Optional destroy callback when buffer is freed
