```c
/**
 * @brief Device section name (used for sorting purposes).
 *
 * @param level Initialization level
 * @param prio Initialization priority
 */
#define Z_DEVICE_SECTION_NAME(level, prio)                                     \
	_CONCAT(INIT_LEVEL_ORD(level), _##prio)

```