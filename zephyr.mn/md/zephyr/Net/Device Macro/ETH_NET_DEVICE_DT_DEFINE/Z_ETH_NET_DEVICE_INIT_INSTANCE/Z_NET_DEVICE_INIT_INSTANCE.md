```c
#define Z_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, instance,	\
				   init_fn, pm, data, config, prio,	\
				   api, l2, l2_ctx_type, mtu)		\
	Z_DEVICE_STATE_DEFINE(dev_id);					\
	Z_DEVICE_DEFINE(node_id, dev_id, name, init_fn, NULL,		\
			Z_DEVICE_DT_FLAGS(node_id), pm, data,		\
			config, POST_KERNEL, prio, api,			\
			&Z_DEVICE_STATE_NAME(dev_id));			\
	NET_L2_DATA_INIT(dev_id, instance, l2_ctx_type);		\
	NET_IF_INIT(dev_id, instance, l2, mtu, NET_IF_MAX_CONFIGS)

```

[[Z_DEVICE_STATE_DEFINE]]

[[Z_DEVICE_DEFINE]]


比 [[Z_ETH_NET_DEVICE_INIT_INSTANCE-RAW]] 模式 增加了

[[NET_L2_DATA_INIT]]

[[NET_IF_INIT]]
