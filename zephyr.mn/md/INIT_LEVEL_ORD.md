```c
/**
 * @brief Obtain the ordinal for an init level.
 *
 * @param level Init level (EARLY, PRE_KERNEL_1, PRE_KERNEL_2, POST_KERNEL,
 * APPLICATION, SMP).
 *
 * @return Init level ordinal.
 */
#define INIT_LEVEL_ORD(level)                                                  \
	COND_CASE_1(Z_INIT_EARLY_##level, (Z_INIT_ORD_EARLY),                  \
		    Z_INIT_PRE_KERNEL_1_##level, (Z_INIT_ORD_PRE_KERNEL_1),    \
		    Z_INIT_PRE_KERNEL_2_##level, (Z_INIT_ORD_PRE_KERNEL_2),    \
		    Z_INIT_POST_KERNEL_##level, (Z_INIT_ORD_POST_KERNEL),      \
		    Z_INIT_APPLICATION_##level, (Z_INIT_ORD_APPLICATION),      \
		    Z_INIT_SMP_##level, (Z_INIT_ORD_SMP),                      \
		    (ZERO_OR_COMPILE_ERROR(0)))

```



```
/* Helper definitions to evaluate level equality */
#define Z_INIT_EARLY_EARLY		 1
#define Z_INIT_PRE_KERNEL_1_PRE_KERNEL_1 1
#define Z_INIT_PRE_KERNEL_2_PRE_KERNEL_2 1
#define Z_INIT_POST_KERNEL_POST_KERNEL	 1
#define Z_INIT_APPLICATION_APPLICATION	 1
#define Z_INIT_SMP_SMP			 1

/* Init level ordinals */
#define Z_INIT_ORD_EARLY	0
#define Z_INIT_ORD_PRE_KERNEL_1 1
#define Z_INIT_ORD_PRE_KERNEL_2 2
#define Z_INIT_ORD_POST_KERNEL	3
#define Z_INIT_ORD_APPLICATION	4
#define Z_INIT_ORD_SMP		5

```


[E:/Project/zephyr/zephyrproject/zephyr/include/zephyr/init.h](E:\Project\zephyr\zephyrproject\zephyr\include\zephyr\init.h)
