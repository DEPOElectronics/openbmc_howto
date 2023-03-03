# Создать SDK проекта

SDK проекта нужен для возможности ручной сборки программ для OpenBmc с целью их отладки
1) Настроить окружение OpenBMC
```
cd ~/openbmc
. setup MyProject
```
2) Создание дистрибьютива SDK
```
bitbake -c populate_sdk obmc-phosphor-image
```
3) Установка SDK на компьютер
```
./tmp/deploy/sdk/openbmc-phosphor-glibc-x86_64-obmc-phosphor-image-armv5e-toolchain-2.1.sh
```
4) Использование окружения SDK
```
. sdk/environment-setup-armv5e-openbmc-linux-gnueabi
```

Для просмотра кода удобно использовать
https://grok.openbmc.org/