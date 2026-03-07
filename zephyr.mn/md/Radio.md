```c
/* Initialize Radio driver callbacks */  
const struct Radio_s Radio = {  
    .Init = SX127xInit,  
    .GetStatus = SX127xGetStatus,  
    .SetModem = SX127xSetModem,  
    .SetChannel = SX127xSetChannel,  
    .IsChannelFree = SX127xIsChannelFree,  
    .Random = SX127xRandom,  
    .SetRxConfig = SX127xSetRxConfig,  
    .SetTxConfig = SX127xSetTxConfig,  
    .CheckRfFrequency = SX127xCheckRfFrequency,  
    .TimeOnAir = SX127xGetTimeOnAir,  
    .Send = SX127xSend,  
    .Sleep = SX127xSetSleep,  
    .Standby = SX127xSetStby,  
    .Rx = SX127xSetRx,  
    .Write = SX127xWrite,  
    .Read = SX127xRead,  
    .WriteBuffer = SX127xWriteBuffer,  
    .ReadBuffer = SX127xReadBuffer,  
    .SetMaxPayloadLength = SX127xSetMaxPayloadLength,  
    .SetPublicNetwork = SX127xSetPublicNetwork,  
    .GetWakeupTime = SX127xGetWakeupTime,  
    .IrqProcess = NULL,  
    .RxBoosted = NULL,  
    .SetRxDutyCycle = NULL,  
    .SetTxContinuousWave = SX127xSetTxContinuousWave,  
};
```

```c
#define SX127xCheckRfFrequency SX1276CheckRfFrequency  
#define SX127xGetBoardTcxoWakeupTime SX1276GetBoardTcxoWakeupTime  
#define SX127xSetAntSwLowPower SX1276SetAntSwLowPower  
#define SX127xSetBoardTcxo SX1276SetBoardTcxo  
#define SX127xSetAntSw SX1276SetAntSw  
#define SX127xReset SX1276Reset  
#define SX127xIoIrqInit SX1276IoIrqInit  
#define SX127xWriteBuffer SX1276WriteBuffer  
#define SX127xReadBuffer SX1276ReadBuffer  
#define SX127xSetRfTxPower SX1276SetRfTxPower  
#define SX127xGetDio1PinState SX1276GetDio1PinState  
#define SX127xInit SX1276Init  
#define SX127xGetStatus SX1276GetStatus  
#define SX127xSetModem SX1276SetModem  
#define SX127xSetChannel SX1276SetChannel  
#define SX127xIsChannelFree SX1276IsChannelFree  
#define SX127xRandom SX1276Random  
#define SX127xSetRxConfig SX1276SetRxConfig  
#define SX127xSetTxConfig SX1276SetTxConfig  
#define SX127xGetTimeOnAir SX1276GetTimeOnAir  
#define SX127xSend SX1276Send  
#define SX127xSetSleep SX1276SetSleep  
#define SX127xSetStby SX1276SetStby  
#define SX127xSetRx SX1276SetRx  
#define SX127xWrite SX1276Write  
#define SX127xRead SX1276Read  
#define SX127xSetMaxPayloadLength SX1276SetMaxPayloadLength  
#define SX127xSetPublicNetwork SX1276SetPublicNetwork  
#define SX127xGetWakeupTime SX1276GetWakeupTime  
#define SX127xSetTxContinuousWave SX1276SetTxContinuousWave
```