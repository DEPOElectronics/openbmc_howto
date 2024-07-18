#ПИД - регулятор
Позволяет настроить группу вентиляторов и сенсоров. Необходимо создать файл config.json
* Сервис `phosphor-pid-control.service`
* Программа `swampd`
* Для настройки [тюнинг](https://github.com/openbmc/phosphor-pid-control/blob/master/tuning.md). Не забудь выключить сервис. `swampd -t -l /tmp -c /temp.json` при этом программа будет пытьться поддерживать обороты вентилятора, указанные в `/etc/thermal.d/setpoint`


Коэффициенты при настройке
* `samplePeriod` - 0.1 для fan, 1 для temp
* `proportionalCoeff` - %/RPM (я делаю 100/7400 (максимум для этого вентилятора)). Прямой коэффициент относительно ошибки
* `integralCoeff` - %/RPM sec. Я сделал `proportionalCoeff*60`
* `feedFwdOffsetCoeff` - Добавок к SetPoint
* `feedFwdGainCoeff` - Коэффициент усиления относительно Setpoint
* `integralLimit_min` - минимальное значение интегрирующей составляющей
* `integralLimit_max` - максимальное значение интегрирующей составляющей
* `outLim_min` - минимальное выходное значение (в процентах?)
* `outLim_max` - максимальное выходное значение (в процентах?)
* `slewNeg` - ограничение скорости снижения оборотов
* `slewPos` - ограничение скорости возрастания оборотов

## Настройка через EM
### fansensor добавить tach

json
```
        {
            "Index": 13,
            "Name": "FAN_CPU0",
            "PowerState": "On",
            "Type": "AspeedFan"
        }
```
console
```
# busctl tree xyz.openbmc_project.FanSensor
`- /xyz
  `- /xyz/openbmc_project
    |- /xyz/openbmc_project/control
    |- /xyz/openbmc_project/inventory
    `- /xyz/openbmc_project/sensors
      `- /xyz/openbmc_project/sensors/fan_tach
        `- /xyz/openbmc_project/sensors/fan_tach/FAN_CPU0

```

### fansensor добавть pwm
json
```
        {
            "BindConnector": "CPU0 connector",
            "Index": 13,
            "Name": "FAN_CPU0",
            "PowerState": "On",
            "Type": "AspeedFan"
        },
        {
            "Name": "CPU0 connector",
            "PwmDev": "pwm-cpu0",
            "PwmName": "PWM-CPU0",
            "Tachs": [],
            "Type": "FANConnector"
        }
```
console
```
# busctl tree xyz.openbmc_project.FanSensor
`- /xyz
  `- /xyz/openbmc_project
    |- /xyz/openbmc_project/control
    | `- /xyz/openbmc_project/control/fanpwm
    |   `- /xyz/openbmc_project/control/fanpwm/PWM_CPU0
    |- /xyz/openbmc_project/inventory
    `- /xyz/openbmc_project/sensors
      |- /xyz/openbmc_project/sensors/fan_pwm
      | `- /xyz/openbmc_project/sensors/fan_pwm/PWM_CPU0
      `- /xyz/openbmc_project/sensors/fan_tach
        `- /xyz/openbmc_project/sensors/fan_tach/FAN_CPU0
```
