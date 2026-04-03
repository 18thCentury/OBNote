
`Timer + ADC + DMA + oversample`



直接调用 `zephyr/drivers/clock_control/stm32_clock_control.h` 并结合 **HAL/LL 库**。

1. **配置 TIM**：设置触发输出（TRGO）。
    
2. **配置 ADC**：设置外部触发源为该 Timer 的 TRGO，开启 DMA 模式。
    
3. **DMA 中断**：在 DMA 传输完成回调中计算平均值，并更新温度。(有的mcu 可以自动计算均值)


#### 内部参考电压 ($V_{REFINT}$) 校准

芯片内部温度传感器的输出受 $V_{DDA}$ 影响。电表工作时，$V_{DDA}$ 可能会有微小波动。

- **做法**：在读取温度通道的同时，读取内部参考电压通道（Channel 17）。
    
- **公式**：利用 $V_{REFINT}$ 的出厂校准值（存储在系统内存中）反推真实的 $V_{DDA}$，再修正温度电压。

在 LL 库模式下，我们需要开启 **扫描模式（Scan Mode）**。


#### 温度 ($Temp$) 校准
