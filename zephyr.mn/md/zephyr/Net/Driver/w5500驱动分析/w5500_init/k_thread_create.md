```
k_thread_create(&ctx->thread, ctx->thread_stack,
            CONFIG_ETH_W5500_RX_THREAD_STACK_SIZE,
            w5500_thread,
            (void *)dev, NULL, NULL,
            K_PRIO_COOP(CONFIG_ETH_W5500_RX_THREAD_PRIO),
            0, K_NO_WAIT);
    k_thread_name_set(&ctx->thread, "eth_w5500");
```