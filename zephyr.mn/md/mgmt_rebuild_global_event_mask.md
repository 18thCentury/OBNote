```c
static inline void mgmt_rebuild_global_event_mask(void)
{
	struct net_mgmt_event_callback *cb, *tmp;

	global_event_mask = 0U;

	STRUCT_SECTION_FOREACH(net_mgmt_event_static_handler, it) {
		mgmt_add_event_mask(it->event_mask);
	}

	SYS_SLIST_FOR_EACH_CONTAINER_SAFE(&event_callbacks, cb, tmp, node) {
		mgmt_add_event_mask(cb->event_mask);
	}
}
```
