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

## GPIO

### Ручное управление
Информацию о линиях GPIO можно Вручную можно управлять линиями GPIO с помощью команд `gpioget`. В случае выходной линии задать состояние с помощью `gpioset`. Получить информацию можно с помощью команды `gpioinfo`

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

## Добавление LED в DeviceTree

1.	Добавить `#include <dt-bindings/gpio/aspeed-gpio.h>` в DevTree
2.	Добавить раздел leds в секцию `/ {..};`
Пример:

```
leds {
	compatible = "gpio-leds";
	bmc_boot {
		label = "front_id";
		gpios = <&gpio ASPEED_GPIO(G, 1) GPIO_ACTIVE_HIGH>;
		linux,default-trigger = "timer";
	};
};
```
После того как LED добавлен в ядро. Доступ через программы управления GPIO до него недоступен, но зато этот светодиод появляется по адресу `/sys/class/leds/`.

*	Посмотреть текущее состояние `cat brightness`
*	Зажечь светодиод `echo 1 > brightness`
*	Погасить светодиод `echo 0 > brightness`
*	Возможные типы мигания `cat trigger`
*	Задать тип мигания `echo heartbeat > trigger`

## Добавление стандартных LED

Управлением LED занимается phosphor-led-manager. Для того чтобы заработали стандартные светодиоды типа ID и fault достаточно назвать их в соответствии с [Name](https://github.com/openbmc/phosphor-led-manager/blob/master/example/led-group-config.json) в конфиге. Либо поменять конфиг.

## Добавление LED со своим именем
Есть вариант добавления с помощью переопределения менеджера phosphor-led-manager и использования файла LED.yaml. Либо включения use-json в файле .bbappend

recipes-phosphor/leds/phosphor-led-manager_%.bbappend

```
FILESEXTRAPATHS:prepend := "${THISDIR}/${PN}:"

SRC_URI += "file://led-group-config.json"

PACKAGECONFIG:append = " use-json use-lamp-test"

do_install:append() {
        install -m 0644 ${WORKDIR}/led-group-config.json ${D}${datadir}/phosphor-led-manager/
}
```

recipes-phosphor/leds/phosphor-led-manager/led-group-config.json

```
{
    "leds": [
        {
            "group": "enclosure_identify",
            "members": [
                {
                    "Name": "identify",
                    "Action": "Blink",
                    "DutyOn": 50,
                    "Period": 1000
                }
            ]
        }
    ]
}
```

## Добавление GPIO кнопки 

### Добавление кнопки в DeviceTree
1.	Добавить `#include <dt-bindings/gpio/aspeed-gpio.h>` в DevTree
2.	Добавить раздел gpio-keys в секцию `/ {..};`
Пример:

```
gpio-keys {
		compatible = "gpio-keys";

		id-button {
			label = "id-button";
			gpios = <&gpio ASPEED_GPIO(N, 3) GPIO_ACTIVE_HIGH>;
			linux,code = <ASPEED_GPIO(N, 3)>;
		};

	};

```
После того как кнопка добавлена в дерево устройств, она начинает писать информацию по адресу `cat /dev/input/by-path/platform-gpio-keys-event`.

### Добавление реакции на кнопку

По адресу `recipes-phosphor/gpio` создать сервис id-button (название по-смыслу), который будет реагировать на нажатие этой кнопки. Создать `recipes-phosphor/packagegroups/packagegroup-obmc-apps.bbappend` 

```
RDEPENDS:${PN}-inventory:append:dazn = " id-button"
```

# Конфигурация

Для добавления поддержкой различных поддерживаемых датчиков надо

1.	Создать файл `recipes-kernel/linux/linux-aspeed/{MACHINE}.cfg`
2.	В файл записать нужный конфиг. Например `CONFIG_PMBUS=y` 
3.	Добавить этот файл в `linux-aspeed_%.bbappend` уровнем выше в раздел `SRC_URI`

# I2C

## Добавление линии в DevTree
В .dts файл добавить нужную линию

```
&i2c2 {
	status = "okay";
};

```

## Блок питания

1.	Добавить в ядро нужную линию I2c
2.	В конфигурацию добавить 

# Управление вентилятором

# Добавить web-интерфейс

В минимальный образ уже добавлен Web-интерфейс. Для этого, в образ системы `/recipes-phosphor/images/obmc-phosphor-image.bbappend` в `OBMC_IMAGE_EXTRA_INSTALL:append` добавлена строка `webui-vue`