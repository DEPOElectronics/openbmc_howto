# Обновление прошивки хоста
## Включение отображения
Для обновления прошивки хоста требуется добавить параметр `flash_bios` в `phosphor-software-manager` и написать сервис `obmc-flash-host-bios@.service` который будет заниматься прошивкой. В сервис будет передана инфа о папке спрошивкой `/tmp/images/%i`.

## Текущая версия
Задать текущую версию можно с помощью dbus
```
/usr/bin/busctl set-property xyz.openbmc_project.Software.BMC.Updater /xyz/openbmc_project/software/bios_active xyz.openbmc_project.Software.Version Version s "$HOST_VERSION"
```

## Подготовка файла с прошивкой
Для подготовки файла надо воспользоваться скриптом `gen-bios-tar`, который располагается в репозитории `phosphor-bmc-code-mgmt`
```
gen-bios-tar -m dacn469535003v2 -o ~/bin/boot.tar.gz -v rev0 ~/projects/pns/image/boot.bin
```
В случае если используется подпись, то
```
gen-bios-tar -m dacn469535003v2 -o ~/bin/boot.tar.gz -v rev0 -s ~/projects/sign/key.pem ~/projects/pns/image/boot.bin
```

Имя машины должно совпадать с именем машины bmc
Добавление нескольких файлов на данный момент не поддерживается скриптом, надо будет дописать