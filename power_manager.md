# Управление питанием
Есть разные программы для управлением питанием. Наиболее часто используются x86-power-control и phosphor-state-manager. Phosphor State Manager стаивтся по-умолчанию, для того иного надо в .conf файле указать
VIRTUAL-RUNTIME_obmc-host-state-manager ?= "x86-power-control"
VIRTUAL-RUNTIME_obmc-chassis-state-manager ?= "x86-power-control"
## Сервисы
Для включения сервера необходимо создать сервисы [systemd](https://github.com/openbmc/docs/blob/master/architecture/openbmc-systemd.md) которые включают, выключают (мягко и жестко), перезагружают сервер.
Для того чтобы данные сервисы вызывались необходимо ссылки на них поместить в папку /lib/systemd/system/(цель).requires (папку возможно необходимо создать) для следующих целей:
- Включение - `obmc-host-startmin@0.target`
- Выключение мягкое - `obmc-chassis-poweroff@0.target`
- Выключение жесткое - `obmc-chassis-hard-poweroff@.target`
- Перезагрузка - `obmc-host-reboot@0.target`? На данный момент так и не заработало


На данный момент не разобрался с тем кто должен обновлять статус, поэтому пока тупо сделал сервис, который раз в несколько секунд проверяет статус GPIO и записывает с помощью 
`busctl set-property xyz.openbmc_project.State.Host /xyz/openbmc_project/state/host0 xyz.openbmc_project.State.Host CurrentHostState s "xyz.openbmc_project.State.Host.HostState.Running"`
или `busctl set-property xyz.openbmc_project.State.Host /xyz/openbmc_project/state/host0 xyz.openbmc_project.State.Host CurrentHostState s "xyz.openbmc_project.State.Host.HostState.Off"`
