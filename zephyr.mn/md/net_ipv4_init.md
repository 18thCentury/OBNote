```c
void net_ipv4_init(void)
{
	if (IS_ENABLED(CONFIG_NET_IPV4_FRAGMENT)) {
		net_ipv4_setup_fragment_buffers();
	}

	if (IS_ENABLED(CONFIG_NET_IPV4_ACD)) {
		net_ipv4_acd_init();
	}
}
```