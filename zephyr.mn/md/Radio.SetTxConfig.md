```c
void SX1276SetTxConfig( RadioModems_t modem, int8_t power, uint32_t fdev,  
                        uint32_t bandwidth, uint32_t datarate,  
                        uint8_t coderate, uint16_t preambleLen,  
                        bool fixLen, bool crcOn, bool freqHopOn,  
                        uint8_t hopPeriod, bool iqInverted, uint32_t timeout )  
{  
    SX1276SetModem( modem );  
  
    SX1276SetStby( );  
  
    SX1276SetRfTxPower( power );  
  
    switch( modem )  
    {  
    case MODEM_FSK:  
        {  
            SX1276.Settings.Fsk.Power = power;  
            SX1276.Settings.Fsk.Fdev = fdev;  
            SX1276.Settings.Fsk.Bandwidth = bandwidth;  
            SX1276.Settings.Fsk.Datarate = datarate;  
            SX1276.Settings.Fsk.PreambleLen = preambleLen;  
            SX1276.Settings.Fsk.FixLen = fixLen;  
            SX1276.Settings.Fsk.CrcOn = crcOn;  
            SX1276.Settings.Fsk.IqInverted = iqInverted;  
            SX1276.Settings.Fsk.TxTimeout = timeout;  
  
            uint32_t fdevInPllSteps = SX1276ConvertFreqInHzToPllStep( fdev );  
            SX1276Write( REG_FDEVMSB, ( uint8_t )( fdevInPllSteps >> 8 ) );  
            SX1276Write( REG_FDEVLSB, ( uint8_t )( fdevInPllSteps & 0xFF ) );  
  
            uint32_t bitRate = ( uint32_t )( SX1276_XTAL_FREQ / datarate );  
            SX1276Write( REG_BITRATEMSB, ( uint8_t )( bitRate >> 8 ) );  
            SX1276Write( REG_BITRATELSB, ( uint8_t )( bitRate & 0xFF ) );  
  
            SX1276Write( REG_PREAMBLEMSB, ( preambleLen >> 8 ) & 0x00FF );  
            SX1276Write( REG_PREAMBLELSB, preambleLen & 0xFF );  
  
            SX1276Write( REG_PACKETCONFIG1,  
                         ( SX1276Read( REG_PACKETCONFIG1 ) &  
                           RF_PACKETCONFIG1_CRC_MASK &  
                           RF_PACKETCONFIG1_PACKETFORMAT_MASK ) |  
                           ( ( fixLen == 1 ) ? RF_PACKETCONFIG1_PACKETFORMAT_FIXED : RF_PACKETCONFIG1_PACKETFORMAT_VARIABLE ) |  
                           ( crcOn << 4 ) );  
            SX1276Write( REG_PACKETCONFIG2, ( SX1276Read( REG_PACKETCONFIG2 ) | RF_PACKETCONFIG2_DATAMODE_PACKET ) );  
        }  
        break;  
    case MODEM_LORA:  
        {  
            SX1276.Settings.LoRa.Power = power;  
            if( bandwidth > 2 )  
            {  
                // Fatal error: When using LoRa modem only bandwidths 125, 250 and 500 kHz are supported  
                while( 1 );  
            }  
            bandwidth += 7;  
            SX1276.Settings.LoRa.Bandwidth = bandwidth;  
            SX1276.Settings.LoRa.Datarate = datarate;  
            SX1276.Settings.LoRa.Coderate = coderate;  
            SX1276.Settings.LoRa.PreambleLen = preambleLen;  
            SX1276.Settings.LoRa.FixLen = fixLen;  
            SX1276.Settings.LoRa.FreqHopOn = freqHopOn;  
            SX1276.Settings.LoRa.HopPeriod = hopPeriod;  
            SX1276.Settings.LoRa.CrcOn = crcOn;  
            SX1276.Settings.LoRa.IqInverted = iqInverted;  
            SX1276.Settings.LoRa.TxTimeout = timeout;  
  
            if( datarate > 12 )  
            {  
                datarate = 12;  
            }  
            else if( datarate < 6 )  
            {  
                datarate = 6;  
            }  
            if( ( ( bandwidth == 7 ) && ( ( datarate == 11 ) || ( datarate == 12 ) ) ) ||  
                ( ( bandwidth == 8 ) && ( datarate == 12 ) ) )  
            {  
                SX1276.Settings.LoRa.LowDatarateOptimize = 0x01;  
            }  
            else  
            {  
                SX1276.Settings.LoRa.LowDatarateOptimize = 0x00;  
            }  
  
            if( SX1276.Settings.LoRa.FreqHopOn == true )  
            {  
                SX1276Write( REG_LR_PLLHOP, ( SX1276Read( REG_LR_PLLHOP ) & RFLR_PLLHOP_FASTHOP_MASK ) | RFLR_PLLHOP_FASTHOP_ON );  
                SX1276Write( REG_LR_HOPPERIOD, SX1276.Settings.LoRa.HopPeriod );  
            }  
  
            SX1276Write( REG_LR_MODEMCONFIG1,  
                         ( SX1276Read( REG_LR_MODEMCONFIG1 ) &  
                           RFLR_MODEMCONFIG1_BW_MASK &  
                           RFLR_MODEMCONFIG1_CODINGRATE_MASK &  
                           RFLR_MODEMCONFIG1_IMPLICITHEADER_MASK ) |  
                           ( bandwidth << 4 ) | ( coderate << 1 ) |  
                           fixLen );  
  
            SX1276Write( REG_LR_MODEMCONFIG2,  
                         ( SX1276Read( REG_LR_MODEMCONFIG2 ) &  
                           RFLR_MODEMCONFIG2_SF_MASK &  
                           RFLR_MODEMCONFIG2_RXPAYLOADCRC_MASK ) |  
                           ( datarate << 4 ) | ( crcOn << 2 ) );  
  
            SX1276Write( REG_LR_MODEMCONFIG3,  
                         ( SX1276Read( REG_LR_MODEMCONFIG3 ) &  
                           RFLR_MODEMCONFIG3_LOWDATARATEOPTIMIZE_MASK ) |  
                           ( SX1276.Settings.LoRa.LowDatarateOptimize << 3 ) );  
  
            SX1276Write( REG_LR_PREAMBLEMSB, ( preambleLen >> 8 ) & 0x00FF );  
            SX1276Write( REG_LR_PREAMBLELSB, preambleLen & 0xFF );  
  
            if( datarate == 6 )  
            {  
                SX1276Write( REG_LR_DETECTOPTIMIZE,  
                             ( SX1276Read( REG_LR_DETECTOPTIMIZE ) &  
                               RFLR_DETECTIONOPTIMIZE_MASK ) |  
                               RFLR_DETECTIONOPTIMIZE_SF6 );  
                SX1276Write( REG_LR_DETECTIONTHRESHOLD,  
                             RFLR_DETECTIONTHRESH_SF6 );  
            }  
            else  
            {  
                SX1276Write( REG_LR_DETECTOPTIMIZE,  
                             ( SX1276Read( REG_LR_DETECTOPTIMIZE ) &  
                             RFLR_DETECTIONOPTIMIZE_MASK ) |  
                             RFLR_DETECTIONOPTIMIZE_SF7_TO_SF12 );  
                SX1276Write( REG_LR_DETECTIONTHRESHOLD,  
                             RFLR_DETECTIONTHRESH_SF7_TO_SF12 );  
            }  
        }  
        break;  
    }  
}
```