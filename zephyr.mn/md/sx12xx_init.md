```c
int sx12xx_init(const struct device *dev)  
{  
    atomic_set(&dev_data.modem_usage, 0);  
  
    dev_data.dev = dev;  
    dev_data.events.TxDone = sx12xx_ev_tx_done;  
    dev_data.events.RxDone = sx12xx_ev_rx_done;  
    dev_data.events.RxError = sx12xx_ev_rx_error;  
    /* TX timeout event raises at the end of the test CW transmission */  
    dev_data.events.TxTimeout = sx12xx_ev_tx_timed_out;  
    Radio.Init(&dev_data.events);  

/*  
 * Automatically place the radio into sleep mode upon boot. 
   * The required `lora_config` call before transmission or reception 
     * will bring the radio out of sleep mode before it is used. The radio 
       * is automatically placed back into sleep mode upon TX or RX * completion. 
         */  
    
    Radio.Sleep();  
  
    return 0;  
}
```