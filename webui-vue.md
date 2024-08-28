[Репозиторий](https://github.com/openbmc/webui-vue) на данный момент реализован с помощью vue2, но активно пытаются перейти на vue3. 
Кастомизация интерфейса описана [здесь](https://github.com/openbmc/webui-vue/blob/master/docs/customization/build.md#build-customization)
# Замена шрифта
`CUSTOM_STYLES=true`, создать свой файл `_depo.scss`, в нём определить `$font-family-base`
