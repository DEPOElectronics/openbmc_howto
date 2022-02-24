# Виртуальные датчики
Программа phosphor-virtual-sensor, умеет брать информацию из реальных датчиков, может производить математические операции. Например: общая мощность нескольких блоков питания, максимальная температура и т.п.

## Добавление в образ
По-умолчанию в образ не включена для включения, необходима зависимость к phosphor-virtual-sensor

## Конфигурация
Конфиг лежит в /usr/share/phosphor-virtual-sensor/ и представляет из себя массив из виртуальных сенсоров.

## Перезапуск
Для перезапуска сервиса выполните: `systemctl restart phosphor-virtual-sensor.service`

## Просмотр созданных объектов
Для просмотра объектов выполнить 
```
# busctl tree xyz.openbmc_project.VirtualSensor

`-/xyz
  `-/xyz/openbmc_project
    `-/xyz/openbmc_project/sensors
      |-/xyz/openbmc_project/sensors/power
      | `-/xyz/openbmc_project/sensors/power/total_power
      `-/xyz/openbmc_project/sensors/temperature
        `-/xyz/openbmc_project/sensors/temperature/Virtual_Inlet_Temp
```
Для более подробной информации про объект
```
busctl introspect xyz.openbmc_project.VirtualSensor /xyz/openbmc_project/sensors/power/total_power
```

Вывод на страницу сенсоров в Web
Для вывода на страницу с сенсорами может потребоваться дополнительная ассоциация с шасси или платой в файле конфигурации. Например:
```
	        "Associations":
	        [
	            [
	                "chassis",
	                "all_sensors",
	                "/xyz/openbmc_project/inventory/system/board/DEPO_DAZN"
	            ],
	            [
	                "inventory",
	                "sensors",
	                "/xyz/openbmc_project/inventory/system/board/DEPO_DAZN"
	            ]
	        ]
```

## Пример конфига
```
[
    {
        "Desc":
        {
            "Name": "total_power",
            "SensorType": "power"
        },
        "Threshold" :
        {
        },
        "Associations":
        [
            [
                "chassis",
                "all_sensors",
                "/xyz/openbmc_project/inventory/system/board/DEPO_DAZN"
            ],
            [
                "inventory",
                "sensors",
                "/xyz/openbmc_project/inventory/system/board/DEPO_DAZN"
            ]
        ],
        "Params":
        {
            "DbusParam":
            [
                {
                    "ParamName": "P0",
                    "Desc":
                    {
                        "Name": "PSU1_Output_Power",
                        "SensorType": "power"
                    }
                },
                {
                    "ParamName": "P1",
                    "Desc":
                    {
                        "Name": "PSU2_Output_Power",
                        "SensorType": "power"
                    }
                }
            ]
        },
        "Expression": "(P0 + P1) >= 0 ? (P0 + P1) : NULL"
    }
]

```

`"/xyz/openbmc_project/inventory/system/board/DEPO_DAZN"` берется из `https://$BMC/redfish/v1/Chassis/`
`"PSU1_Output_Power"` берется из `https://$BMC/redfish/v1/Chassis/DEPO_DAZN/Sensors`