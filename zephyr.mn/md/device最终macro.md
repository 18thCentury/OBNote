```
__aligned(__alignof(struct deivce)) struct deivce  __device_dts_ord_102 __attribute__((section("._device.static.3_80_"))) = 
{										
		.name = "w5500@0",								
		.config = (&w5500_0_config),							
		.api = (&w5500_api_funcs),								
		.state = (&__devstate_##dev_id),							
		.data = (&w5500_0_runtime),							
		.ops = {.init = (w5500_init),.deinit = (deinit_fn_),},
		.flags = (flags_),							
		.deps = (deps_), /**/			
		IF_ENABLED(CONFIG_PM_DEVICE, Z_DEVICE_INIT_PM_BASE(pm_)) /**/		
		IF_ENABLED(CONFIG_DEVICE_DT_METADATA,					
			   (IF_ENABLED(DT_NODE_EXISTS(node_id_),			
				       (.dt_meta = &Z_DEVICE_DT_METADATA_NAME_GET(	
						dev_id_),))))				\
	}


static const __aligned(__alignof(struct init_entry)) struct init_entry  __used __noasan __attribute__((__section__(".z_init_POST_KERNEL_P_80_SUB_00102_"))) __init___device_dts_ord_102 = {
 .init_fn = NULL,
 .dev =  (const struct device *) & __device_dts_ord_102,
 }
 


```