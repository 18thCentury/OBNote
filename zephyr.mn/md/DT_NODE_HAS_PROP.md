```c
/**
 * @brief Does a devicetree node have a property?
 *
 * Tests whether a devicetree node has a property defined.
 *
 * This tests whether the property is defined at all, not whether a
 * boolean property is true or false. To get a boolean property's
 * truth value, use DT_PROP(node_id, prop) instead.
 *
 * @param node_id node identifier
 * @param prop lowercase-and-underscores property name
 * @return 1 if the node has the property, 0 otherwise.
 */
#define DT_NODE_HAS_PROP(node_id, prop) \
	IS_ENABLED(DT_CAT4(node_id, _P_, prop, _EXISTS))

```


build\zephyr\include\generated\zephyr\devicetree_generated.h
