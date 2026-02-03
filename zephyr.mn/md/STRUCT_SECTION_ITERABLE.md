```c
/**
 * @brief Defines a new element for an iterable section.
 *
 * @details
 * Convenience helper combining __in_section() and Z_DECL_ALIGN().
 * The section name is the struct type prepended with an underscore.
 * The subsection is "static" and the subsubsection is the variable name.
 *
 * In the linker script, create output sections for these using
 * ITERABLE_SECTION_ROM() or ITERABLE_SECTION_RAM().
 *
 * @note In order to store the element in ROM, a const specifier has to
 * be added to the declaration: const STRUCT_SECTION_ITERABLE(...);
 */
#define STRUCT_SECTION_ITERABLE(struct_type, varname) \
	STRUCT_SECTION_ITERABLE_ALTERNATE(struct_type, struct_type, varname)

```