# entity-manager
Состоит из нескольких частей
*  fru-device сканирует шину i2c, обнаруживает устройства у которых есть fru информация и заносит ее в dbus. Посмотреть инфу можно с помощью команды 
* 2 часть берет сопостовляет конфигурационные файлы с найденными fru устройствами и в случае совпадения, создает устройства в dbus

Просмотр обнаруженных fru
```
busctl tree --no-pager xyz.openbmc_project.FruDevice
`-/xyz
  `-/xyz/openbmc_project
    `-/xyz/openbmc_project/FruDevice
      |-/xyz/openbmc_project/FruDevice/2_100
      |-/xyz/openbmc_project/FruDevice/2_101
      |-/xyz/openbmc_project/FruDevice/2_102
      |-/xyz/openbmc_project/FruDevice/2_103
      |-/xyz/openbmc_project/FruDevice/2_111
      |-/xyz/openbmc_project/FruDevice/2_116
      |-/xyz/openbmc_project/FruDevice/2_117
      |-/xyz/openbmc_project/FruDevice/2_118
      |-/xyz/openbmc_project/FruDevice/2_119
      |-/xyz/openbmc_project/FruDevice/2_20
      |-/xyz/openbmc_project/FruDevice/2_21
      |-/xyz/openbmc_project/FruDevice/2_22
      |-/xyz/openbmc_project/FruDevice/2_23
      |-/xyz/openbmc_project/FruDevice/2_36
      |-/xyz/openbmc_project/FruDevice/2_37
      |-/xyz/openbmc_project/FruDevice/2_38
      |-/xyz/openbmc_project/FruDevice/2_39
      |-/xyz/openbmc_project/FruDevice/2_52
      |-/xyz/openbmc_project/FruDevice/2_53
      |-/xyz/openbmc_project/FruDevice/2_54
      |-/xyz/openbmc_project/FruDevice/2_55
      |-/xyz/openbmc_project/FruDevice/2_68
      |-/xyz/openbmc_project/FruDevice/2_69
      |-/xyz/openbmc_project/FruDevice/2_70
      |-/xyz/openbmc_project/FruDevice/2_71
      |-/xyz/openbmc_project/FruDevice/2_80
      |-/xyz/openbmc_project/FruDevice/2_84
      |-/xyz/openbmc_project/FruDevice/2_85
      |-/xyz/openbmc_project/FruDevice/2_86
      |-/xyz/openbmc_project/FruDevice/2_87
      |-/xyz/openbmc_project/FruDevice/2_89
      |-/xyz/openbmc_project/FruDevice/FSP800_20FM
      |-/xyz/openbmc_project/FruDevice/FSP800_50FS
      `-/xyz/openbmc_project/FruDevice/G1136_1300WNA_
```
Что за устройства 2_?? я не знаю, наверное стоит разобраться с черным списком

Просмотр информации об устройстве
```
~# busctl introspect --no-pager xyz.openbmc_project.FruDevice \
>  /xyz/openbmc_project/FruDevice/FSP800_20FM
NAME                                TYPE      SIGNATURE RESULT/VALUE       FLAGS
org.freedesktop.DBus.Introspectable interface -         -                  -
.Introspect                         method    -         s                  -
org.freedesktop.DBus.Peer           interface -         -                  -
.GetMachineId                       method    -         s                  -
.Ping                               method    -         -                  -
org.freedesktop.DBus.Properties     interface -         -                  -
.Get                                method    ss        v                  -
.GetAll                             method    s         a{sv}              -
.Set                                method    ssv       -                  -
.PropertiesChanged                  signal    sa{sv}as  -                  -
xyz.openbmc_project.FruDevice       interface -         -                  -
.ADDRESS                            property  u         80                 emits-change
.BUS                                property  u         2                  emits-change
.Common_Format_Version              property  s         "1"                emits-change
.PRODUCT_ASSET_TAG                  property  s         "2020/06/20"       emits-change writable
.PRODUCT_FRU_VERSION_ID             property  s         "9M.SA03.000F.004" emits-change
.PRODUCT_LANGUAGE_CODE              property  s         "25"               emits-change
.PRODUCT_MANUFACTURER               property  s         "FSP GROUP"        emits-change
.PRODUCT_PART_NUMBER                property  s         "9PA8002401"       emits-change
.PRODUCT_PRODUCT_NAME               property  s         "FSP800-20FM"      emits-change
.PRODUCT_SERIAL_NUMBER              property  s         " S0232000165"     emits-change
.PRODUCT_VERSION                    property  s         "20"               emits-change
```
# Конфигурационный файл
Далее создается конфигурационный json файл, и кладется в /usr/share/entity-manager/configurations/
Конфигурационный файл состоит из нескольких разделов

## Раздел "Probe"
Здесь располагается правило сопоставления информации из fru с этим конфигом. Наиболее часто сопостовляется по PRODUCT_PRODUCT_NAME, например:
```
"Probe": "xyz.openbmc_project.FruDevice({'PRODUCT_PRODUCT_NAME': 'PSSF132202A'})"
```

