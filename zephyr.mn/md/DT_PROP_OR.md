```
#define DT_PROP_OR(node_id, prop, default_value) \
	COND_CODE_1(DT_NODE_HAS_PROP(node_id, prop), \
		    (DT_PROP(node_id, prop)), (default_value))
```

[[DT_NODE_HAS_PROP]]
[[DT_PROP]]
