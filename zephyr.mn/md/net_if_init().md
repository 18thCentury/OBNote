```c
void net_if_init(void)
{
	int if_count = 0;

	NET_DBG("");

	k_mutex_lock(&lock, K_FOREVER);

	net_tc_tx_init();

	STRUCT_SECTION_FOREACH(net_if, iface) {
#if defined(CONFIG_NET_INTERFACE_NAME)
		memset(net_if_get_config(iface)->name, 0,
		       sizeof(iface->config.name));
#endif

		init_iface(iface);

#if defined(CONFIG_NET_INTERFACE_NAME)
		/* If the driver did not set the name, then set
		 * a default name for the network interface.
		 */
		if (net_if_get_config(iface)->name[0] == '\0') {
			set_default_name(iface);
		}
#endif

		net_stats_prometheus_init(iface);

		if_count++;
	}

	if (if_count == 0) {
		NET_ERR("There is no network interface to work with!");
		goto out;
	}

#if defined(CONFIG_ASSERT)
	/* Do extra check that verifies that interface count is properly
	 * done.
	 */
	int count_if;

	NET_IFACE_COUNT(&count_if);
	NET_ASSERT(count_if == if_count);
#endif

	iface_ipv6_init(if_count);
	iface_ipv4_init(if_count);
	iface_router_init();

#if defined(CONFIG_NET_PKT_TIMESTAMP_THREAD)
	k_thread_create(&tx_thread_ts, tx_ts_stack,
			K_KERNEL_STACK_SIZEOF(tx_ts_stack),
			net_tx_ts_thread,
			NULL, NULL, NULL, K_PRIO_COOP(1), 0, K_NO_WAIT);
	k_thread_name_set(&tx_thread_ts, "tx_tstamp");
#endif /* CONFIG_NET_PKT_TIMESTAMP_THREAD */

out:
	k_mutex_unlock(&lock);
}
```



---
```c
/**
 * @brief Count the number of network interfaces.
 *
 * @param[out] _dst Pointer to location where result is written.
 */
#define NET_IFACE_COUNT(_dst) \
		do {							\
			extern struct net_if _net_if_list_start[];	\
			extern struct net_if _net_if_list_end[];	\
			*(_dst) = ((uintptr_t)_net_if_list_end -	\
				   (uintptr_t)_net_if_list_start) /	\
				sizeof(struct net_if);			\
		} while (0)

```