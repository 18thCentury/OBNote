```c

static int net_init(void)
{
	net_hostname_init(); // do nothing

	NET_DBG("Priority %d", CONFIG_NET_INIT_PRIO);

	net_pkt_init(); // do nothing

	net_context_init(); // k_sem_init(&contexts_lock, 1, K_SEM_MAX_LIMIT);

	l3_init();

	net_mgmt_event_init();

	init_rx_queues();

	return services_init();
}

```