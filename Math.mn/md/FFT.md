
## 算法加速：FFT 的分治逻辑

**核心思想：** 利用旋转因子 $W_N^{kn} = e^{-j\frac{2\pi}{N}kn}$ 的**对称性**，将 $O(N^2)$ 的任务拆解为 $O(N \log N)$。

DFT:
$$X(k) = \sum_{n=0}^{N-1} x(n) e^{-j\frac{2\pi}{N}kn}, \quad k = 0, 1, \dots, N-1$$
为
$$X(k) = \sum_{n=0}^{N-1} x(n) W_N^{kn}, \quad k = 0, 1, \dots, N-1$$
### 旋转因子的特性:
旋转因子 $W_N^{kn}$ 具有两个特性：

- **对称性**：$W_N^{kn + N/2} = -W_N^{kn}$
	- 过程: 
$$W_N^{kn + N/2} = e^{-j\frac{2\pi}{N}(kn+N/2)} = e^{-j\frac{2\pi}{N}kn} \cdot e^{-j\frac{2\pi}{N} \cdot N/2}= e^{-j\frac{2\pi}{N}kn} \cdot e^{-j\pi} = -e^{-j\frac{2\pi}{N}{kn}}$$
	- 其中:$e^{-j\pi} = \cos(-\pi)+j\cdot\sin(-\pi)=-1$
    
- **周期性**：$W_N^{kn + N} = W_N^{kn}$
	- 过程:
$$W_N^{kn + N} = e^{-j\frac{2\pi}{N}(kn+N)} = e^{-j\frac{2\pi}{N}kn} \cdot e^{-j\frac{2\pi}{N} \cdot N}= e^{-j\frac{2\pi}{N}kn} \cdot e^{-j2\pi} = e^{-j\frac{2\pi}{N}{kn}}$$
	- 其中:$e^{-j2\pi} = \cos(-2\pi)+j\cdot\sin(-2\pi)=1$

### 1. 奇偶拆分 (Cooley-Tukey 算法)

将 $N$ 点 DFT 拆分为两个 $N/2$ 点 DFT：

$$X[k] = E[k] + O[k] = \sum_{r=0}^{N/2-1} x[2r] W_N^{2rk} + \sum_{r=0}^{N/2-1} x[2r+1] W_N^{(2r+1)k}$$

- $E[k]$：偶数下标项的 DFT:
$$E[k] = \sum_{r=0}^{N/2-1} x[2r] W_N^{2rk}$$
- $O[k]$：奇数下标项的 DFT。
$$O[k] = \sum_{r=0}^{N/2-1} x[2r+1] W_N^{(2r+1)k} = W_N^k \sum_{r=0}^{N/2-1} x[2r+1] W_N^{2rk}$$


- **旋转因子的化简：** $W_N^{2rk} = (e^{-j\frac{2\pi}{N}})^{2rk} = e^{-j\frac{2\pi}{N/2}rk} = W_{N/2}^{rk}$。
- **提取公因子：** 在奇数项中，我们将 $W_N^{(2r+1)k}$ 拆为 $W_N^{2rk} \cdot W_N^k$。

**公式变为：**

$$X[k] = \sum_{r=0}^{N/2-1} x[2r] W_{N/2}^{rk} + W_N^k \sum_{r=0}^{N/2-1} x[2r+1] W_{N/2}^{rk}$$
 **代数推导（消除耦合的疑惑）**

计算 $X[k]$ 的后半部分，即当 $k$ 变成 $k + N/2$ 时（这里 $0 \le k < N/2$）：

$$X[k + N/2] = \sum_{r=0}^{N/2-1} x[2r] W_{N/2}^{r(k + N/2)} + W_N^{k + N/2} \sum_{r=0}^{N/2-1} x[2r+1] W_{N/2}^{r(k + N/2)}$$

现在我们分别拆解这两个求和项里的“旋转因子”：

1. **偶数项的旋转因子：**
$$W_{N/2}^{r(k + N/2)} = W_{N/2}^{rk} \cdot W_{N/2}^{r \cdot N/2}$$
    
    由于 $W_{N/2}^{r \cdot N/2} = (e^{-j\frac{2\pi}{N/2}})^{r \cdot N/2} = e^{-j 2\pi r} = 1$（复平面上转了整整 $r$ 圈），
    
    所以：**偶数项部分完全没变**，依然等于 $E[k]$。
    
2. **奇数项的旋转因子：**
    
    同样的道理，求和号内部的 $W_{N/2}^{r(k + N/2)}$ 依然等于 $W_{N/2}^{rk}$。
    
    但是！求和号外面那个提取出来的系数变了：
    $$W_N^{k + N/2} = W_N^k \cdot W_N^{N/2} = W_N^k \cdot e^{-j\frac{2\pi}{N} \cdot \frac{N}{2}} = W_N^k \cdot e^{-j\pi} = W_N^k \cdot (-1)$$
    
    所以：**奇数项部分只是变了个号**，变成了 $-W_N^k O[k]$。

**蝴蝶操作 (Butterfly Operation)**

利用 $W_N^{k+N/2} = -W_N^k$ 的特性，计算一次 $E[k]$ 和 $O[k]$ 可以同时得到两个输出：

1. $X[k] = E[k] + W_N^k O[k]$
    
2. $X[k+N/2] = E[k] - W_N^k O[k]$
    
    这使得计算量减半，每一层只需做 $N/2$ 次复数乘法。
    


### 3. 实现步骤：倒位序排序 (Bit-Reversal)

为了让最底层的两个元素（如 $x[0]$ 和 $x[4]$）在内存中相邻以便进行蝴蝶合并，必须先对原始索引进行二进制反转排序：

- $1 (001_2) \leftrightarrow 4 (100_2)$
    
- $3 (011_2) \leftrightarrow 6 (110_2)$