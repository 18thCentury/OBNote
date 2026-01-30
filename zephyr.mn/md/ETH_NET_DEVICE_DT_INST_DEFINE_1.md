

```
ETH_NET_DEVICE_DT_INST_DEFINE(0,
		    w5500_init, NULL,
		    &w5500_0_runtime, &w5500_0_config,
		    CONFIG_ETH_INIT_PRIORITY, &w5500_api_funcs, NET_ETH_MTU);
```