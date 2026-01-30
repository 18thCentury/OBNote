```c
/**
 * @brief Issue an error if the given init level is not supported.
 *
 * @param level Init level
 */
#define Z_DEVICE_CHECK_INIT_LEVEL(level)                                       \
	COND_CASE_1(Z_INIT_PRE_KERNEL_1_##level, (),                           \
		    Z_INIT_PRE_KERNEL_2_##level, (),                           \
		    Z_INIT_POST_KERNEL_##level, (),                            \
		    (ZERO_OR_COMPILE_ERROR(0)))

```
