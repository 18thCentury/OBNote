```c
void SX1276SetRx( uint32_t timeout )  
{  
    bool rxContinuous = false;  
    TimerStop( &TxTimeoutTimer );  
  
    switch( SX1276.Settings.Modem )  
    {  
    case MODEM_FSK:  
        {  
            rxContinuous = SX1276.Settings.Fsk.RxContinuous;  
  
            // DIO0=PayloadReady  
            // DIO1=FifoLevel            
            // DIO2=SyncAddr            
            // DIO3=FifoEmpty            
            // DIO4=Preamble            
            // DIO5=ModeReady            
            SX1276Write( REG_DIOMAPPING1, ( SX1276Read( REG_DIOMAPPING1 ) & RF_DIOMAPPING1_DIO0_MASK &     
																            RF_DIOMAPPING1_DIO1_MASK &  
                                                                            RF_DIOMAPPING1_DIO2_MASK ) |  
                                                                            RF_DIOMAPPING1_DIO0_00 |  
                                                                            RF_DIOMAPPING1_DIO1_00 |  
                                                                            RF_DIOMAPPING1_DIO2_11 );  
  
            SX1276Write( REG_DIOMAPPING2, ( SX1276Read( REG_DIOMAPPING2 ) & RF_DIOMAPPING2_DIO4_MASK &  
                                                                            RF_DIOMAPPING2_MAP_MASK ) |  
                                                                            RF_DIOMAPPING2_DIO4_11 |  
                                                                            RF_DIOMAPPING2_MAP_PREAMBLEDETECT );  
  
            SX1276.Settings.FskPacketHandler.FifoThresh = SX1276Read( REG_FIFOTHRESH ) & 0x3F;  
  
            SX1276Write( REG_RXCONFIG, RF_RXCONFIG_AFCAUTO_ON | RF_RXCONFIG_AGCAUTO_ON | RF_RXCONFIG_RXTRIGER_PREAMBLEDETECT );  
  
            SX1276.Settings.FskPacketHandler.PreambleDetected = false;  
            SX1276.Settings.FskPacketHandler.SyncWordDetected = false;  
            SX1276.Settings.FskPacketHandler.NbBytes = 0;  
            SX1276.Settings.FskPacketHandler.Size = 0;  
        }  
        break;  
    case MODEM_LORA:  
        {  
            if( SX1276.Settings.LoRa.IqInverted == true )  
            {  
                SX1276Write( REG_LR_INVERTIQ, ( ( SX1276Read( REG_LR_INVERTIQ ) & RFLR_INVERTIQ_TX_MASK & RFLR_INVERTIQ_RX_MASK ) | RFLR_INVERTIQ_RX_ON | RFLR_INVERTIQ_TX_OFF ) );  
                SX1276Write( REG_LR_INVERTIQ2, RFLR_INVERTIQ2_ON );  
            }  
            else  
            {  
                SX1276Write( REG_LR_INVERTIQ, ( ( SX1276Read( REG_LR_INVERTIQ ) & RFLR_INVERTIQ_TX_MASK & RFLR_INVERTIQ_RX_MASK ) | RFLR_INVERTIQ_RX_OFF | RFLR_INVERTIQ_TX_OFF ) );  
                SX1276Write( REG_LR_INVERTIQ2, RFLR_INVERTIQ2_OFF );  
            }  
  
            // ERRATA 2.3 - Receiver Spurious Reception of a LoRa Signal  
            if( SX1276.Settings.LoRa.Bandwidth < 9 )  
            {  
                SX1276Write( REG_LR_DETECTOPTIMIZE, SX1276Read( REG_LR_DETECTOPTIMIZE ) & 0x7F );  
                SX1276Write( REG_LR_IFFREQ2, 0x00 );  
                switch( SX1276.Settings.LoRa.Bandwidth )  
                {  
                case 0: // 7.8 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x48 );  
                    SX1276SetChannel(SX1276.Settings.Channel + 7810 );  
                    break;  
                case 1: // 10.4 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x44 );  
                    SX1276SetChannel(SX1276.Settings.Channel + 10420 );  
                    break;  
                case 2: // 15.6 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x44 );  
                    SX1276SetChannel(SX1276.Settings.Channel + 15620 );  
                    break;  
                case 3: // 20.8 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x44 );  
                    SX1276SetChannel(SX1276.Settings.Channel + 20830 );  
                    break;  
                case 4: // 31.2 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x44 );  
                    SX1276SetChannel(SX1276.Settings.Channel + 31250 );  
                    break;  
                case 5: // 41.4 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x44 );  
                    SX1276SetChannel(SX1276.Settings.Channel + 41670 );  
                    break;  
                case 6: // 62.5 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x40 );  
                    break;  
                case 7: // 125 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x40 );  
                    break;  
                case 8: // 250 kHz  
                    SX1276Write( REG_LR_IFFREQ1, 0x40 );  
                    break;  
                }  
            }  
            else  
            {  
                SX1276Write( REG_LR_DETECTOPTIMIZE, SX1276Read( REG_LR_DETECTOPTIMIZE ) | 0x80 );  
            }  
  
            rxContinuous = SX1276.Settings.LoRa.RxContinuous;  
  
            if( SX1276.Settings.LoRa.FreqHopOn == true )  
            {  
                SX1276Write( REG_LR_IRQFLAGSMASK, //RFLR_IRQFLAGS_RXTIMEOUT |  
                                                  //RFLR_IRQFLAGS_RXDONE |                                                  //RFLR_IRQFLAGS_PAYLOADCRCERROR |                                                  RFLR_IRQFLAGS_VALIDHEADER |  
                                                  RFLR_IRQFLAGS_TXDONE |  
                                                  RFLR_IRQFLAGS_CADDONE |  
                                                  //RFLR_IRQFLAGS_FHSSCHANGEDCHANNEL |  
                                                  RFLR_IRQFLAGS_CADDETECTED );  
  
                // DIO0=RxDone, DIO2=FhssChangeChannel  
                SX1276Write( REG_DIOMAPPING1, ( SX1276Read( REG_DIOMAPPING1 ) & RFLR_DIOMAPPING1_DIO0_MASK & RFLR_DIOMAPPING1_DIO2_MASK  ) | RFLR_DIOMAPPING1_DIO0_00 | RFLR_DIOMAPPING1_DIO2_00 );  
            }  
            else  
            {  
                SX1276Write( REG_LR_IRQFLAGSMASK, //RFLR_IRQFLAGS_RXTIMEOUT |  
                                                  //RFLR_IRQFLAGS_RXDONE |                                                  //RFLR_IRQFLAGS_PAYLOADCRCERROR |                                                  RFLR_IRQFLAGS_VALIDHEADER |  
                                                  RFLR_IRQFLAGS_TXDONE |  
                                                  RFLR_IRQFLAGS_CADDONE |  
                                                  RFLR_IRQFLAGS_FHSSCHANGEDCHANNEL |  
                                                  RFLR_IRQFLAGS_CADDETECTED );  
  
                // DIO0=RxDone  
                SX1276Write( REG_DIOMAPPING1, ( SX1276Read( REG_DIOMAPPING1 ) & RFLR_DIOMAPPING1_DIO0_MASK ) | RFLR_DIOMAPPING1_DIO0_00 );  
            }  
            SX1276Write( REG_LR_FIFORXBASEADDR, 0 );  
            SX1276Write( REG_LR_FIFOADDRPTR, 0 );  
        }  
        break;  
    }  
  
    SX1276.Settings.State = RF_RX_RUNNING;  
    if( timeout != 0 )  
    {  
        TimerSetValue( &RxTimeoutTimer, timeout );  
        TimerStart( &RxTimeoutTimer );  
    }  
  
    if( SX1276.Settings.Modem == MODEM_FSK )  
    {  
        SX1276SetOpMode( RF_OPMODE_RECEIVER );  
  
        if( rxContinuous == false )  
        {  
            TimerSetValue( &RxTimeoutSyncWord, SX1276.Settings.Fsk.RxSingleTimeout );  
            TimerStart( &RxTimeoutSyncWord );  
        }  
    }  
    else  
    {  
        if( rxContinuous == true )  
        {  
            SX1276SetOpMode( RFLR_OPMODE_RECEIVER );  
        }  
        else  
        {  
            SX1276SetOpMode( RFLR_OPMODE_RECEIVER_SINGLE );  
        }  
    }  
}
```