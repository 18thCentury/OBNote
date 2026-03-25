```c
static inline void init_iface(struct net_if *iface)
{
	const struct net_if_api *api = net_if_get_device(iface)->api;

	if (!api || !api->init) {
		NET_ERR("Iface %p driver API init NULL", iface);
		return;
	}

	/* By default IPv4 and IPv6 are enabled for a given network interface.
	 * These can be turned off later if needed.
	 */
#if defined(CONFIG_NET_NATIVE_IPV4)
	net_if_flag_set(iface, NET_IF_IPV4);
#endif
#if defined(CONFIG_NET_NATIVE_IPV6)
	net_if_flag_set(iface, NET_IF_IPV6);
#endif

	net_virtual_init(iface);

	NET_DBG("On iface %d", net_if_get_by_iface(iface));

#ifdef CONFIG_USERSPACE
	k_object_init(iface);
#endif

	k_mutex_init(&iface->lock);
	k_mutex_init(&iface->tx_lock);

	api->init(iface);

	net_ipv6_pe_init(iface);
}
```
