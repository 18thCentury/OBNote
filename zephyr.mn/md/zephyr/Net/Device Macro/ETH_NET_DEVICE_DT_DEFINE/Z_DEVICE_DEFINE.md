```c
/**
 * @brief Define a @ref device and all other required objects.
 *
 * This is the common macro used to define @ref device objects. It can be used
 * to define both Devicetree and software devices.
 *
 * @param node_id Devicetree node id for the device (DT_INVALID_NODE if a
 * software device).
 * @param dev_id Device identifier (used to name the defined @ref device).
 * @param name Name of the device.
 * @param init_fn Device init function.
 * @param flags Device flags.
 * @param pm Reference to @ref pm_device_base associated with the device.
 * (optional).
 * @param data Reference to device data.
 * @param config Reference to device config.
 * @param level Initialization level.
 * @param prio Initialization priority.
 * @param api Reference to device API.
 * @param state Reference to device state.
 * @param ... Optional dependencies, manually specified.
 */
#define Z_DEVICE_DEFINE(node_id, dev_id, name, init_fn, deinit_fn, flags, pm,  \
			data, config, level, prio, api, state, ...)      \
	Z_DEVICE_NAME_CHECK(name);      \
\
	IF_ENABLED(CONFIG_DEVICE_DEPS,      \
		   (Z_DEVICE_DEPS_DEFINE(node_id, dev_id, __VA_ARGS__);))       \
\
	IF_ENABLED(CONFIG_DEVICE_DT_METADATA,      \
		   (IF_ENABLED(DT_NODE_EXISTS(node_id),      \
			      (Z_DEVICE_DT_METADATA_DEFINE(node_id, dev_id);))))      \
\
	Z_DEVICE_BASE_DEFINE(node_id, dev_id, name, init_fn, deinit_fn, flags,      \
			     pm, data, config, level, prio, api, state,         \
			     Z_DEVICE_DEPS_NAME(dev_id));      \
\
	Z_DEVICE_INIT_ENTRY_DEFINE(node_id, dev_id, level, prio);      \
\
	IF_ENABLED(CONFIG_LLEXT,      \
		   (IF_ENABLED(DT_NODE_EXISTS(node_id),      \
				(Z_DEVICE_EXPORT(node_id);))))

```

