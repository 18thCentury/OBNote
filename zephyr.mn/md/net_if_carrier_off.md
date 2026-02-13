```c
/**
 * @brief Underlying network device has lost the carrier (cable disconnected).
 *
 * @details The function should be used by the respective network device driver
 *          or L2 implementation to update its state on a network interface.
 *
 * @param iface Pointer to network interface
 */
void net_if_carrier_off(struct net_if *iface)
{
	if (iface == NULL) {
		return;
	}

	net_if_lock(iface);

	if (net_if_flag_test_and_clear(iface, NET_IF_LOWER_UP)) {
		update_operational_state(iface);
	}

	net_if_unlock(iface);
}
```

对比 [[net_eth_carrier_off]]:
