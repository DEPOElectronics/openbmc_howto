# Настройки
Для каждой настройки создается dbus интерфейс  с помощью phosphor-settings

Можно добавлять свои интерфейсы добавляя по примеру host-template.yaml.
Т.к мне нужно было удалить интерфейс `xyz.openbmc_project.Control.Boot.Mode` то я удаляю host-template.yaml и добавляю свою копию host.yaml