```
/**
 * @brief Get a node's dependency ordinal
 * @param node_id Node identifier
 * @return the node's dependency ordinal as an integer literal
 */
#define DT_DEP_ORD(node_id) DT_CAT(node_id, _ORD)

```