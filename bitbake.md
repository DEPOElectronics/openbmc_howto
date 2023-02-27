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
```
devtool modify u-boot-fw-utils-aspeed
# fix include/configs/ast-common.h
git commit -am 'add fieldmode=true'
devtool update-recipe u-boot-foot-utils-aspeed -a {my layer path}
devtool finish u-boot-fw-utils-aspeed {my layer path}
```
## Сборка в Docker
https://github.com/crops/poky-container