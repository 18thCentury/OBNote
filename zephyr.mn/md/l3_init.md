```c
static inline void l3_init(void)
{
	net_pmtu_init();
	net_icmpv4_init();
	net_icmpv6_init();
	net_ipv4_init();
	net_ipv6_init();

	net_ipv4_autoconf_init();

	if (IS_ENABLED(CONFIG_NET_UDP) ||
	    IS_ENABLED(CONFIG_NET_TCP) ||
	    IS_ENABLED(CONFIG_NET_SOCKETS_PACKET) ||
	    IS_ENABLED(CONFIG_NET_SOCKETS_CAN)) {
		net_conn_init();
	}

	net_tcp_init();

	net_route_init();

	NET_DBG("Network L3 init done");
}
```