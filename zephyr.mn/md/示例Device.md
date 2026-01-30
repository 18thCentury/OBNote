基础变量:
## [[Z_DEVICE_DEFINE]]

| node_id   | DT_N_S_soc_S_spi_40013000_S_w5500_0 |
| --------- | ----------------------------------- |
| dev_id    | dts_ord_102                         |
| name      | "w5500@0"                           |
| init_fn   | w5500_init                          |
| deinit_fn | NULL                                |
| flags     | Z_DEVICE_DT_FLAGS(node_id)          |
| pm        | NULL                                |
| data      | &w5500_0_runtime                    |
| config    | &w5500_0_config                     |
| level     | POST_KERNEL                         |
| prio      | CONFIG_ETH_INIT_PRIORITY            |
| api       | &w5500_api_funcs                    |
| state     | &Z_DEVICE_STATE_NAME(dev_id)        |

```
#define Z_DEVICE_DEFINE(node_id, dev_id, name, init_fn, deinit_fn, flags, pm,  \
			data, config, level, prio, api, state, ...)      \
	Z_DEVICE_NAME_CHECK(name);      \
\
	IF_ENABLED(CONFIG_DEVICE_DEPS,      \
		   (Z_DEVICE_DEPS_DEFINE(node_id, dev_id, __VA_ARGS__);))       \
\
	IF_ENABLED(CONFIG_DEVICE_DT_METADATA,      \
		   (IF_ENABLED(DT_NODE_EXISTS(node_id),      \
			      (Z_DEVICE_DT_METADATA_DEFINE(node_id, dev_id);))))      \
\
	Z_DEVICE_BASE_DEFINE(node_id, dev_id, name, init_fn, deinit_fn, flags,      \
			     pm, data, config, level, prio, api, state,         \
			     Z_DEVICE_DEPS_NAME(dev_id));      \
\
	Z_DEVICE_INIT_ENTRY_DEFINE(node_id, dev_id, level, prio);      \
\
	IF_ENABLED(CONFIG_LLEXT,      \
		   (IF_ENABLED(DT_NODE_EXISTS(node_id),      \
				(Z_DEVICE_EXPORT(node_id);))))

```

---
#### [[Z_DEVICE_NAME_CHECK]]

---
#### [[Z_DEVICE_DEPS_DEFINE]]

---
#### Z_DEVICE_DT_METADATA_DEFINE

---

#### [[Z_DEVICE_BASE_DEFINE]]


```c
Z_DEVICE_BASE_DEFINE(node_id, dev_id, name, init_fn, deinit_fn, flags,      \
			     pm, data, config, level, prio, api, state,         \
			     Z_DEVICE_DEPS_NAME(dev_id)); 


#define Z_DEVICE_BASE_DEFINE(node_id, dev_id, name, init_fn, deinit_fn, flags, pm, data, config, level, prio, api, state, deps)    \
	COND_CODE_1(DT_NODE_EXISTS(node_id), (), (static))    \
	COND_CODE_1(Z_DEVICE_IS_MUTABLE(node_id), (), (const))    \
	STRUCT_SECTION_ITERABLE_NAMED_ALTERNATE(    \
		device, COND_CODE_1(Z_DEVICE_IS_MUTABLE(node_id), (device_mutable), (device)),     \
		Z_DEVICE_SECTION_NAME(level, prio), DEVICE_NAME_GET(dev_id)) =    \
		Z_DEVICE_INIT(name, init_fn, deinit_fn, flags, pm, data, config, api, state, deps, node_id, dev_id)
```

Z_DEVICE_BASE_DEFINE 基础变量:

| node_id   | DT_N_S_soc_S_spi_40013000_S_w5500_0 |     |
| --------- | ----------------------------------- | --- |
| dev_id    | dts_ord_102                         |     |
| name      | "w5500@0"                           |     |
| init_fn   | w5500_init                          |     |
| deinit_fn | NULL                                |     |
| flags     | Z_DEVICE_DT_FLAGS(node_id)          |     |
| pm        | NULL                                |     |
| data      | &w5500_0_runtime                    |     |
| config    | &w5500_0_config                     |     |
| level     | POST_KERNEL                         |     |
| prio      | CONFIG_ETH_INIT_PRIORITY            | 80  |
| api       | &w5500_api_funcs                    |     |
| state     | &Z_DEVICE_STATE_NAME(dev_id)        |     |
| deps      | Z_DEVICE_DEPS_NAME(dev_id)          |     |

