Основная группа проекта в GitHub [здесь](https://github.com/openbmc/)
Официальный [репозиторий](https://github.com/openbmc/openbmc)
Официальная [документация](https://github.com/openbmc/docs/)
Группа от [Aspeed](https://github.com/AspeedTech-BMC/) 
Обычно использую клон репозитория от OpenBmc. В случае с ast2600 я использую репозиторий от OpenBMC, а пакет linux от Aspeed. Т.к в линуксе от Aspeed есть гораздо больше драйверов для ast2600, которые еще не успели перекочевать в основной проект
# [Архитектура проекта](architecture)
# [Скачивание и обновление репозитория](git)
# [Сборка OpenBMC или других программ](build)
# [Создание новой сборки](new)
# [Ядро Linux](kernel)
# [Device Tree](dev_tree.md)
# [GPIO](gpio.md)
# [LED](led.md)

### Добавление реакции на кнопку

По адресу `recipes-phosphor/gpio` создать сервис id-button (название по-смыслу), который будет реагировать на нажатие этой кнопки. Создать `recipes-phosphor/packagegroups/packagegroup-obmc-apps.bbappend` 

```
RDEPENDS:${PN}-inventory:append:dazn = " id-button"
```
# [I2C](i2c.md), [I3C](i3c)

# [kcs](kcs)
# [IKVM](ikvm.md)
# [IPMI](IPMI)
# [Serial Over Lan SOL](sol.md)

# WEB

## Добавить web-интерфейс

В минимальный образ уже добавлен Web-интерфейс. Для этого, в образ системы `/recipes-phosphor/images/obmc-phosphor-image.bbappend` в `OBMC_IMAGE_EXTRA_INSTALL:append` добавлена строка `webui-vue`

# [Отладка](debug.md)
# [Управление питанием](power_manager.md)
# [Управление вентиляторами](fan_control.md)
# [Датчики на плате](inventory.md)
# [Драйверы](drivers/README.md)
# [Логирование](logging.md)
# [Запуск в qemu](qemu.md)
# [Redfish](redfish.md)
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
# [Web-интерфейс](webui-vue)