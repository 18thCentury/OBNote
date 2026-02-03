```c
/**
 * @brief Defines a new element of alternate data type for an iterable section.
 *
 * @details
 * Special variant of STRUCT_SECTION_ITERABLE(), for placing alternate
 * data types within the iterable section of a specific data type. The
 * data type sizes and semantics must be equivalent!
 */
#define STRUCT_SECTION_ITERABLE_ALTERNATE(secname, struct_type, varname) \
	TYPE_SECTION_ITERABLE(struct struct_type, varname, secname, varname)

```

[[TYPE_SECTION_ITERABLE]]
