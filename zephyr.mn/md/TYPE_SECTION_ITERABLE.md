```c
/**
 * @brief Defines a new element for an iterable section for a generic type.
 *
 * @details
 * Convenience helper combining __in_section() and Z_DECL_ALIGN().
 * The section name will be '.[SECNAME].static.[SECTION_POSTFIX]'
 *
 * In the linker script, create output sections for these using
 * ITERABLE_SECTION_ROM() or ITERABLE_SECTION_RAM().
 *
 * @note In order to store the element in ROM, a const specifier has to
 * be added to the declaration: const TYPE_SECTION_ITERABLE(...);
 *
 * @param[in]  type data type of variable
 * @param[in]  varname name of variable to place in section
 * @param[in]  secname type name of iterable section.
 * @param[in]  section_postfix postfix to use in section name
 */
#define TYPE_SECTION_ITERABLE(type, varname, secname, section_postfix) \
	Z_DECL_ALIGN(type) varname \
	__in_section(_##secname, static, _CONCAT(section_postfix, _)) __used __noasan
```


```c
#define ___in_section(a, b, c) \
	__attribute__((section("." Z_STRINGIFY(a)			\
				"." Z_STRINGIFY(b)			\
				"." Z_STRINGIFY(c))))
#define __in_section(a, b, c) ___in_section(a, b, c)
```