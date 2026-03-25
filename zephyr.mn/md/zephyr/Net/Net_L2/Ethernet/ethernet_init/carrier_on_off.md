```c
static void carrier_on_off(struct k_work *work)
{
	struct ethernet_context *ctx = CONTAINER_OF(work, struct ethernet_context,
						    carrier_work);
	bool eth_carrier_up;

	if (ctx->iface == NULL) {
		return;
	}

	eth_carrier_up = atomic_test_bit(&ctx->flags, ETH_CARRIER_UP);

	if (eth_carrier_up == ctx->is_net_carrier_up) {
		return;
	}

	ctx->is_net_carrier_up = eth_carrier_up;

	NET_DBG("Carrier %s for interface %p", eth_carrier_up ? "ON" : "OFF",
		ctx->iface);

	if (eth_carrier_up) {
		ethernet_mgmt_raise_carrier_on_event(ctx->iface);
		net_if_carrier_on(ctx->iface);
	} else {
		ethernet_mgmt_raise_carrier_off_event(ctx->iface);
		net_if_carrier_off(ctx->iface);
	}
}

```

[[carrier_on_off]]
