```c
#define Z_ETH_NET_DEVICE_INIT(node_id, dev_id, name, init_fn, pm, data,	\
			      config, prio, api, mtu)			\
	Z_ETH_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, 0,	\
				       init_fn, pm, data, config, prio,	\
				       api, mtu)
```