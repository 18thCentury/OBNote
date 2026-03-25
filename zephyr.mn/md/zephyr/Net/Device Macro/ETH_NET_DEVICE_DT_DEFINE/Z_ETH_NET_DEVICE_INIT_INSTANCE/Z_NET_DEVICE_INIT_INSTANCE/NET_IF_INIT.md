```ruby
#define NET_IF_INIT(dev_id, sfx, _l2, _mtu, _num_configs)		\
	static STRUCT_SECTION_ITERABLE(net_if_dev, NET_IF_DEV_GET_NAME(dev_id, sfx)) = 	\
	{	.dev = &(DEVICE_NAME_GET(dev_id)),			\
		.l2 = &(NET_L2_GET_NAME(_l2)),				\
		.l2_data = &(NET_L2_GET_DATA(dev_id, sfx)),		\
		.mtu = _mtu,						\
		.flags = {BIT(NET_IF_LOWER_UP)},			\
	};								\
	static Z_DECL_ALIGN(struct net_if)				\
		       NET_IF_GET_NAME(dev_id, sfx)[_num_configs]	\
		       __used __noasan __in_section(_net_if, static, dev_id) =\
	{			\
		[0 ... (_num_configs - 1)] = {				\
			.if_dev = &(NET_IF_DEV_GET_NAME(dev_id, sfx)),	\
			NET_IF_CONFIG_INIT				\
		}							\
	};								\
	IF_ENABLED(CONFIG_NET_STATISTICS_VIA_PROMETHEUS,		\
		   (static PROMETHEUS_COLLECTOR_DEFINE(			\
			   NET_PROMETHEUS_GET_COLLECTOR_NAME(dev_id,sfx),	\
			   net_stats_prometheus_scrape,			\
			   NET_IF_GET(dev_id, sfx));			\
		    NET_STATS_PROMETHEUS(NET_IF_GET(dev_id, sfx), dev_id, sfx);))

```


---


![[NET_L2_GET_NAME]]

![[NET_IF_DEV_GET_NAME]]

![[NET_IF_GET_NAME]]
![[NET_L2_GET_DATA]]

![[___in_section]]

[[NET_IF_CONFIG_INIT]]

[[DEVICE_NAME_GET]]

[[Z_DECL_ALIGN]]
