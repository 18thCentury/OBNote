遍历 `_net_if__list_start` 和 `_net_if_list_end`


```c
/**
 * @brief Iterate over a specified iterable section.
 *
 * @details
 * Iterator for structure instances gathered by STRUCT_SECTION_ITERABLE().
 * The linker must provide a _<struct_type>_list_start symbol and a
 * _<struct_type>_list_end symbol to mark the start and the end of the
 * list of struct objects to iterate over. This is normally done using
 * ITERABLE_SECTION_ROM() or ITERABLE_SECTION_RAM() in the linker script.
 */
#define STRUCT_SECTION_FOREACH(struct_type, iterator) \
	STRUCT_SECTION_FOREACH_ALTERNATE(struct_type, struct_type, iterator)
```


[E:\Project\zephyr\zephyrproject\zephyr\include\zephyr\sys\iterable_sections.h](E:\Project\zephyr\zephyrproject\zephyr\include\zephyr\sys\iterable_sections.h)
