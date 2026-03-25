```
static const struct w5500_config w5500_0_config = {
    .spi = SPI_DT_SPEC_INST_GET(0, SPI_WORD_SET(8)),
    .interrupt = GPIO_DT_SPEC_INST_GET(0, int_gpios),
    .reset = GPIO_DT_SPEC_INST_GET_OR(0, reset_gpios, { 0 }),
    .timeout = CONFIG_ETH_W5500_TIMEOUT,
    .mac_cfg = NET_ETH_MAC_DT_INST_CONFIG_INIT(0),
};
```

传递给 ETH_NET_DEVICE_DT_INST_DEFINE
```
ETH_NET_DEVICE_DT_INST_DEFINE(0,
            w5500_init, NULL,
            &w5500_0_runtime, &w5500_0_config,
            CONFIG_ETH_INIT_PRIORITY, &w5500_api_funcs, NET_ETH_MTU);
```
