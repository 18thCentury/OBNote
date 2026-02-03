```c
//#define dev_id dts_ord_102
// 定义 dev_state
__aligned(__alignof(struct device_state)) struct device_state __devstate_##dev_id __attribute__((__section__(".z_devstate")));

// 定义 device
__aligned(__alignof(struct deivce)) struct deivce  __device_##dev_id __attribute__((section("._device.static.3_80_"))) = 
{
		.name = "w5500@0",
		.config = (&w5500_0_config),
		.api = (&w5500_api_funcs),
		.state = (&__devstate_##dev_id),// 这里引用.z_devstate
		.data = (&w5500_0_runtime),
		.ops = {.init = (w5500_init),.deinit = (deinit_fn_),},
		.flags = (flags_),
		.deps = (deps_), /**/
		IF_ENABLED(CONFIG_PM_DEVICE, Z_DEVICE_INIT_PM_BASE(pm_)) /**/
		IF_ENABLED(CONFIG_DEVICE_DT_METADATA,
			   (IF_ENABLED(DT_NODE_EXISTS(node_id_),
				       (.dt_meta = &Z_DEVICE_DT_METADATA_NAME_GET(
						dev_id_),))))
	}

// 定义 入口
static const __aligned(__alignof(struct init_entry)) struct init_entry  __used __noasan __attribute__((__section__(".z_init_POST_KERNEL_P_80_SUB_00102_"))) __init___device_dts_ord_102 = 
{
 .init_fn = NULL,
 .dev =  (const struct device *) & __device_##dev_id,
 }
 
// 定义 l2 data
static struct ethernet_context _net_l2_data_##dev_id##instance __used;

// 定义 net_if_dev
__aligned(__alignof(struct net_if_dev)) struct net_if_dev  __net_if_dev_##dev_id##_instance __attribute__((section(".net_if_dev.static.__net_if_dev_##dev_id##_instance"))) __used __noasan = 
{	
	.dev = &(DEVICE_NAME_GET(dev_id)),          //__device_##dev_id
	.l2 = &(NET_L2_GET_NAME(_l2)),              // _net_l2_##ETHERNET_L2
	.l2_data = &(NET_L2_GET_DATA(dev_id, sfx)), //& _net_l2_data_##dev_id##0
	.mtu = _mtu,                                //NET_ETH_MTU
	.flags = {BIT(NET_IF_LOWER_UP)},
};

// 定义 net_if
__aligned(__alignof(struct net_if)) struct net_if __net_if_##dev_id##_instance [NET_IF_MAX_CONFIGS]  __used __noasan __attribute__((section("._net_if.static.##dev_id))) =
{			\
	[0 ... (NET_IF_MAX_CONFIGS - 1)] = {		
		.if_dev = &(NET_IF_DEV_GET_NAME(dev_id, sfx)),	// &__net_if_dev_##dev_id##0
		.config = {					\
			.ip = {}, \
			NET_IF_DHCPV4_INIT			\.dhcpv4.state = NET_DHCPV4_DISABLED
			NET_IF_DHCPV6_INIT			\.dhcpv6.state = NET_DHCPV6_DISABLED,
		}
	}							
};

```


最终流程:

net_if 来管理 net_if_dev 
net_if_dev 管理  device , l2