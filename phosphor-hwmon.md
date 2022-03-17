# Датчики hwmon
Эта служба может взять датчик по пути из DeviceTree и занести его в DBUS. Но не жди что он тут же отобразится в Веб-интерфейсе, для этого надо будет еще совершить дополнительные действия. Наверное создать ассоциацию, но пока-что не получилось. [Документация](https://github.com/openbmc/docs/blob/master/architecture/sensor-architecture.md)[Доступные настройки датчика](https://www.kernel.org/doc/Documentation/hwmon/sysfs-interface)

## Расположение
* Рецепты располагаются в `recipes-phosphor/sensors`
* Конфиг располагается `/etc/default/obmc/hwmon`
* Посмотреть дерево устройств для нахождения пути кофига в работающей системе можно в `/sys/firmware/devicetree/base`

Путь конфига должен повторить путь дерева устройств. Например: `recipes-phosphor/sensors/phosphor-hwmon/obmc/hwmon/ahb/apb/bus@1e78a000/i2c-bus@1c0/lm96163@4c.conf`

## Работа службы
При сборке проекта будут созданы несколько сервисов
`xyz.openbmc_project.Hwmon@[ПУТЬ].service` каждый сервис создаст объект dbus `xyz.openbmc_project.Hwmon-[ID].Hwmon1` который можно посмотреть с помощью [инструментов dbus](dbus)