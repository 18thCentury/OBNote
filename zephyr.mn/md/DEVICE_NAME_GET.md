```c
/**
 * @brief Expands to the name of a global device object.
 *
 * Return the full name of a device object symbol created by DEVICE_DEFINE(),
 * using the `dev_id` provided to DEVICE_DEFINE(). This is the name of the
 * global variable storing the device structure, not a pointer to the string in
 * the @ref device.name field.
 *
 * It is meant to be used for declaring extern symbols pointing to device
 * objects before using the DEVICE_GET macro to get the device object.
 *
 * This macro is normally only useful within device driver source code. In other
 * situations, you are probably looking for device_get_binding().
 *
 * @param dev_id Device identifier.
 *
 * @return The full name of the device object defined by device definition
 * macros.
 */
#define DEVICE_NAME_GET(dev_id) _CONCAT(__device_, dev_id)

