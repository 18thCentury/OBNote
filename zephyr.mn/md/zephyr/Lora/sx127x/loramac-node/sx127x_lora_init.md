```c
static int sx127x_lora_init(const struct device *dev)  
{  
    int ret;  
    uint8_t regval;  
  
    if (!spi_is_ready_dt(&dev_config.bus)) {  
       LOG_ERR("SPI device not ready");  
       return -ENODEV;  
    }  
  
    ret = sx12xx_configure_pin(tcxo_power, GPIO_OUTPUT_INACTIVE);  
    if (ret) {  
       return ret;  
    }  
  
    /* Setup Reset gpio and perform soft reset */  
    ret = sx12xx_configure_pin(reset, GPIO_OUTPUT_ACTIVE);  
    if (ret) {  
       return ret;  
    }  
  
    k_sleep(K_MSEC(100));  
    gpio_pin_set_dt(&dev_config.reset, 0);  
    k_sleep(K_MSEC(100));  
  
    ret = sx127x_read(REG_VERSION, &regval, 1);  
    if (ret < 0) {  
       LOG_ERR("Unable to read version info");  
       return -EIO;  
    }  
  
    LOG_INF("SX127x version 0x%02x found", regval);  
  
    ret = sx127x_antenna_configure();  
    if (ret < 0) {  
       LOG_ERR("Unable to configure antenna");  
       return -EIO;  
    }  
  
    ret = sx12xx_init(dev);  
    if (ret < 0) {  
       LOG_ERR("Failed to initialize SX12xx common");  
       return ret;  
    }  
  
    return 0;  
}
```