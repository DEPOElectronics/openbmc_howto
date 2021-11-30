# Начальный образ

В качестве начального образа взят минимальный ast-2500 [образ](https://github.com/gluhow/openbmc/tree/minimum/meta-sample/meta-ast2500-min)

*	Ast2500
*	64Мб flash
*	meta-bytedance/g220a DevTree 
*	u-boot 2019.04
*	static mtd


Создание образа под свою плату

1.	Скопировать  meta-depo/meta-min
2.	сonf
	1.	`bblayers.conf.sample` переименовать слой
    2.	`BBFILE_COLLECTIONS += "mylayer"`
    3.	`BBFILE_PATTERN_dazn-layer = "^${LAYERDIR}/"`
    4.	`LAYERSERIES_COMPAT_dazn-layer = "honister"`
3. local.conf.sample переименовать MACHINE    

# Включение/выключение

Управление выключением/выключением осуществляется по GPIO.

# Установка своего Device Tree

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

## Переименование линий gpio

Для переименования линий gpio требуется в DeviceTree включить следующий блок

```
&gpio {
	status = "okay";
	gpio-line-names =
	/*A0-A7*/	"nameA0","nameA1","nameA2","etc","","","","",
	/*B0-B7*/	"","","","","","","","",
	/*C0-C7*/	"","","","","","","","",
	/*D0-D7*/	"","","","","","","","",
	/*E0-E7*/	"","","","","","","","",
	/*F0-F7*/	"","","","","","","","",
	/*G0-G7*/	"","","","","","","","",
	/*H0-H7*/	"","","","","","","","",
	/*I0-I7*/	"","","","","","","","",
	/*J0-J7*/	"","","","","","","","",
	/*K0-K7*/	"","","","","","","","",
	/*L0-L7*/	"","","","","","","","",
	/*M0-M7*/	"","","","","","","","",
	/*N0-N7*/	"","","","","","","","",
	/*O0-O7*/	"","","","","","","","",
	/*P0-P7*/	"","","","","","","","",
	/*Q0-Q7*/	"","","","","","","","",
	/*R0-R7*/	"","","","","","","","",
	/*S0-S7*/	"","","","","","","","",
	/*T0-T7*/	"","","","","","","","",
	/*U0-U7*/	"","","","","","","","",
	/*V0-V7*/	"","","","","","","","",
	/*W0-W7*/	"","","","","","","","",
	/*X0-X7*/	"","","","","","","","",
	/*Y0-Y7*/	"","","","","","","","",
	/*Z0-Z7*/	"","","","","","","","",
	/*AA0-AA7*/	"","","","","","","","",
	/*AB0-AB7*/	"","","","","","","","",
	/*AC0-AC7*/	"","","","","","","","nameAC7";
};

```



# Конфигурация

Для добавления поддержкой различных поддерживаемых датчиков надо

1.	Создать файл `recipes-kernel/linux/linux-aspeed/{MACHINE}.cfg`
2.	В файл записать нужный конфиг. Например `CONFIG_PMBUS=y` 
3.	Добавить этот файл в `linux-aspeed_%.bbappend` уровнем выше в раздел `SRC_URI`

# Управление вентилятором

# Добавить web-интерфейс

В минимальный образ уже добавлен Web-интерфейс. Для этого, в образ системы `/recipes-phosphor/images/obmc-phosphor-image.bbappend` в `OBMC_IMAGE_EXTRA_INSTALL:append` добавлена строка `webui-vue`