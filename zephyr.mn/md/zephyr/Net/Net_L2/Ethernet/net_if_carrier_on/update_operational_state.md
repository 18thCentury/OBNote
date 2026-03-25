```c
static void update_operational_state(struct net_if *iface)
{
	enum net_if_oper_state prev_state = iface->if_dev->oper_state;
	enum net_if_oper_state new_state = NET_IF_OPER_UNKNOWN;

	if (!net_if_is_admin_up(iface)) {
		new_state = NET_IF_OPER_DOWN;
		goto exit;
	}

	if (!device_is_ready(net_if_get_device(iface))) {
		new_state = NET_IF_OPER_LOWERLAYERDOWN;
		goto exit;
	}

	if (!net_if_is_carrier_ok(iface)) {
#if defined(CONFIG_NET_L2_VIRTUAL)
		if (net_if_l2(iface) == &NET_L2_GET_NAME(VIRTUAL)) {
			new_state = NET_IF_OPER_LOWERLAYERDOWN;
		} else
#endif /* CONFIG_NET_L2_VIRTUAL */
		{
			new_state = NET_IF_OPER_DOWN;
		}

		goto exit;
	}

	if (net_if_is_dormant(iface)) {
		new_state = NET_IF_OPER_DORMANT;
		goto exit;
	}

	new_state = NET_IF_OPER_UP;

exit:
	if (net_if_oper_state_set(iface, new_state) != new_state) {
		NET_ERR("Failed to update oper state to %d", new_state);
		return;
	}

	NET_DBG("iface %d (%p), oper state %s admin %s carrier %s dormant %s",
		net_if_get_by_iface(iface), iface,
		net_if_oper_state2str(net_if_oper_state(iface)),
		net_if_is_admin_up(iface) ? "UP" : "DOWN",
		net_if_is_carrier_ok(iface) ? "ON" : "OFF",
		net_if_is_dormant(iface) ? "ON" : "OFF");

	if (net_if_oper_state(iface) == NET_IF_OPER_UP) {
		if (prev_state != NET_IF_OPER_UP) {
			notify_iface_up(iface);
		}
	} else {
		if (prev_state == NET_IF_OPER_UP) {
			notify_iface_down(iface);
		}
	}
}
```