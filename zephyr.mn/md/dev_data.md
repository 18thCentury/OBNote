```c
static struct sx12xx_data {  
    const struct device *dev;  
    struct k_poll_signal *operation_done;  
    lora_recv_cb async_rx_cb;  
    void *async_user_data;  
    RadioEvents_t events;  
    struct lora_modem_config tx_cfg;  
    atomic_t modem_usage;  
    struct sx12xx_rx_params rx_params;  
} dev_data;
```