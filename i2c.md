# Работа шины i2c
Для того чтобы с шиной возможно было взаимодействовать, её надо описать в [DevTree](dev_tree)

В .dts файл добавить нужную линию

```
&i2c2 {
	status = "okay";
};

```

# Инструменты
Просмотр линий
* `i2cdetect -y` - cписок линий
* `i2cdetect -y 2`  - список адресов на линии 2

Дамп устройства
```
i2c dump -y y 58
```

# Ручное добавление eeprom at24
Для ручного добавления eeprom (автоматическое - через добавление в DevTree) надо сделать инстант устройства драйвером 24c128. Для шины 3, и адреса epprom 0x57
`echo 24c128 0x57 > /sys/bus/i2c/devices/i2c-3/new_device`, после этого в файле `/sys/bus/i2c/devices/3-0057/eeprom` появится информация из этой eeprom

# Bind/unbind
Если драйвер не смог инициализироваться при загрузке (например не было питания устройства), можно его добавить позже с помощью bind/unbind, аналогично шине (см. ниже)

# Добавление микросхемы RTC
Для добавления микросхемы RTC её достаточно описать в [DeviceTree](dev_tree). После этого время будет браться с неё при загрузке BMC автоматически.

# Работа с шиной
Список доступных шин
```
i2cdetect -l
i2c-2	i2c       	1e78a0c0.i2c-bus                	I2C adapter
i2c-3	i2c       	1e78a100.i2c-bus                	I2C adapter
i2c-4	i2c       	1e78a140.i2c-bus                	I2C adapter
i2c-5	i2c       	1e78a180.i2c-bus                	I2C adapter
i2c-6	i2c       	1e78a1c0.i2c-bus                	I2C adapter
i2c-7	i2c       	1e78a300.i2c-bus                	I2C adapter
i2c-9	i2c       	1e78a380.i2c-bus                	I2C adapter

```

# Отключение/подключение шины во время работы
```
echo -n "1e78a140.i2c-bus" > /sys/bus/platform/drivers/aspeed-i2c-bus/unbind
echo -n "1e78a1c0.i2c-bus" > /sys/bus/platform/drivers/aspeed-i2c-bus/bind
```
