```c
static void init_rx_queues(void)
{
	net_tc_rx_init();

	/* Starting TX side. The ordering is important here and the TX
	 * can only be started when RX side is ready to receive packets.
	 */
	net_if_init();

	/* This will take the interface up and start everything. */
	net_if_post_init();

	/* Things to init after network interface is working */
	net_post_init();
}
```


