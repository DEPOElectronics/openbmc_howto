# Bitbake
## Скачать все
`bitbake obmc-phosphor-image --runall=fetch`
## Проверка значения переменно
`bitbake obmc-phosphor-image -e | grep <Variable>`
## Очистить пакет для пересборки или из-зи ошибок
`bitbake obmc-phosphor-image -c cleanall`
## Указать значение внешней переменной
Указать значение переменной через переменные окружения не получилось, поэтому я создал отдельный .conf файл и указываю его как POSTFILE Bitbake. Есть еще PREFILE см. `bitbake help`
`bitbake obmc-phosphor-image -R ~/projects/sign/sign.conf`