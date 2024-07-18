# Serial on LAN (SOL)

## Вручную

За SOL отвечает программа obmc-console. Её настройки находятся по адресу /etc/obmc-console/server.\[tty№\].conf.
По-умолчанию используется VUART. Но можно вручную создать файл и запустить сервер

```
touch /etc/obmc-console/server.ttyS0.conf 
echo "baud = 115200" > /etc/obmc-console/server.ttyS0.conf
systemctl start obmc-console@ttyS0
```
Можно запускать сервер без сервиса с помощью `obmc-console-server`. Для проверки можно использовать `obmc-console-client`

Для того чтобы Linux работал с консолью, её надо прописать в [DevTree](console).


Добавить в настройку obmc-console использование ttyS0

recipes-phosphor/console/obmc-console_%.bbappend
	
```
FILESEXTRAPATHS:prepend := "${THISDIR}/${PN}/${MACHINE}:"
OBMC_CONSOLE_HOST_TTY = "ttyS0"

SRC_URI:remove = "file://${BPN}.conf"
SRC_URI += "file://server.ttyS0.conf"

do_install:append() {
        # Remove upstream-provided configuration
        rm -rf ${D}${sysconfdir}/${BPN}

        # Install the server configuration
        install -m 0755 -d ${D}${sysconfdir}/${BPN}
        install -m 0644 ${WORKDIR}/*.conf ${D}${sysconfdir}/${BPN}/

}
```

recipes-phosphor/console/obmc-console/${ИМЯ платы}/server.ttyS0.conf

```
baud = 115200
local-tty = ttyS0
```

## Статус соединения в Web-интерфейсе
Корректный статус будет работать только после того как настроить статус питания хоста
## VUART & MUX
Если BMC управляет шиной, то можно воспользоваться VUARTOM, но хост не может им управлять. Управлять хост может UART1 и UART2, но к ним напрямую нет доступа из BMC. Для работы с ними можно воспользоваться аппаратным MUXом. см. uart_route
По-умолчанию все UART направлены на физические интерфейсы

Просмотр 
```
cat /proc/tty/driver/serial
```