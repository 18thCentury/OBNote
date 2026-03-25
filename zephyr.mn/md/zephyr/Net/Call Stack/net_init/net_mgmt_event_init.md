```c
void net_mgmt_event_init(void)
{
	mgmt_rebuild_global_event_mask();

#if defined(CONFIG_NET_MGMT_EVENT_THREAD)
	#if defined(CONFIG_NET_MGMT_THREAD_PRIO_CUSTOM)
		#define THREAD_PRIORITY CONFIG_NET_MGMT_THREAD_PRIORITY
	#elif defined(CONFIG_NET_TC_THREAD_COOPERATIVE)
		/* Lowest priority cooperative thread */
		#define THREAD_PRIORITY K_PRIO_COOP(CONFIG_NUM_COOP_PRIORITIES - 1)
	#else
		#define THREAD_PRIORITY K_PRIO_PREEMPT(CONFIG_NUM_PREEMPT_PRIORITIES - 1)
	#endif
	struct k_work_queue_config q_cfg = {
		.name = "net_mgmt",
		.no_yield = false,
	};

	k_work_queue_init(&mgmt_work_q_obj);
	k_work_queue_start(&mgmt_work_q_obj, mgmt_stack,
			   K_KERNEL_STACK_SIZEOF(mgmt_stack),
			   THREAD_PRIORITY, &q_cfg);

	NET_DBG("Net MGMT initialized: queue of %u entries, stack size of %u",
		CONFIG_NET_MGMT_EVENT_QUEUE_SIZE,
		CONFIG_NET_MGMT_EVENT_STACK_SIZE);
#endif /* CONFIG_NET_MGMT_EVENT_THREAD */
}

```


`CONFIG_NET_MGMT_EVENT_THREAD` :
