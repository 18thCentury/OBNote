```c
/**
 * @brief Network Interface Device structure
 *
 * Used to handle a network interface on top of a device driver instance.
 * There can be many net_if_dev instance against the same device.
 *
 * Such interface is mainly to be used by the link layer, but is also tight
 * to a network context: it then makes the relation with a network context
 * and the network device.
 *
 * Because of the strong relationship between a device driver and such
 * network interface, each net_if_dev should be instantiated by one of the
 * network device init macros found in net_if.h.
 */
struct net_if_dev {
	/** The actually device driver instance the net_if is related to */
	const struct device *dev;

	/** Interface's L2 layer */
	const struct net_l2 * const l2;

	/** Interface's private L2 data pointer */
	void *l2_data;

	/** For internal use */
	ATOMIC_DEFINE(flags, NET_IF_NUM_FLAGS);

	/** The hardware link address */
	struct net_linkaddr link_addr;

#if defined(CONFIG_NET_OFFLOAD)
	/** TCP/IP Offload functions.
	 * If non-NULL, then the TCP/IP stack is located
	 * in the communication chip that is accessed via this
	 * network interface.
	 */
	struct net_offload *offload;
#endif /* CONFIG_NET_OFFLOAD */

	/** The hardware MTU */
	uint16_t mtu;

#if defined(CONFIG_NET_SOCKETS_OFFLOAD)
	/** A function pointer to create an offloaded socket.
	 *  If non-NULL, the interface is considered offloaded at socket level.
	 */
	net_socket_create_t socket_offload;
#endif /* CONFIG_NET_SOCKETS_OFFLOAD */

	/** RFC 2863 operational status */
	enum net_if_oper_state oper_state;

	/** Last time the operational state was changed.
	 * This is used to determine how long the interface has been in the
	 * current operational state.
	 *
	 * This value is set to 0 when the interface is created, and then
	 * updated whenever the operational state changes.
	 *
	 * The value is in milliseconds since boot.
	 */
	int64_t oper_state_change_time;
};
```