```c
void ethernet_init(struct net_if *iface)
{
	struct ethernet_context *ctx = net_if_l2_data(iface);

	NET_DBG("Initializing Ethernet L2 %p for iface %d (%p)", ctx,
		net_if_get_by_iface(iface), iface);

#if defined(CONFIG_NET_DSA) && !defined(CONFIG_NET_DSA_DEPRECATED)
	/* DSA port may need to handle flags */
	dsa_eth_init(iface);
#endif

	if (IS_ENABLED(CONFIG_ETH_NET_IF_NO_AUTO_START)) {
		/* Do not start Ethernet interface automatically */
		net_if_flag_set(iface, NET_IF_NO_AUTO_START);
	}

	ctx->ethernet_l2_flags = NET_L2_MULTICAST;
	ctx->iface = iface;
	k_work_init(&ctx->carrier_work, carrier_on_off);

	if (net_eth_get_hw_capabilities(iface) & ETHERNET_PROMISC_MODE) {
		ctx->ethernet_l2_flags |= NET_L2_PROMISC_MODE;
	}

#if defined(CONFIG_NET_NATIVE_IP) && !defined(CONFIG_NET_RAW_MODE)
	if (net_eth_get_hw_capabilities(iface) & ETHERNET_HW_FILTERING) {
		net_if_mcast_mon_register(&mcast_monitor, NULL, ethernet_mcast_monitor_cb);
	}
#endif

	net_arp_init();

	ctx->is_init = true;
}
```


重点:
	k_work_init(&ctx->carrier_work, carrier_on_off);