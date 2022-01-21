Для того чтобы заработал IKVM в браузере в DevTree ядра в раздел `reserved-memory` должны быть добавлены разделы (возможно не все, надо тестировать):

*	vga\_memory
*	flash\_memory
*	coldfire\_memory
*	video\_engine\_memory

Например:

```
	reserved-memory {
		#address-cells = <1>;
		#size-cells = <1>;
		ranges;

		vga_memory: framebuffer@9f000000 {
			no-map;
			reg = <0x9f000000 0x01000000>; /* 16M */
		};

		flash_memory: region@98000000 {
			no-map;
			reg = <0x98000000 0x04000000>; /* 64M */
		};

		coldfire_memory: codefire_memory@9ef00000 {
			reg = <0x9ef00000 0x00100000>;
			no-map;
		};

		gfx_memory: framebuffer {
			size = <0x01000000>;
			alignment = <0x01000000>;
			compatible = "shared-dma-pool";
			reusable;
		};

		video_engine_memory: jpegbuffer {
			size = <0x02000000>;	/* 32M */
			alignment = <0x01000000>;
			compatible = "shared-dma-pool";
			reusable;
		};
	};

```
