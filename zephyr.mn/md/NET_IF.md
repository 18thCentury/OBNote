

```c
net_if_area     0x0000000020003030      0x120 load address 0x0000000008030b20
                0x0000000020003030                _net_if_list_start = .
 *(SORT_BY_NAME(SORT_BY_ALIGNMENT(._net_if.static.*)))
 ._net_if.static.dts_ord_102
                0x0000000020003030       0x90 zephyr/drivers/ethernet/libdrivers__ethernet.a(eth_w5500.c.obj)
 ._net_if.static.dts_ord_76
                0x00000000200030c0       0x90 zephyr/drivers/ethernet/libdrivers__ethernet.a(eth_stm32_hal_common.c.obj)
                0x0000000020003150                _net_if_list_end = .
```

注意到:
1.  Section 是定义在 ram 中
2. 有 list_start 和 list_end

在哪里被使用?
