```c
/**
 * @brief Like ETH_NET_DEVICE_DT_DEFINE for an instance of a DT_DRV_COMPAT
 * compatible
 *
 * @param inst instance number.  This is replaced by
 * <tt>DT_DRV_COMPAT(inst)</tt> in the call to ETH_NET_DEVICE_DT_DEFINE.
 *
 * @param ... other parameters as expected by ETH_NET_DEVICE_DT_DEFINE.
 */
#define ETH_NET_DEVICE_DT_INST_DEFINE(inst, ...) \
	ETH_NET_DEVICE_DT_DEFINE(DT_DRV_INST(inst), __VA_ARGS__)
```

```c
#define DT_DRV_COMPAT wch_ch395_spi
node_id : DT_DRV_INST(inst)


/**
 * @brief Node identifier for an instance of a `DT_DRV_COMPAT` compatible
 * @param inst instance number
 * @return a node identifier for the node with `DT_DRV_COMPAT` compatible and
 *         instance number @p inst
 */
#define DT_DRV_INST(inst) DT_INST(inst, DT_DRV_COMPAT)


```

```c
/**
 * @brief Like ETH_NET_DEVICE_INIT but taking metadata from a devicetree.
 * Create an Ethernet network interface and bind it to network device.
 *
 * @param node_id The devicetree node identifier.
 * @param init_fn Address to the init function of the driver.
 * @param pm Reference to struct pm_device associated with the device.
 * (optional).
 * @param data Pointer to the device's private data.
 * @param config The address to the structure containing the
 * configuration information for this instance of the driver.
 * @param prio The initialization level at which configuration occurs.
 * @param api Provides an initial pointer to the API function struct
 * used by the driver. Can be NULL.
 * @param mtu Maximum transfer unit in bytes for this network interface.
 */
#define ETH_NET_DEVICE_DT_DEFINE(node_id, init_fn, pm, data, config,	\
				 prio, api, mtu)			\
	Z_ETH_NET_DEVICE_INIT(node_id, Z_DEVICE_DT_DEV_ID(node_id),	\
			      DEVICE_DT_NAME(node_id), init_fn, pm,	\
			      data, config, prio, api, mtu)
```

```c
#define Z_ETH_NET_DEVICE_INIT(node_id, dev_id, name, init_fn, pm, data,	\
			      config, prio, api, mtu)			\
	Z_ETH_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, 0,	\
				       init_fn, pm, data, config, prio,	\
				       api, mtu)
```

```c
#if !defined(CONFIG_ETH_DRIVER_RAW_MODE)

#define Z_ETH_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, instance,	\
				       init_fn, pm, data, config, prio,	\
				       api, mtu)			\
	Z_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, instance,	\
				   init_fn, pm, data, config, prio,	\
				   api, ETHERNET_L2,			\
				   NET_L2_GET_CTX_TYPE(ETHERNET_L2), mtu)

#else /* CONFIG_ETH_DRIVER_RAW_MODE */

#define Z_ETH_NET_DEVICE_INIT_INSTANCE(node_id, dev_id, name, instance,	\
				       init_fn, pm, data, config, prio,	\
				       api, mtu)			\
	Z_DEVICE_STATE_DEFINE(dev_id);					\
	Z_DEVICE_DEFINE(node_id, dev_id, name, init_fn, NULL,		\
			Z_DEVICE_DT_FLAGS(node_id), pm, data,		\
			config, POST_KERNEL, prio, api,			\
			&Z_DEVICE_STATE_NAME(dev_id));

#endif /* CONFIG_ETH_DRIVER_RAW_MODE */
```

CONFIG_ETH_DRIVER_RAW_MODE:
> 通常情况下，当 Zephyr 以太网驱动程序接收到一个帧时，它会将其封装在 `net_pkt` 结构中，并移交给 **L2（以太网层）**，随后传递给 **L3（IP 层）**。这个过程涉及大量的解析、IP 首部检查和路由操作。
> 当启用 **`CONFIG_ETH_DRIVER_RAW_MODE`** 时，驱动程序会被指令**绕过**大部分标准处理逻辑。它不再将以太网帧视为 IP 流量的载体，而是将其视为 **“原始” 载荷（Raw Payload）** 。




```c
/* By default, device identifiers are obtained using the dependency ordinal.
 * When LLEXT_EXPORT_DEV_IDS_BY_HASH is defined, the main Zephyr binary exports
 * DT identifiers via EXPORT_GROUP_SYMBOL_NAMED as hashed versions of their
 * paths. When matching extensions are built, that is what they need to look
 * for.
 *
 * The ordinal or hash used in this name can be mapped to the path by
 * examining zephyr/include/generated/zephyr/devicetree_generated.h.
 */
#if defined(LL_EXTENSION_BUILD) && defined(CONFIG_LLEXT_EXPORT_DEV_IDS_BY_HASH)
#define Z_DEVICE_DT_DEV_ID(node_id) Z_DEVICE_DT_HASH(node_id)
#else
#define Z_DEVICE_DT_DEV_ID(node_id) Z_DEVICE_DT_DEP_ORD(node_id)
#endif
```

```c
/* This macro synthesizes a unique dev_id from a devicetree node by using
 * the node's dependency ordinal.
 *
 * The ordinal used in this name can be mapped to the path by
 * examining zephyr/include/generated/zephyr/devicetree_generated.h.
 */
#define Z_DEVICE_DT_DEP_ORD(node_id) _CONCAT(dts_ord_, DT_DEP_ORD(node_id))

```

```c
/**
 * @brief Get a node's dependency ordinal
 * @param node_id Node identifier
 * @return the node's dependency ordinal as an integer literal
 */
#define DT_DEP_ORD(node_id) DT_CAT(node_id, _ORD)

```


最终的deviceid : `dts_ord_0_ORD`