Для отображения посткодов через gpio я использую свой [драйвер](http://gitlab-niokr.depo.local/niokr/program/bmc/post-display)
Для его использования надо в [ядре](../kernel) включить загрузку модулей (т.к по-умолчанию этот драйвер загружаю как модуль) а в [дереве устройств](../dev_tree) указать используемые линии gpio. [Например](http://gitlab-niokr.depo.local/niokr/dpc621-bv/openbmc/-/blob/master/meta-depo/meta-dpc621-bv/recipes-kernel/linux/linux-aspeed/aspeed-dpc621-bv.dts#L108):
```
	7seg {
		compatible = "post_disp";
		data-gpios = <&sgpio 1 GPIO_ACTIVE_HIGH>,
                         <&sgpio 3 GPIO_ACTIVE_HIGH>,
                         <&sgpio 5 GPIO_ACTIVE_HIGH>,
                         <&sgpio 7 GPIO_ACTIVE_HIGH>,
                         <&sgpio 9 GPIO_ACTIVE_HIGH>,
                         <&sgpio 11 GPIO_ACTIVE_HIGH>,
                         <&sgpio 13 GPIO_ACTIVE_HIGH>,
                         <&sgpio 15 GPIO_ACTIVE_HIGH>;
	};

```