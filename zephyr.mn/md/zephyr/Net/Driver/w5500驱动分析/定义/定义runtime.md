```
static struct w5500_runtime w5500_0_runtime = {
    .tx_sem = Z_SEM_INITIALIZER(w5500_0_runtime.tx_sem,
                    1,  UINT_MAX),
    .int_sem  = Z_SEM_INITIALIZER(w5500_0_runtime.int_sem,
                      0, UINT_MAX),
};
```

定义信号量
