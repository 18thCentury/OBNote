```c

/**
 * @brief Get a devicetree property value
 *
 * For properties whose bindings have the following types, this macro
 * expands to:
 *
 * - string: a string literal
 * - boolean: `0` if the property is false, or `1` if it is true
 * - int: the property's value as an integer literal
 * - array, uint8-array, string-array: an initializer expression in braces,
 *   whose elements are integer or string literals (like `{0, 1, 2}`,
 *   `{"hello", "world"}`, etc.)
 * - phandle: a node identifier for the node with that phandle
 *
 * A property's type is usually defined by its binding. In some
 * special cases, it has an assumed type defined by the devicetree
 * specification even when no binding is available: `compatible` has
 * type string-array, `status` has type string, and
 * `interrupt-controller` has type boolean.
 *
 * For other properties or properties with unknown type due to a
 * missing binding, behavior is undefined.
 *
 * For usage examples, see DT_PATH(), DT_ALIAS(), DT_NODELABEL(),
 * and DT_INST() above.
 *
 * @param node_id node identifier
 * @param prop lowercase-and-underscores property name
 * @return a representation of the property's value
 */
#define DT_PROP(node_id, prop) DT_CAT3(node_id, _P_, prop)


```