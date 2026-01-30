```c
/**
 * @brief Get a node identifier for an instance of a compatible
 *
 * All nodes with a particular compatible property value are assigned
 * instance numbers, which are zero-based indexes specific to that
 * compatible. You can get a node identifier for these nodes by
 * passing DT_INST() an instance number, @p inst, along with the
 * lowercase-and-underscores version of the compatible, @p compat.
 *
 * Instance numbers have the following properties:
 *
 * - for each compatible, instance numbers start at 0 and are contiguous
 * - exactly one instance number is assigned for each node with a compatible,
 *   **including disabled nodes**
 * - enabled nodes (status property is `okay` or missing) are assigned the
 *   instance numbers starting from 0, and disabled nodes have instance
 *   numbers which are greater than those of any enabled node
 *
 * No other guarantees are made. In particular:
 *
 * - instance numbers **in no way reflect** any numbering scheme that
 *   might exist in SoC documentation, node labels or unit addresses,
 *   or properties of the /aliases node (use DT_NODELABEL() or DT_ALIAS()
 *   for those)
 * - there **is no general guarantee** that the same node will have
 *   the same instance number between builds, even if you are building
 *   the same application again in the same build directory
 *
 * Example devicetree fragment:
 *
 * @code{.dts}
 *     serial1: serial@40001000 {
 *             compatible = "vnd,soc-serial";
 *             status = "disabled";
 *             current-speed = <9600>;
 *             ...
 *     };
 *
 *     serial2: serial@40002000 {
 *             compatible = "vnd,soc-serial";
 *             status = "okay";
 *             current-speed = <57600>;
 *             ...
 *     };
 *
 *     serial3: serial@40003000 {
 *             compatible = "vnd,soc-serial";
 *             current-speed = <115200>;
 *             ...
 *     };
 * @endcode
 *
 * Assuming no other nodes in the devicetree have compatible
 * `"vnd,soc-serial"`, that compatible has nodes with instance numbers
 * 0, 1, and 2.
 *
 * The nodes `serial@40002000` and `serial@40003000` are both enabled, so
 * their instance numbers are 0 and 1, but no guarantees are made
 * regarding which node has which instance number.
 *
 * Since `serial@40001000` is the only disabled node, it has instance
 * number 2, since disabled nodes are assigned the largest instance
 * numbers. Therefore:
 *
 * @code{.c}
 *     // Could be 57600 or 115200. There is no way to be sure:
 *     // either serial@40002000 or serial@40003000 could
 *     // have instance number 0, so this could be the current-speed
 *     // property of either of those nodes.
 *     DT_PROP(DT_INST(0, vnd_soc_serial), current_speed)
 *
 *     // Could be 57600 or 115200, for the same reason.
 *     // If the above expression expands to 57600, then
 *     // this expands to 115200, and vice-versa.
 *     DT_PROP(DT_INST(1, vnd_soc_serial), current_speed)
 *
 *     // 9600, because there is only one disabled node, and
 *     // disabled nodes are "at the end" of the instance
 *     // number "list".
 *     DT_PROP(DT_INST(2, vnd_soc_serial), current_speed)
 * @endcode
 *
 * Notice how `"vnd,soc-serial"` in the devicetree becomes `vnd_soc_serial`
 * (without quotes) in the DT_INST() arguments. (As usual, `current-speed`
 * in the devicetree becomes `current_speed` as well.)
 *
 * Nodes whose `compatible` property has multiple values are assigned
 * independent instance numbers for each compatible.
 *
 * @param inst instance number for compatible @p compat
 * @param compat lowercase-and-underscores compatible, without quotes
 * @return node identifier for the node with that instance number and
 *         compatible
 */
#define DT_INST(inst, compat) UTIL_CAT(DT_N_INST, DT_DASH(inst, compat))

```

