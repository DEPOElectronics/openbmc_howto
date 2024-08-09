# Bitbake
## Скачать все
`bitbake obmc-phosphor-image --runall=fetch`
## Проверка значения переменной
`bitbake obmc-phosphor-image -e | grep <Variable>`
## Очистить пакет для пересборки или из-за ошибок
`bitbake obmc-phosphor-image -c cleanall`
## Указать значение внешней переменной
Указать значение переменной через переменные окружения не получилось, поэтому я создал отдельный .conf файл и указываю его как POSTFILE Bitbake. Есть еще PREFILE см. `bitbake help`
`bitbake obmc-phosphor-image -R ~/projects/sign/sign.conf`
## Создать патч
Для создания патчей используется инструмент [devtool](devtool)
## Сборка в Docker
https://github.com/crops/poky-container

## Частые ошибки
### BB_ENV_EXTRAWHITE
```
$ bitbake obmc-phosphor-image
ERROR: Variable BB_ENV_EXTRAWHITE has been renamed to BB_ENV_PASSTHROUGH_ADDITIONS
ERROR: Shell environment variable BB_ENV_EXTRAWHITE has been renamed to BB_ENV_PASSTHROUGH_ADDITIONS
ERROR: Exiting to allow enviroment variables to be corrected
```
Надо сделать
```
export BB_ENV_PASSTHROUGH_ADDTIONS="$BB_ENV_EXTRAWHITE" && unset BB_ENV_EXTRAWHITE
```