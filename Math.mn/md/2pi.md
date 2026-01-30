### 狄拉克 $\delta$ 函数的归一化

你之前提到的推导中，关键一步是这个积分：

$$\int_{-\infty}^{\infty} e^{i\omega(t-\tau)} d\omega$$

在数学上，这个积分的结果并不是简单的 $\delta(t-\tau)$，而是：

$$\int_{-\infty}^{\infty} e^{i\omega(t-\tau)} d\omega = 2\pi \delta(t-\tau)$$

为什么多了一个 $2\pi$？

我们可以通过对 $\delta$ 函数的定义进行检验。 $\delta$ 函数必须满足：$\int \delta(t) dt = 1$。

如果你去计算 $\frac{1}{2\pi} \int_{-\infty}^{\infty} \left[ \int_{-\infty}^{\infty} e^{i\omega t} d\omega \right] dt$，你会发现如果不除以 $2\pi$，这个能量（面积）就会变成 $2\pi$ 而不是 $1$。