
# Цифровая подпись образа
По-умолчанию каждый образ подписывается "открытой" цифровой подписью, "закрытый" ключ которой находится в `meta-phosphor/recipes-phosphor/flash/files/OpenBMC.priv`.
Подпись будет записываться  в образ если включить флаг `verify_signature` в пакете `phosphor-software-manager`
Для этого в файл bbappend обавить
`PACKAGECONFIG:append = " verify_signature"`. Для примера смотри `meta-yadro/meta-nicole/recipes-phosphor/flash/phosphor-software-manager_%.bbappend`
Для использования своей подписи нужно
- Создать её `openssl genpkey -algorithm rsa -out key.pem`
- Задать в переменной `SIGNING_KEY` путь к этому ключу
При сборке образа в образ зашивается публичный ключ, получаемый из приватного.
- Публичный ключ в образе находится здесь: `/etc/activationdata/{keyname}/publickey`
- Просмотреть публичный ключ к своему приватному ключу вручную можно так: `openssl pkey -in key.pem -pubout`
Ключи должны совпасть

Если кроме образа BMC еще предпологается прошивка [хоста](host_firmware_update.md), то файлы прошивки надо прописать в `optional-images`. Для этого в файл .bbappend добавляем
```
EXTRA_OEMESON:append = " \
    -Doptional-images='boot.bin' \
"
```

Для того чтобы система отказывалась принимать чужие прошивки, необходимо выставить fieldmode.
Вручную это можно сделать [так](https://github.com/openbmc/docs/blob/master/architecture/code-update/code-update.md#software-field-mode)
```
export token=`curl -k -H "Content-Type: application/json" -X POST https://${bmc}/login -d '{"username" :  "root", "password" :  "0penBmc"}' | grep token | awk '{print $2;}' | tr -d '"'`
curl -b cjar -k -H 'Content-Type: application/json' -X PUT -d '{"data":1}' -H "X-Auth-Token: $token" https://bmc/xyz/openbmc_project/software/attr/FieldModeEnabled
```
Или так:
```
busctl set-property xyz.openbmc_project.Software.BMC.Updater /xyz/openbmc_project/software xyz.openbmc_project.Control.FieldMode FieldModeEnabled b true
```
Просмотр текущего состояния
```
busctl get-property xyz.openbmc_project.Software.BMC.Updater /xyz/openbmc_project/software xyz.openbmc_project.Control.FieldMode FieldModeEnabled
```
Для того чтобы этот режим был выставлен автоматом, надо в переменную окружения u-boot добавить `fieldmode=true`. На данный момент phosphor-bmc-mgmt имеет баг, и ищет данную переменную по адресу  /dev/mtd/u-boot-env.