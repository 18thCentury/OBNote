```
/**
 * @brief Synthesize a unique name for the device state associated with
 * @p dev_id.
 */
#define Z_DEVICE_STATE_NAME(dev_id) _CONCAT(__devstate_, dev_id)

```