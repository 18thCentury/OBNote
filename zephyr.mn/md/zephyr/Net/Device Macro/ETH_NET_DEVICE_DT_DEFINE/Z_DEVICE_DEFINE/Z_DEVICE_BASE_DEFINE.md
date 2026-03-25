定义 `static struct device DEVICE_NAME_GET(dev_id) __attribute__( section() )`

```c
/**
 * @brief Define a @ref device
 *
 * @param node_id Devicetree node id for the device (DT_INVALID_NODE if a
 * software device).
 * @param dev_id Device identifier (used to name the defined @ref device).
 * @param name Name of the device.
 * @param init_fn Init function.
 * @param deinit_fn De-init function.
 * @param flags Device flags.
 * @param pm Reference to @ref pm_device_base associated with the device.
 * (optional).
 * @param data Reference to device data.
 * @param config Reference to device config.
 * @param level Initialization level.
 * @param prio Initialization priority.
 * @param api Reference to device API.
 * @param ... Optional dependencies, manually specified.
 */
#define Z_DEVICE_BASE_DEFINE(node_id, dev_id, name, init_fn, deinit_fn, flags, pm, data, config, level, prio, api, state, deps)    \
	COND_CODE_1(DT_NODE_EXISTS(node_id), (), (static))    \
	COND_CODE_1(Z_DEVICE_IS_MUTABLE(node_id), (), (const))    \
	STRUCT_SECTION_ITERABLE_NAMED_ALTERNATE(    \
		device, COND_CODE_1(Z_DEVICE_IS_MUTABLE(node_id), (device_mutable), (device)),     \
		Z_DEVICE_SECTION_NAME(level, prio), DEVICE_NAME_GET(dev_id)) =    \
		Z_DEVICE_INIT(name, init_fn, deinit_fn, flags, pm, data, config, api, state, deps, node_id, dev_id)
```










```