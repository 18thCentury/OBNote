```
/** @brief Determine if a DT node is mutable */
#define Z_DEVICE_IS_MUTABLE(node_id) \
	COND_CODE_1(IS_ENABLED(CONFIG_DEVICE_MUTABLE), (DT_PROP(node_id, zephyr_mutable)), (0))

```

[[CONFIG_DEVICE_MUTABLE]]
