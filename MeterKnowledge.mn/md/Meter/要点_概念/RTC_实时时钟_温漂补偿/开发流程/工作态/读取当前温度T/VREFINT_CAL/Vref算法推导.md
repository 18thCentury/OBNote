推导:
变量:
$V\_in$:实际温度输入.
$V\_ref\_cal\_adcv$ : ref 电压在标准温度下的标准ADC值
$V\_ref\_adcv$ : 当前的ref 电压ADC值
$V\_ref\_in$ : 参考标准电压.

计算式1:
$$VDD\_real = VDD * V\_ref\_adcv/V\_ref\_cal_adcv$$ 计算式2:
$$ADC\_value =  V\_in/Vdd * 2^{精度位} $$
计算式3:
$$ADC\_value\_real = V\_in / Vdd\_real *2^{精度位} $$
将计算式1带入计算式3:
$$ADC\_value\_real = \frac{V\_in}{(\frac{Vdd *V\_ref\_adcv}{V\_ref\_cal\_adcv})} *2^{精度位} = \frac{V\_in}{Vdd} * 2^{精度位} * \frac{V\_ref\_cal\_adcv}{V\_ref\_cal\_adcv} = ADC\_value * \frac{V\_ref\_cal\_adcv}{V\_ref\_cal\_adcv}$$
