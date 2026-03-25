```c
void SX1276Init( RadioEvents_t *events )  
{  
    uint8_t i;  
  
    RadioEvents = events;  
  
    // Initialize driver timeout timers  
    TimerInit( &TxTimeoutTimer, SX1276OnTimeoutIrq );  
    TimerInit( &RxTimeoutTimer, SX1276OnTimeoutIrq );  
    TimerInit( &RxTimeoutSyncWord, SX1276OnTimeoutIrq );  
  
    SX1276Reset( );  
  
    RxChainCalibration( );  
  
    SX1276SetOpMode( RF_OPMODE_SLEEP );  
  
    SX1276IoIrqInit( DioIrq );  
  
    for( i = 0; i < sizeof( RadioRegsInit ) / sizeof( RadioRegisters_t ); i++ )  
    {  
        SX1276SetModem( RadioRegsInit[i].Modem );  
        SX1276Write( RadioRegsInit[i].Addr, RadioRegsInit[i].Value );  
    }  
  
    SX1276SetModem( MODEM_FSK );  
  
    SX1276.Settings.State = RF_IDLE;  
}
```