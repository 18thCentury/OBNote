During initialization, call :c:func:`net_eth_mac_load` before registering
the address with the network interface. The helper copies any statically provided
bytes, fills the remaining octets, and performs the necessary validation.
Drivers can still fall back to SoC-specific storage when no configuration was provided:

它的目的是解决“网卡的物理地址从哪儿来”的问题。在嵌入式开发中，MAC 地址可能来自设备树（DTS）、固件随机生成、或是从 SoC 的唯一 ID（UID）转换而来。

net_eth_mac_load 是一个辅助函数，它帮你规范了这一获取过程。

1. 它的核心逻辑
当你调用 net_eth_mac_load 时，Zephyr 会按以下优先级确定 MAC 地址：
设备树 (DTS) 指定：如果你在设备树的节点中写了 local-mac-address = [xx xx xx xx xx xx];，它会优先读取这个值。
运行时分配/随机化：如果 DTS 中没有指定，该函数会自动为你填充剩下的字节。通常它会保留厂商前缀（OUI），然后根据 SoC 的唯一标识符（UID）哈希出一个固定的后缀。
验证：它会自动检查该 MAC 是否合法（例如是否为多播地址）。

2. 为什么要用它，而不是自己写死？
用 net_eth_mac_load 有三个好处：
- 唯一性：由于它通常结合 SoC 的 UID 生成后缀，即使你批量生产 1000 台设备，每台设备虽然跑同样的固件，但生成的 MAC 地址是各自唯一的，不会造成局域网冲突。
- 一致性：Zephyr 的网络管理层（net_mgmt）和 Shell 指令（net iface）看到的地址与硬件地址是统一的。
- 灵活性：如果以后你想从外部 EEPROM 读取 MAC，或者通过 Kconfig 随机生成，你只需要修改配置，而不需要改驱动代码。