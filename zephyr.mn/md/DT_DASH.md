```c
/** @brief Helper for node identifier macros to expand args */
#define DT_DASH(...) MACRO_MAP_CAT(DT_DASH_PREFIX, __VA_ARGS__)
/** @brief Helper for DT_DASH(): prepends _ to a name */
#define DT_DASH_PREFIX(name) _##name


```

解释:
依次调用 DT_DASH_PREFIX (name)

E:\Project\zephyr\zephyrproject\zephyr\include\zephyr\sys\util_internal.h