``` c
/**
 * @brief Fill in the struct device_ops
 *
 * @param init_fn_ Initialization function
 * @param deinit_fn_ De-initialization function
 */
#define Z_DEVICE_OPS(init_fn_, deinit_fn_)     \
	{                                                                      \
		.init = (init_fn_),                                            \
		IF_ENABLED(CONFIG_DEVICE_DEINIT_SUPPORT,                       \
			   (.deinit = (deinit_fn_),))                          \
	}

```