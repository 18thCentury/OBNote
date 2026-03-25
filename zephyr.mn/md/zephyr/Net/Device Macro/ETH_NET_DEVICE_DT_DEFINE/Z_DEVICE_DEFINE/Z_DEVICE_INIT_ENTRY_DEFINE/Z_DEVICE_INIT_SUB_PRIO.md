```c
/**
 * @brief Init sub-priority of the device
 *
 * The sub-priority is defined by the devicetree ordinal, which ensures that
 * multiple drivers running at the same priority level run in an order that
 * respects the devicetree dependencies.
 */
#define Z_DEVICE_INIT_SUB_PRIO(node_id)                                        \
	COND_CODE_1(DT_NODE_EXISTS(node_id),                                   \
		    (DT_DEP_ORD_STR_SORTABLE(node_id)), (0))

```

