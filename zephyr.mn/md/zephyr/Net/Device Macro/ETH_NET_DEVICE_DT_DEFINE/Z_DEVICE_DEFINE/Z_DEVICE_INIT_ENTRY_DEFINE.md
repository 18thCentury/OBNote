
```c
/**
 * @brief Define the init entry for a device.
 *
 * @param node_id Devicetree node id for the device (DT_INVALID_NODE if a
 * software device).
 * @param dev_id Device identifier.
 * @param level Initialization level.
 * @param prio Initialization priority.
 */
#define Z_DEVICE_INIT_ENTRY_DEFINE(node_id, dev_id, level, prio)         \
	Z_DEVICE_CHECK_INIT_LEVEL(level)         \
         \
	static const Z_DECL_ALIGN(struct init_entry) __used __noasan Z_INIT_ENTRY_SECTION(         \
		level, prio, Z_DEVICE_INIT_SUB_PRIO(node_id))         \
		Z_INIT_ENTRY_NAME(DEVICE_NAME_GET(dev_id)) = {         \
			.init_fn = NULL,         \
			.dev = (const struct device *)&DEVICE_NAME_GET(dev_id),         \
		}
```


[[Z_DEVICE_CHECK_INIT_LEVEL]]
[[Z_DECL_ALIGN]]
[[Z_INIT_ENTRY_SECTION]]
[[Z_DEVICE_INIT_SUB_PRIO]]
[[DEVICE_NAME_GET]]
[[Z_INIT_ENTRY_NAME]]


注释:
**`__used` 和 `__noasan`**:

- `__used` 告诉编译器该符号即使未直接引用，也不要优化掉它。
    
- `__noasan` 禁用地址空间布局随机化（ASLR），确保这个符号在程序加载时的地址位置是固定的。