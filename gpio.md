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
После того как кнопка добавлена в дерево устройств, она начинает писать информацию по адресу `/dev/input/by-path/platform-gpio-keys-event`.
Почему-то начались проблемы с platform-gpio-keys-event. И **[phosphor-gpio-monitor](https://github.com/openbmc/phosphor-gpio-monitor)**  в режиме работы с отдельными сервисами. В итоге без добавления gpio-keys секции. Использую [phosphor-gpio-monitor](phosphor-gpio-monitor) в multi режиме

## Ручное управление GPIO
При необходимости вручную управлять gpio, можно воспользоваться sysfs. Сначала нужно найти номер пина. Для этого нужно к номеру базы добавить номер линии.

```
# ls /sys/class/gpio/  
export       gpiochip792  unexport
```
gpiochip792 - база 792
```
# gpiofind BMC_PWR_BMC
gpiochip0 208
```
линия 208
Значит номер пина 792+208=1000
```
# echo 1000 > /sys/class/gpio/export 
```
После этого будет создан каталог `/sys/class/gpio/gpio1000`

Изменить направление gpio
```
echo in > /sys/class/gpio/gpio1000/direction
echo out > /sys/class/gpio/gpio1000/direction
```

Задать значение `echo 1 > /sys/class/gpio/gpio1000/value`
Считать значение `cat /sys/class/gpio/gpio1000/value`

## Управление через Windows
Либо установка сторонеей программы типа putty, либо работа через консоль. В самом простом случае - отпарвка
```
mode COM1 BAUD=9600 PARITY=n DATA=8
echo 123 > COM1
```

Например, если установлен PowerShell, то можно воспользоваться его возможностями.

Запись:

```csharp
PS> [System.IO.Ports.SerialPort]::getportnames()
COM3
PS> $port= new-Object System.IO.Ports.SerialPort COM3,9600,None,8,one
PS> $port.open()
PS> $port.WriteLine("Hello world")
PS> $port.Close()
```

Чтение:

```csharp
PS> $port= new-Object System.IO.Ports.SerialPort COM3,9600,None,8,one
PS> $port.Open()
PS> $port.ReadLine()
```