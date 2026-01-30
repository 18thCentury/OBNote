```c
/**
 * @brief Get an network interface's device
 *
 * @param iface Pointer to a network interface structure
 *
 * @return a pointer to the device driver instance
 */
static inline const struct device *net_if_get_device(struct net_if *iface)
{
	if (iface == NULL || iface->if_dev == NULL) {
		return NULL;
	}
	return iface->if_dev->dev;
}

```

#### dev 如何传递到 iface
