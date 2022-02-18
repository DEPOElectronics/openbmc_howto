### Ручное управление
Информацию о линиях GPIO можно Вручную можно управлять линиями GPIO с помощью команд `gpioget`. В случае выходной линии задать состояние с помощью `gpioset`. Получить информацию можно с помощью команды `gpioinfo`

## Переименование линий gpio

Для переименования линий gpio требуется в [DeviceTree](dev_tree.md) включить следующий блок

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