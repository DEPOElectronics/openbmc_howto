# Управление питанием
Есть разные программы для управлением питанием. Наиболее часто используются `x86-power-control` и `phosphor-state-manager`. Phosphor State Manager далее `PSM` ставится по-умолчанию, для того иного надо в .conf файле указать
```
VIRTUAL-RUNTIME_obmc-host-state-manager ?= "x86-power-control"
VIRTUAL-RUNTIME_obmc-chassis-state-manager ?= "x86-power-control"
```

# BMCWeb
На вход получает следующие сообщения `bmcweb/static/redfish/v1/schema/Resource_v1.xml` и переводит их в dbus (у кого найдет)

| Действие | Сообщение | Dbus|
|-|-|-|
|Включить|On \|\| ForceOn|xyz.openbmc_project.State.Host.Transition.On|
|Немедленно выключить|ForceOff|xyz.openbmc_project.State.Chassis.Transition.Off|
|Немедленно перезагрузить|ForceRestart|xyz.openbmc_project.State.Host.Transition.ForceWarmReboot|
|Постепенное выключение|GracefulShutdown|xyz.openbmc_project.State.Host.Transition.Off|
|Постепенная перезагрузка|GracefulRestart|xyz.openbmc_project.State.Host.Transition.GracefulWarmReboot|
|Выключить-включить (отсутствует)|PowerCycle|xyz.openbmc_project.State.Host.Transition.Reboot|

# PSM
## Инициализация

Шасси считается включенным при наличии `"org.openbmc.control.Power" "/org/openbmc/control/power0" "pgood"`
Хост считается включенным если
- Включенно шасси
- Включено FirmwareCondition `xyz.openbmc_project.Condition.HostFirmware` = Running

## Реакция на команды
При обнаружении изменения dbus PSM запускает следующие цели:

|Dbus|Systemd|
|--|--|
|xyz.openbmc_project.State.Host.Transition.On|obmc-host-start@{}.target|
|xyz.openbmc_project.State.Chassis.Transition.Off|obmc-chassis-hard-poweroff@{}.target|
|xyz.openbmc_project.State.Host.Transition.ForceWarmReboot|obmc-host-force-warm-reboot@{}.target или obmc-host-reboot@{}.target|
|xyz.openbmc_project.State.Host.Transition.Off|obmc-host-shutdown@{}.target|
|xyz.openbmc_project.State.Host.Transition.GracefulWarmReboot|obmc-host-warm-reboot@{}.target или obmc-host-reboot@{}.target|
|xyz.openbmc_project.State.Host.Transition.Reboot|obmc-host-reboot@{}.target|

## Включение


Для включения сервера необходимо создать сервисы [systemd](https://github.com/openbmc/docs/blob/master/architecture/openbmc-systemd.md) которые включают, выключают (мягко и жестко), перезагружают сервер.
Для того чтобы данные сервисы вызывались необходимо ссылки на них поместить в папку /lib/systemd/system/(цель).requires (папку возможно необходимо создать) для следующих целей:
- Включение - `obmc-host-startmin@0.target`
- Выключение мягкое - `obmc-chassis-poweroff@0.target`
- Выключение жесткое - `obmc-chassis-hard-poweroff@.target`
- Перезагрузка - `obmc-host-reboot@0.target`? На данный момент так и не заработало


На данный момент не разобрался с тем кто должен обновлять статус, поэтому пока тупо сделал сервис, который раз в несколько секунд проверяет статус GPIO и записывает с помощью 
`busctl set-property xyz.openbmc_project.State.Host /xyz/openbmc_project/state/host0 xyz.openbmc_project.State.Host CurrentHostState s "xyz.openbmc_project.State.Host.HostState.Running"`
или `busctl set-property xyz.openbmc_project.State.Host /xyz/openbmc_project/state/host0 xyz.openbmc_project.State.Host CurrentHostState s "xyz.openbmc_project.State.Host.HostState.Off"`
