
```
/**
 * @brief Initializer for @ref device.
 *
 * @param name_ Name of the device.
 * @param init_fn_ Init function (optional).
 * @param deinit_fn_ De-init function (optional).
 * @param flags_ Device flags.
 * @param pm_ Reference to @ref pm_device_base (optional).
 * @param data_ Reference to device data.
 * @param config_ Reference to device config.
 * @param api_ Reference to device API ops.
 * @param state_ Reference to device state.
 * @param deps_ Reference to device dependencies.
 * @param node_id_ Devicetree node identifier
 * @param dev_id_ Device identifier token, as passed to Z_DEVICE_BASE_DEFINE
 */
#define Z_DEVICE_INIT(name_, init_fn_, deinit_fn_, flags_, pm_, data_, config_, api_, state_, deps_, node_id_, dev_id_)					\
	{										\
		.name = name_,								\
		.config = (config_),							\
		.api = (api_),								\
		.state = (state_),							\
		.data = (data_),							\
		.ops = Z_DEVICE_OPS(init_fn_, deinit_fn_),				\
		.flags = (flags_),							\
		IF_ENABLED(CONFIG_DEVICE_DEPS, (.deps = (deps_),)) /**/			\
		IF_ENABLED(CONFIG_PM_DEVICE, Z_DEVICE_INIT_PM_BASE(pm_)) /**/		\
		IF_ENABLED(CONFIG_DEVICE_DT_METADATA,					\
			   (IF_ENABLED(DT_NODE_EXISTS(node_id_),			\
				       (.dt_meta = &Z_DEVICE_DT_METADATA_NAME_GET(	\
						dev_id_),))))				\
	}

```
