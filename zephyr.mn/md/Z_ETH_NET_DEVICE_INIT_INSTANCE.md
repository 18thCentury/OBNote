```c
#if !defined(CONFIG_ETH_DRIVER_RAW_MODE)

#define Z_ETH_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, instance,	\
				       init_fn, pm, data, config, prio,	\
				       api, mtu)			\
	Z_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, instance,	\
				   init_fn, pm, data, config, prio,	\
				   api, ETHERNET_L2,			\
				   NET_L2_GET_CTX_TYPE(ETHERNET_L2), mtu)

#else /* CONFIG_ETH_DRIVER_RAW_MODE */

#endif /* CONFIG_ETH_DRIVER_RAW_MODE */

```