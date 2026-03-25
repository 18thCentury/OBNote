```c
int sx12xx_lora_config(const struct device *dev,  
              struct lora_modem_config *config)  
{  
    bool crc = !config->packet_crc_disable;  
    uint32_t bw_idx;  
    int ret;  
  
    ret = sx12xx_get_bandwidth_idx(config->bandwidth, &bw_idx);  
    if (ret < 0) {  
       LOG_ERR("Unsupported bandwidth: %d", config->bandwidth);  
       return ret;  
    }  
  
    /* Ensure available, decremented after configuration */  
    if (!modem_acquire(&dev_data)) {  
       return -EBUSY;  
    }  
  
    Radio.SetChannel(config->frequency);  
  
    if (config->tx) {  
       /* Store TX config locally for airtime calculations */  
       memcpy(&dev_data.tx_cfg, config, sizeof(dev_data.tx_cfg));  
       /* Configure radio driver */  
       Radio.SetTxConfig(MODEM_LORA, config->tx_power, 0,  
               bw_idx, config->datarate,  
               config->coding_rate, config->preamble_len,  
               false, crc, 0, 0, config->iq_inverted, 4000);  
    } else {  
       /* TODO: Get symbol timeout value from config parameters */  
       Radio.SetRxConfig(MODEM_LORA, bw_idx,  
               config->datarate, config->coding_rate,  
               0, config->preamble_len, 10, false, 0,  
               crc, false, 0, config->iq_inverted, true);  
    }  
  
    Radio.SetPublicNetwork(config->public_network);  
  
    modem_release(&dev_data);  
    return 0;  
}
```