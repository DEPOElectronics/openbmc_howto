# Консоль UART
Для того чтобы работать по UART надо добавить их в DevTree

[Задать своё дерево](dev_tree.md)

Добавить в DevTree

```
&uart1 {
	//Host Console
	status = "okay";
	pinctrl-names = "default";
	pinctrl-0 = <&pinctrl_txd1_default
		     &pinctrl_rxd1_default>;
};

```
Пины подключаются в соответствии с тем, какие линии подключены. Обычно либо только RX, TX. Либо все 8.

По-умолчанию для дебага BMC используется &uart5, а в алиасах указан serial4

```
	aliases {
		serial0 = &uart1;
		serial1 = &uart2; 
		serial4 = &uart5;
	};

```

Для uart5 обычно настройки не нужны

```
&uart2 {
	//IPMICOM1
	status = "ok";
	pinctrl-names = "default";
	pinctrl-0 = <&pinctrl_txd2_default
		&pinctrl_rxd2_default
		&pinctrl_nrts2_default
		&pinctrl_ndtr2_default
		&pinctrl_ndsr2_default
		&pinctrl_ncts2_default
		&pinctrl_ndcd2_default
		&pinctrl_nri2_default>;
};

&uart5 {
	status = "okay";
};
```

&uart1..&uart4 подключаются к ножкам [GPIO](gpio). Если не описывать uart, то можно работать управляя этими ножками

В системе описанные uart видны как tty*. Если их не описывать, то ttyS* в системе присутствуют, но чтение/запись не работает

##  Ручная настройка tty
Для ручной настройки baudrate и т.п используется stty
Просмотр текущих настроек ttyS0
```
# stty -F /dev/ttyS0 
speed 9600 baud; line = 0;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; eol = <undef>; eol2 = <undef>; swtch = <undef>; start = ^Q; stop = ^S; susp = ^Z; rprnt = ^R; werase = ^W; lnext = ^V; flush = ^O; min = 1; time = 0;
-brkint -imaxbel
```
Задать скорость 115200 для ttyS0 `stty -F /dev/ttyS0 115200`
## Управление через Windows
Либо установка сторонней программы типа putty, либо работа через консоль. В самом простом случае - отправка
```
mode COM1 BAUD=9600 PARITY=n DATA=8
echo 123 > COM1
```

Например, если установлен PowerShell, то можно воспользоваться его возможностями.

Запись:

```powershell
PS> [System.IO.Ports.SerialPort]::getportnames()
COM3
PS> $port= new-Object System.IO.Ports.SerialPort COM3,9600,None,8,one
PS> $port.open()
PS> $port.WriteLine("Hello world")
PS> $port.Close()
```

Чтение:

```powershell
PS> $port= new-Object System.IO.Ports.SerialPort COM3,9600,None,8,one
PS> $port.Open()
PS> $port.ReadLine()
```
## VUART & MUX
Если BMC управляет шиной, то можно воспользоваться VUARTOM, но хост не может им управлять. Управлять хост может UART1 и UART2, но к ним напрямую нет доступа из BMC. Для работы с ними можно воспользоваться аппаратным MUXом. см. uart_route
По-умолчанию все UART направлены на физические интерфейсы
## Просмотр 
```bash
cat /proc/tty/driver/serial
```