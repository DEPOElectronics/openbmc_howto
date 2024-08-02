# Установка своего Device Tree
Замена ядра на своё нужно тогда, когда к плате статично подключено оборудование, отличающееся от стандартного.
В качестве основы беру AST2500-evb

1.	Заменить `KERNEL_DEVICETREE =` в файле `conf/machine/[system].conf` на свой файл .dtb
2.	Добавить dts-файл по пути `recipes-kernel/linux/linux-aspeed/[name].dts`
3.	Добавить в файл `recipes-kernel/linux/linux-aspeed_%.bbappend` 

```
FILESEXTRAPATHS:prepend := "${THISDIR}/${PN}:"

SRC_URI += " \
             file://aspeed-bmc-[name].dts \
           "

# Merge source tree by original project with our layer of additional files

do_patch:append () {
    WD=${WORKDIR}/../oe-local-files/
    [ -d ${WD} ] || WD=${WORKDIR}
    cp -r ${WD}/${KMACHINE}-bmc-depo-${MACHINE}.dts \
   	  ${WD}/*.dtsi \
          ${STAGING_KERNEL_DIR}/arch/arm/boot/dts
}
```

## Включаемые файлы в DeviceTree

Зачастую удобно хранить DeviceTree не одним большим файлом, а в нескольких разделенных логически файлах. Для этого в главном файле .dts используется директива `#include "NewFile.dtsi"`.
Так же новый файл необходимо добавить в `SRC_URI` файла `recipes-kernel/linux/linux-aspeed_%.bbappend`

## Подключение i2c RTC
Пример:
```
&i2c2 {
	/* i2c_pmbus */
	status = "okay";

	rtc@6f {
		compatible = "microchip,mcp7941x";
		reg = <0x6f>;
		interrupts = <110>;
	};
};
```

## [Подключение консоли UART](console)


# Удалить eth1

Из DevTree удалить блок

```
&mac1 {
       status = "okay";

       pinctrl-names = "default";
       pinctrl-0 = <&pinctrl_rgmii2_default &pinctrl_mdio2_default>;
};

```
