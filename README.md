# [Сборка OpenBMC или других программ](build)
# [Создание новой сборки](new)
# [Device Tree](dev_tree.md)
# [GPIO](gpio.md)
# [LED](led.md)

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

# [I2C](i2c.md), [I3C](i3c)

# [kcs](kcs)
# [IKVM](ikvm.md)
# [IPMI](IPMI)
# [Serial Over Lan SOL](sol.md)

# WEB

## Добавить web-интерфейс

В минимальный образ уже добавлен Web-интерфейс. Для этого, в образ системы `/recipes-phosphor/images/obmc-phosphor-image.bbappend` в `OBMC_IMAGE_EXTRA_INSTALL:append` добавлена строка `webui-vue`


## Включить лог по dbus

Добавить `-Dredfish-dbus-log=enabled` в ``EXTRA_OEMESON:append` в вашем слое `bmcweb_%.bbappend``

# [Отладка](debug.md)
# [Управление питанием](power_manager.md)
# [Управление вентиляторами](fan_control.md)
# [Датчики на плате](inventory.md)
# [Драйверы](drivers/README.md)
# [Логирование](event_log.md)
# [Запуск в qemu](qemu.md)
# [Redfish](redfish.md)
# Ядро
Посмотреть параметры ядра внутри BMC `zcat /proc/config.gz`
# [Версия BMC](version.md)
# [Выбор файловой системы](ubifs.md)
# [Добавление резервной флэш](reserved_flash.md)
# [Обновление прошивки хоста](host_firmware_update.md)
# [Обновление MAC адреса](MAC)
# [Цифровая подпись образа](signing.md)
# [BitBake](bitbake.md) 
# [Включение шины LPC](LPC.md)
# [Настройки](phosphor-settings)
В том числе удаление настройки загрузки хоста
# [Частые проблемы](bugs)
# [Автоматическое тестирование](test)
# Список устанавливаемых пакетов
```
VIRTUAL-RUNTIME_obmc-inventory-manager = "entity-manager"
VIRTUAL-RUNTIME_obmc-sensors-hwmon = "dbus-sensors"
```
packagegroup
# [АЦП](adc.md)
Запись консоли хоста
# [Работа с SPD ОЗУ](DIMM)