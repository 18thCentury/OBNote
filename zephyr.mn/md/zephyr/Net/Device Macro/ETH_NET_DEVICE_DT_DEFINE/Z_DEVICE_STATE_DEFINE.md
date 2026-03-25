```
/**
 * @brief Utility macro to define and initialize the device state.
 *
 * @param dev_id Device identifier.
 */
#define Z_DEVICE_STATE_DEFINE(dev_id) \
	static Z_DECL_ALIGN(struct device_state) Z_DEVICE_STATE_NAME(dev_id)   \
		__attribute__((__section__(".z_devstate")))
```