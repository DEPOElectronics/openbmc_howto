# Датчики hwmon
Эта служба может взять датчик по пути из DeviceTree и занести его в DBUS. Но не жди что он тут же отобразится в Веб-интерфейсе, для этого надо будет еще совершить дополнительные действия. Наверное создать ассоциацию, но пока-что не получилось. [Документация](https://github.com/openbmc/docs/blob/master/architecture/sensor-architecture.md)[Доступные настройки датчика](https://www.kernel.org/doc/Documentation/hwmon/sysfs-interface)

## Расположение
* Рецепты располагаются в `recipes-phosphor/sensors`
* Конфиг располагается `/etc/default/obmc/hwmon`
* Посмотреть дерево устройств для нахождения пути кофига в работающей системе можно в `/sys/firmware/devicetree/base`

Путь конфига должен повторить путь дерева устройств. Например: `recipes-phosphor/sensors/phosphor-hwmon/obmc/hwmon/ahb/apb/bus@1e78a000/i2c-bus@1c0/lm96163@4c.conf`

Адреса устройств i2c можно посмотреть с помощью команды `i2cdetect -l` например для AST2500:
```
# i2cdetect -l
i2c-0	i2c       	1e78a040.i2c-bus                	I2C adapter
i2c-1	i2c       	1e78a080.i2c-bus                	I2C adapter
i2c-2	i2c       	1e78a0c0.i2c-bus                	I2C adapter
i2c-3	i2c       	1e78a100.i2c-bus                	I2C adapter
i2c-4	i2c       	1e78a140.i2c-bus                	I2C adapter
i2c-5	i2c       	1e78a180.i2c-bus                	I2C adapter
i2c-6	i2c       	1e78a1c0.i2c-bus                	I2C adapter
i2c-7	i2c       	1e78a300.i2c-bus                	I2C adapter
i2c-8	i2c       	1e78a340.i2c-bus                	I2C adapter
i2c-9	i2c       	1e78a380.i2c-bus                	I2C adapter
i2c-13	i2c       	1e78a480.i2c-bus                	I2C adapter
```

## Работа службы
При сборке проекта будут созданы несколько сервисов
`xyz.openbmc_project.Hwmon@[ПУТЬ].service` каждый сервис создаст объект dbus `xyz.openbmc_project.Hwmon-[ID].Hwmon1` который можно посмотреть с помощью [инструментов dbus](dbus)