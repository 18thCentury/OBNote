```c
macro
NET_PKT_SLAB_DEFINE(name, count)
  K_MEM_SLAB_DEFINE(name, sizeof(struct net_pkt), count, 4);
  NET_PKT_ALLOC_STATS_DEFINE(pkt_alloc_stats_##name, name)
 

Expansion Preview
_Static_assert((((sizeof(struct net_pkt)) % (4)) == 0), "" "slab_block_size must be a multiple of slab_align");
_Static_assert(((((4) & ((4) - 1)) == 0)), "" "slab_align must be a power of 2");
char __attribute__((section("." "noinit" "." "net_pkt.c" "." "k_mem_slab_buf_tx_pkts"))) _k_mem_slab_buf_tx_pkts[
	(8) 
	* ( // 字节对齐
		(   
			((unsigned long) (sizeof(struct net_pkt)) + ((unsigned long) (sizeof(void *)) - 1))  /  (unsigned long) (sizeof(void *))
		)
	    * (unsigned long) (sizeof(void *))
	)];
struct k_mem_slab tx_pkts __attribute__((section("." "_k_mem_slab" "." "static" "." "tx_pkts_"))) __attribute__((__used__)) = {
	.wait_q = {{{(&(&(tx_pkts).wait_q)->waitq)}, {(&(&(tx_pkts).wait_q)->waitq)}}}, 
	.lock = {},
	.buffer = _k_mem_slab_buf_tx_pkts, .free_list = ((void *) 0),
	.info = {
		8,
		((((unsigned long) (sizeof(struct net_pkt)) + ((unsigned long) (sizeof(void *)) - 1)) / (unsigned long) (sizeof(
			  void *))) * (unsigned long) (sizeof(void *))),
		0
	}
};
```


#### Create a net_pkt slab
A net_pkt slab is used to store meta-information about network packets. It must be coupled with a data fragment pool (@ref `NET_PKT_DATA_POOL_DEFINE`) used to store the actual packet data. The macro can be used by an application to define additional custom per-context TX packet slabs (see `net_context_setup_pools()`).
Params:
	name — Name of the slab.
	count — Number of net_pkt in this slab.
