```c
static inline int lora_config(const struct device *dev,  
                struct lora_modem_config *config)  
{  
    const struct lora_driver_api *api =  
       (const struct lora_driver_api *)dev->api;  
  
    return api->config(dev, config);  
}
```

Reference to ![[sx12xx_lora_config]]
