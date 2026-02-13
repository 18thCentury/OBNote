```c
/**
 * @brief Underlying network device has detected the carrier (cable connected).
 *
 * @details The function should be used by the respective network device driver
 *          or L2 implementation to update its state on a network interface.
 *
 * @param iface Pointer to network interface
 */
void net_if_carrier_on(struct net_if *iface)
{
	if (iface == NULL) {
		return;
	}

	net_if_lock(iface);

	if (!net_if_flag_test_and_set(iface, NET_IF_LOWER_UP)) {
		update_operational_state(iface);
	}

	net_if_unlock(iface);
}
```