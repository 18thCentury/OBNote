```c
/**
 * @brief Get a devicetree node's name with unit-address as a string literal
 *
 * This returns the node name and unit-address from a node identifier.
 *
 * Example devicetree fragment:
 *
 * @code{.dts}
 *     / {
 *             soc {
 *                     node: my-node@12345678 { ... };
 *             };
 *     };
 * @endcode
 *
 * Example usage:
 *
 * @code{.c}
 *    DT_NODE_FULL_NAME(DT_NODELABEL(node)) // "my-node@12345678"
 * @endcode
 *
 * @param node_id node identifier
 * @return the node's name with unit-address as a string in the devicetree
 */
#define DT_NODE_FULL_NAME(node_id) DT_CAT(node_id, _FULL_NAME)
```