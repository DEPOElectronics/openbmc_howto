## Добавление LED в DeviceTree

1.	Добавить `#include <dt-bindings/gpio/aspeed-gpio.h>` в [DevTree](dev_tree.md)
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