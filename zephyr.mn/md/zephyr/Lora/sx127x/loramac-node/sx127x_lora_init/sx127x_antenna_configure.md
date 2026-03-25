```c
static int sx127x_antenna_configure(void)  
{  
    int ret;  
  
    ret = sx12xx_configure_pin(antenna_enable, GPIO_OUTPUT_INACTIVE);  
    if (ret) {  
       return ret;  
    }  
  
    ret = sx12xx_configure_pin(rfi_enable, GPIO_OUTPUT_INACTIVE);  
    if (ret) {  
       return ret;  
    }  
  
    ret = sx12xx_configure_pin(rfo_enable, GPIO_OUTPUT_INACTIVE);  
    if (ret) {  
       return ret;  
    }  
  
    ret = sx12xx_configure_pin(pa_boost_enable, GPIO_OUTPUT_INACTIVE);  
    if (ret) {  
       return ret;  
    }  
  
    return 0;  
}
```