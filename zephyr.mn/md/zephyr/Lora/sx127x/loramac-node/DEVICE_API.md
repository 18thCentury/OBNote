```c
static DEVICE_API(lora, sx127x_lora_api) = {  
    .config = sx12xx_lora_config,  
    .airtime = sx12xx_airtime,  
    .send = sx12xx_lora_send,  
    .send_async = sx12xx_lora_send_async,  
    .recv = sx12xx_lora_recv,  
    .recv_async = sx12xx_lora_recv_async,  
    .test_cw = sx12xx_lora_test_cw,  
};
```

expand to:

```c
static const struct lora_driver_api sx127x_lora_api 
		__attribute__((section("." "_lora_driver_api" "." "static" "." "sx127x_lora_api_"))) 
		__attribute__((__used__)) = 
{
//...
};
```