```
CONFIG_ETH_INIT_PRIORITY=80
```

---
######  1. [[STRUCT_SECTION_ITERABLE_NAMED_ALTERNATE]]
```c
#define STRUCT_SECTION_ITERABLE_NAMED_ALTERNATE(struct_type, secname, name, varname) \
	TYPE_SECTION_ITERABLE(struct struct_type, varname, secname, name)
```

```c
STRUCT_SECTION_ITERABLE_NAMED_ALTERNATE(    \
		device, COND_CODE_1(Z_DEVICE_IS_MUTABLE(node_id), (device_mutable), (device)), Z_DEVICE_SECTION_NAME(level, prio), DEVICE_NAME_GET(dev_id))

```

STRUCT_SECTION_ITERABLE_NAMED_ALTERNATE 变量定义:

| struct_type | device                                                                |                     |
| ----------- | --------------------------------------------------------------------- | ------------------- |
| secname     | COND_CODE_1(Z_DEVICE_IS_MUTABLE(node_id), (device_mutable), (device)) | device              |
| name        | [[Z_DEVICE_SECTION_NAME]] (level, prio)                               | _3_80               |
| varname     | [[DEVICE_NAME_GET]](dev_id)                                           | \_\_device_##dev_id |


---
1.1 [[TYPE_SECTION_ITERABLE]]
 [[Z_DECL_ALIGN]] / 
```
#define TYPE_SECTION_ITERABLE(type, varname, secname, section_postfix) \
	Z_DECL_ALIGN(type) varname \
	__in_section(_##secname, static, _CONCAT(section_postfix, _)) __used __noasan
	
#define ___in_section(a, b, c) \
	__attribute__((section("." Z_STRINGIFY(a)			\
				"." Z_STRINGIFY(b)			\
				"." Z_STRINGIFY(c))))
#define __in_section(a, b, c) ___in_section(a, b, c)
```
TYPE_SECTION_ITERABLE 变量定义:

| type            | struct deivce       |
| --------------- | ------------------- |
| varname         | \_\_device_##dev_id |
| secname         | device              |
| section_postfix | 3\_80_              |




```c
=> TYPE_SECTION_ITERABLE(struct struct_type, varname, secname, name)
=> a: _device ,b: static , c:3_80
```
==>
```c

__aligned(__alignof(struct deivce)) struct deivce  __device_dts_ord_102 __attribute__((section("._device.static.3_80_")))

```

> 查看:
> [build\zephyr\zephyr.map](E:\Project\zephyr\zephyrproject\application\APPS\build\zephyr\zephyr.map)
```
 ._device.static.3_80_
                0x00000000080279bc       0x1c zephyr/drivers/ethernet/libdrivers__ethernet.a(eth_w5500.c.obj)
                0x00000000080279bc                __device_dts_ord_102
```


---
 ###### 2. [[Z_DEVICE_INIT]]
 ```c
 #define Z_DEVICE_INIT(name_, init_fn_, deinit_fn_, flags_, pm_, data_, config_, api_, state_, deps_, node_id_, dev_id_)					\
	{										\
		.name = name_,								\
		.config = (config_),							\
		.api = (api_),								\
		.state = (state_),							\
		.data = (data_),							\
		.ops = Z_DEVICE_OPS(init_fn_, deinit_fn_),				\
		.flags = (flags_),							\
		IF_ENABLED(CONFIG_DEVICE_DEPS, (.deps = (deps_),)) /**/			\
		IF_ENABLED(CONFIG_PM_DEVICE, Z_DEVICE_INIT_PM_BASE(pm_)) /**/		\
		IF_ENABLED(CONFIG_DEVICE_DT_METADATA,					\
			   (IF_ENABLED(DT_NODE_EXISTS(node_id_),			\
				       (.dt_meta = &Z_DEVICE_DT_METADATA_NAME_GET(	\
						dev_id_),))))				\
	}
 ```



