```c
/**
 * @brief Inform ethernet L2 driver that ethernet carrier is detected.
 * This happens when cable is connected.
 *
 * @param iface Network interface
 */
 
void net_eth_carrier_on(struct net_if *iface)
{
	struct ethernet_context *ctx = net_if_l2_data(iface);

	if (!atomic_test_and_set_bit(&ctx->flags, ETH_CARRIER_UP)) {
		k_work_submit(&ctx->carrier_work);
	}
}
```

[[carrier_on_off]]
