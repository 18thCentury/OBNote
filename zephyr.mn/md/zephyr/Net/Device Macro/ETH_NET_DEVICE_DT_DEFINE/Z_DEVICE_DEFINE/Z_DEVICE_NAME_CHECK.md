```c

/**
 * @brief Compile time check for device name length
 *
 * @param name Device name.
 */
#define Z_DEVICE_NAME_CHECK(name)                                              \
	BUILD_ASSERT(sizeof(Z_STRINGIFY(name)) <= Z_DEVICE_MAX_NAME_LEN,       \
			    Z_STRINGIFY(name) " too long")

```


```c
#define Z_DEVICE_MAX_NAME_LEN 48U
```
