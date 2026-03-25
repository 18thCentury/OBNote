```c
/**
 * @brief Defines a new element for an iterable section with a custom name,
 * placed in a custom section.
 *
 * The name can be used to customize how iterable section entries are sorted.
 * @see STRUCT_SECTION_ITERABLE_NAMED()
 */
#define STRUCT_SECTION_ITERABLE_NAMED_ALTERNATE(struct_type, secname, name, varname) \
	TYPE_SECTION_ITERABLE(struct struct_type, varname, secname, name)
```