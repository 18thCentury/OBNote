
```c

/**
 * @brief Return a string name for a devicetree node.
 *
 * This macro returns a string literal usable as a device's name from a
 * devicetree node identifier.
 *
 * @param node_id The devicetree node identifier.
 *
 * @return The value of the node's `label` property, if it has one.
 * Otherwise, the node's full name in `node-name@unit-address` form.
 */
#define DEVICE_DT_NAME(node_id)                                                \
	DT_PROP_OR(node_id, label, DT_NODE_FULL_NAME(node_id))
```