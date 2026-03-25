```
/* This macro synthesizes a unique dev_id from a devicetree node by using
 * the node's dependency ordinal.
 *
 * The ordinal used in this name can be mapped to the path by
 * examining zephyr/include/generated/zephyr/devicetree_generated.h.
 */
#define Z_DEVICE_DT_DEP_ORD(node_id) _CONCAT(dts_ord_, DT_DEP_ORD(node_id))

```