
使用 `net_if_flag_set`函数

```c
/** Network interface flags. */
enum net_if_flag {
	/** Interface is admin up. */
	NET_IF_UP,

	/** Interface is pointopoint */
	NET_IF_POINTOPOINT,

	/** Interface is in promiscuous mode */
	NET_IF_PROMISC,

	/** Do not start the interface immediately after initialization.
	 * This requires that either the device driver or some other entity
	 * will need to manually take the interface up when needed.
	 * For example for Ethernet this will happen when the driver calls
	 * the net_eth_carrier_on() function.
	 */
	NET_IF_NO_AUTO_START,

	/** Power management specific: interface is being suspended */
	NET_IF_SUSPENDED,

	/** Flag defines if received multicasts of other interface are
	 * forwarded on this interface. This activates multicast
	 * routing / forwarding for this interface.
	 */
	NET_IF_FORWARD_MULTICASTS,

	/** Interface supports IPv4 */
	NET_IF_IPV4,

	/** Interface supports IPv6 */
	NET_IF_IPV6,

	/** Interface up and running (ready to receive and transmit). */
	NET_IF_RUNNING,

	/** Driver signals L1 is up. */
	NET_IF_LOWER_UP,

	/** Driver signals dormant. */
	NET_IF_DORMANT,

	/** IPv6 Neighbor Discovery disabled. */
	NET_IF_IPV6_NO_ND,

	/** IPv6 Multicast Listener Discovery disabled. */
	NET_IF_IPV6_NO_MLD,

	/** Mutex locking on TX data path disabled on the interface. */
	NET_IF_NO_TX_LOCK,

/** @cond INTERNAL_HIDDEN */
	/* Total number of flags - must be at the end of the enum */
	NET_IF_NUM_FLAGS
/** @endcond */
};
```


