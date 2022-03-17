# DBUS
## Инструменты
busctl - основной инструмент для просмотра и простого редактирования элементов
Примеры испльзования:
- просмотр объектов dbus
```
# busctl
```
- Просмотр дерева HealthMon
```
# busctl tree xyz.openbmc_project.HealthMon
`-/xyz
  `-/xyz/openbmc_project
    `-/xyz/openbmc_project/sensors
      `-/xyz/openbmc_project/sensors/utilization
        |-/xyz/openbmc_project/sensors/utilization/CPU
        |-/xyz/openbmc_project/sensors/utilization/Memory
        `-/xyz/openbmc_project/sensors/utilization/Storage_RW
```
- Просмотр свойств
```
# busctl introspect xyz.openbmc_project.HealthMon /xyz/openbmc_project/sensors/utilization/CPU
NAME                                          TYPE      SIGNATURE RESULT/VALUE                             FLAGS
org.freedesktop.DBus.Introspectable           interface -         -                                        -
.Introspect                                   method    -         s                                        -
org.freedesktop.DBus.Peer                     interface -         -                                        -
.GetMachineId                                 method    -         s                                        -
.Ping                                         method    -         -                                        -
org.freedesktop.DBus.Properties               interface -         -                                        -
.Get                                          method    ss        v                                        -
.GetAll                                       method    s         a{sv}                                    -
.Set                                          method    ssv       -                                        -
.PropertiesChanged                            signal    sa{sv}as  -                                        -
xyz.openbmc_project.Association.Definitions   interface -         -                                        -
.Associations                                 property  a(sss)    0                                        emits-change writable
xyz.openbmc_project.Sensor.Threshold.Critical interface -         -                                        -
.CriticalAlarmHigh                            property  b         false                                    emits-change writable
.CriticalAlarmLow                             property  b         false                                    emits-change writable
.CriticalHigh                                 property  d         90                                       emits-change writable
.CriticalLow                                  property  d         nan                                      emits-change writable
.CriticalHighAlarmAsserted                    signal    d         -                                        -
.CriticalHighAlarmDeasserted                  signal    d         -                                        -
.CriticalLowAlarmAsserted                     signal    d         -                                        -
.CriticalLowAlarmDeasserted                   signal    d         -                                        -
xyz.openbmc_project.Sensor.Threshold.Warning  interface -         -                                        -
.WarningAlarmHigh                             property  b         false                                    emits-change writable
.WarningAlarmLow                              property  b         false                                    emits-change writable
.WarningHigh                                  property  d         80                                       emits-change writable
.WarningLow                                   property  d         nan                                      emits-change writable
.WarningHighAlarmAsserted                     signal    d         -                                        -
.WarningHighAlarmDeasserted                   signal    d         -                                        -
.WarningLowAlarmAsserted                      signal    d         -                                        -
.WarningLowAlarmDeasserted                    signal    d         -                                        -
xyz.openbmc_project.Sensor.Value              interface -         -                                        -
.MaxValue                                     property  d         100                                      emits-change writable
.MinValue                                     property  d         0                                        emits-change writable
.Unit                                         property  s         "xyz.openbmc_project.Sensor.Value.Uni... emits-change writable
.Value                                        property  d         5.43353                                  emits-change writable
```
- Просмотр определенного свойства
```
# busctl get-property xyz.openbmc_project.HealthMon /xyz/openbmc_project/sensors/utilization/CPU xyz.openbmc_project.Sensor.Value Unit
s "xyz.openbmc_project.Sensor.Value.Unit.Percent"
```