| node_id   | DT_N_S_soc_S_spi_40013000_S_w5500_0 |                        |
| --------- | ----------------------------------- | ---------------------- |
| dev_id    | dts_ord_102                         |                        |
| name      | "w5500@0"                           |                        |
| init_fn   | w5500_init                          |                        |
| deinit_fn | NULL                                |                        |
| flags     | Z_DEVICE_DT_FLAGS(node_id)          |                        |
| pm        | NULL                                |                        |
| data      | &w5500_0_runtime                    |                        |
| config    | &w5500_0_config                     |                        |
| level     | POST_KERNEL                         |                        |
| prio      | CONFIG_ETH_INIT_PRIORITY            |                        |
| api       | &w5500_api_funcs                    |                        |
| state     | &[[Z_DEVICE_STATE_NAME]](dev_id)    | &\_\_devstate_##dev_id |
| deps      | [[Z_DEVICE_DEPS_NAME]](dev_id)      |                        |
```c
Z_DEVICE_INIT(name, init_fn, deinit_fn, flags, pm, data, config, api, state, deps, node_id, dev_id)
```

Z_DEVICE_INIT 的变量

| name_       | "w5500@0"                           |
| ----------- | ----------------------------------- |
| init\_fn\_  | w5500_init                          |
| deinit\_fn_ | NULL                                |
| flags_      |                                     |
| pm_         | NULL                                |
| data_       | &w5500_0_runtime                    |
| config_     | &w5500_0_config                     |
| api_        | &w5500_api_funcs                    |
| state_      | &\_\_devstate_##dev_id              |
| deps_       |                                     |
| node\_id_   | DT_N_S_soc_S_spi_40013000_S_w5500_0 |
| dev\_id_    | dts_ord_102                         |

```c
{										
		.name = "w5500@0",								
		.config = (&w5500_0_config),							
		.api = (&w5500_api_funcs),								
		.state = (&__devstate_##dev_id),							
		.data = (&w5500_0_runtime),							
		.ops = {.init = (w5500_init),.deinit = (deinit_fn_),},
		.flags = (flags_),							
		IF_ENABLED(CONFIG_DEVICE_DEPS, (.deps = (deps_),)) /**/			
		IF_ENABLED(CONFIG_PM_DEVICE, Z_DEVICE_INIT_PM_BASE(pm_)) /**/		
		IF_ENABLED(CONFIG_DEVICE_DT_METADATA,					
			   (IF_ENABLED(DT_NODE_EXISTS(node_id_),			
				       (.dt_meta = &Z_DEVICE_DT_METADATA_NAME_GET(	
						dev_id_),))))				\
	}

```

---
#### [[Z_DEVICE_INIT_ENTRY_DEFINE]]

```
#define Z_DEVICE_INIT_ENTRY_DEFINE(node_id, dev_id, level, prio)         \
	Z_DEVICE_CHECK_INIT_LEVEL(level)         \
	static const Z_DECL_ALIGN(struct init_entry) __used __noasan Z_INIT_ENTRY_SECTION(\
		level, prio, Z_DEVICE_INIT_SUB_PRIO(node_id))         \
		Z_INIT_ENTRY_NAME(DEVICE_NAME_GET(dev_id)) = {         \
			.init_fn = NULL,         \
			.dev = (const struct device *)&DEVICE_NAME_GET(dev_id),         \
		}

```
Z_DEVICE_INIT_ENTRY_DEFINE 变量:

| node_id | DT_N_S_soc_S_spi_40013000_S_w5500_0 |     |
| ------- | ----------------------------------- | --- |
| dev_id  | dts_ord_102                         |     |
| level   | POST_KERNEL                         |     |
| prio    | CONFIG_ETH_INIT_PRIORITY            | 80  |

[[Z_DEVICE_CHECK_INIT_LEVEL]] / [[Z_DECL_ALIGN]] / 

[[Z_INIT_ENTRY_SECTION]] / [[Z_DEVICE_INIT_SUB_PRIO]]
```
__attribute__((__section__(      \
		".z_init_" #level "_P_" STRINGIFY(prio) "_SUB_" STRINGIFY(sub_prio)"_")))
```
=>
```
 .z_init_POST_KERNEL_P_80_SUB_00102_
```

[[Z_INIT_ENTRY_NAME]] / [[DEVICE_NAME_GET]]

```
Z_INIT_ENTRY_NAME(DEVICE_NAME_GET(dev_id))
=>
__init___device_##dev_id_
```

```c
static const __aligned(__alignof(struct init_entry)) struct init_entry  __used __noasan __attribute__((__section__(".z_init_POST_KERNEL_P_80_SUB_00102_"))) __init___device_dts_ord_102 = {
 .init_fn = NULL,
 .dev =  (const struct device *) & __device_dts_ord_102,
 }
 
```




