```c
/**
 * @brief Init entry section.
 *
 * Each init entry is placed in a section with a name crafted so that it allows
 * linker scripts to sort them according to the specified
 * level/priority/sub-priority.
 */
#define Z_INIT_ENTRY_SECTION(level, prio, sub_prio)      \
	__attribute__((__section__(      \
		".z_init_" #level "_P_" STRINGIFY(prio) "_SUB_" STRINGIFY(sub_prio)"_")))

```