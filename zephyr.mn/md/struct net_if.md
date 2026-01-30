```c
/**
 * @brief Network Interface structure
 *
 * Used to handle a network interface on top of a net_if_dev instance.
 * There can be many net_if instance against the same net_if_dev instance.
 *
 */
struct net_if {
	/** The net_if_dev instance the net_if is related to */
	struct net_if_dev *if_dev;

#if defined(CONFIG_NET_STATISTICS_PER_INTERFACE)
	/** Network statistics related to this network interface */
	struct net_stats stats;

	/** Promethus collector for this network interface */
	IF_ENABLED(CONFIG_NET_STATISTICS_VIA_PROMETHEUS,
		   (struct prometheus_collector *collector);)
#endif /* CONFIG_NET_STATISTICS_PER_INTERFACE */

	/** Network interface instance configuration */
	struct net_if_config config;

#if defined(CONFIG_NET_POWER_MANAGEMENT)
	/** Keep track of packets pending in traffic queues. This is
	 * needed to avoid putting network device driver to sleep if
	 * there are packets waiting to be sent.
	 */
	int tx_pending;
#endif

	/** Mutex protecting this network interface instance */
	struct k_mutex lock;

	/** Mutex used when sending data */
	struct k_mutex tx_lock;

	/** Network interface specific flags */
	/** Enable IPv6 privacy extension (RFC 8981), this is enabled
	 * by default if PE support is enabled in configuration.
	 */
	uint8_t pe_enabled : 1;

	/** If PE is enabled, then this tells whether public addresses
	 * are preferred over temporary ones for this interface.
	 */
	uint8_t pe_prefer_public : 1;

	/** Unused bit flags (ignore) */
	uint8_t _unused : 6;
};
```