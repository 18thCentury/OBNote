```
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