# Просмотр добавленного оборудования
Для просмотра найденного и добавленного оборудования. Здесь можно посмотреть минимальные и максимальные значения для датчиков, рассматривая отдельные поля
```
~# busctl tree --no-pager xyz.openbmc_project.EntityManager
`-/xyz
  `-/xyz/openbmc_project
    |-/xyz/openbmc_project/EntityManager
    `-/xyz/openbmc_project/inventory
      `-/xyz/openbmc_project/inventory/system
        `-/xyz/openbmc_project/inventory/system/powersupply
          |-/xyz/openbmc_project/inventory/system/powersupply/FSP800_20FM_PSU1
          | |-/xyz/openbmc_project/inventory/system/powersupply/FSP800_20FM_PSU1/FSP800_20FM_PSU1_FRU
          | |-/xyz/openbmc_project/inventory/system/powersupply/FSP800_20FM_PSU1/PSU1
          | |-/xyz/openbmc_project/inventory/system/powersupply/FSP800_20FM_PSU1/PSU1_Fan_1
          | `-/xyz/openbmc_project/inventory/system/powersupply/FSP800_20FM_PSU1/PSU1_LCC
          `-/xyz/openbmc_project/inventory/system/powersupply/Gospower_G1136_1300WNA_PSU2
            |-/xyz/openbmc_project/inventory/system/powersupply/Gospower_G1136_1300WNA_PSU2/Gospower_1300W_PSU2_FRU
            |-/xyz/openbmc_project/inventory/system/powersupply/Gospower_G1136_1300WNA_PSU2/PSU2
            |-/xyz/openbmc_project/inventory/system/powersupply/Gospower_G1136_1300WNA_PSU2/PSU2_Fan_1
            `-/xyz/openbmc_project/inventory/system/powersupply/Gospower_G1136_1300WNA_PSU2/PSU2_LCC
```

Просмотр текущих значений раполагается в других местах в зависимости от типа оборудования. Например для блоков питания информацию надо смотреть здесь:
```
~# busctl tree --no-pager xyz.openbmc_project.PSUSensor    
`-/xyz
  `-/xyz/openbmc_project
    |-/xyz/openbmc_project/State
    | `-/xyz/openbmc_project/State/Decorator
    |   |-/xyz/openbmc_project/State/Decorator/PSU1_OperationalStatus
    |   `-/xyz/openbmc_project/State/Decorator/PSU2_OperationalStatus
    |-/xyz/openbmc_project/control
    | `-/xyz/openbmc_project/control/fanpwm
    |   `-/xyz/openbmc_project/control/fanpwm/Pwm_PSU2_Fan_1
    `-/xyz/openbmc_project/sensors
      |-/xyz/openbmc_project/sensors/current
      | |-/xyz/openbmc_project/sensors/current/PSU2_Input_Current
      | `-/xyz/openbmc_project/sensors/current/PSU2_Output_Current
      |-/xyz/openbmc_project/sensors/fan_pwm
      | `-/xyz/openbmc_project/sensors/fan_pwm/Pwm_PSU2_Fan_1
      |-/xyz/openbmc_project/sensors/fan_tach
      | `-/xyz/openbmc_project/sensors/fan_tach/PSU2_Fan_Speed_1
      |-/xyz/openbmc_project/sensors/power
      | |-/xyz/openbmc_project/sensors/power/PSU2_Input_Power
      | `-/xyz/openbmc_project/sensors/power/PSU2_Output_Power
      |-/xyz/openbmc_project/sensors/temperature
      | `-/xyz/openbmc_project/sensors/temperature/PSU2_Temperature
      `-/xyz/openbmc_project/sensors/voltage
        |-/xyz/openbmc_project/sensors/voltage/PSU1_Input_Voltage
        `-/xyz/openbmc_project/sensors/voltage/PSU2_Input_Voltage
~# busctl introspect xyz.openbmc_project.PSUSensor /xyz/openbmc_project/sensors/voltage/PSU1_Input_Voltage
NAME                                                  TYPE      SIGNATURE RESULT/VALUE                             FLAGS
org.freedesktop.DBus.Introspectable                   interface -         -                                        -
.Introspect                                           method    -         s                                        -
org.freedesktop.DBus.Peer                             interface -         -                                        -
.GetMachineId                                         method    -         s                                        -
.Ping                                                 method    -         -                                        -
org.freedesktop.DBus.Properties                       interface -         -                                        -
.Get                                                  method    ss        v                                        -
.GetAll                                               method    s         a{sv}                                    -
.Set                                                  method    ssv       -                                        -
.PropertiesChanged                                    signal    sa{sv}as  -                                        -
xyz.openbmc_project.Association.Definitions           interface -         -                                        -
.Associations                                         property  a(sss)    2 "inventory" "sensors" "/xyz/openbmc... emits-change
xyz.openbmc_project.Sensor.Value                      interface -         -                                        -
.MaxValue                                             property  d         300                                      emits-change
.MinValue                                             property  d         0                                        emits-change
.Unit                                                 property  s         "xyz.openbmc_project.Sensor.Value.Uni... emits-change
.Value                                                property  d         226.25                                   emits-change writable
xyz.openbmc_project.State.Decorator.Availability      interface -         -                                        -
.Available                                            property  b         true                                     emits-change writable
xyz.openbmc_project.State.Decorator.OperationalStatus interface -         -                                        -
.Functional                                           property  b         true                                     emits-change
~# busctl get-property xyz.openbmc_project.PSUSensor /xyz/openbmc_project/sensors/voltage/PSU1_Input_Voltage xyz.openbmc_project.Sensor.Value Value
d 225.75

```
# Остановка сервиса
Для остановки сервиса:
`systemctl stop xyz.openbmc_project.EntityManager.service`

# Изменение кофигурации
При изменении конфигурационного файла, для просмотра изменений необходимо
1) Удалить файл /var/configuration/system.json
2) Перезапустить сервис entitymanager
```
rm -f /var/configuration/system.json &&systemctl restart xyz.openbmc_project.EntityManager.service
```
	

#  [Добавление платы](em_board)
# [Добавление блока питания](em_psu)
# Проблемы
entity-manager работает только когда по dbus несколько секунд не будет приходить изменений типа `NameOwnerChanged`
Проверить можно вызвал команду:
```
dbus-monitor --system type='signal',sender='org.freedesktop.DBus',member='NameOwnerChanged'"
 ```