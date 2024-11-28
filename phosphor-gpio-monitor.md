# phosphor-gpio-monitor
Заставить систему реагировать на кнопки
Есть 2 режима  - обычный, тогда каждой кнопке создается отдельный сервис и мульти, тогда есть только `phosphor-multi-gpio-monitor.service` с конфигом `phosphor-multi-gpio-monitor.json`. 
В обычном случае привязка к gpio-keys, в мульти случае привязка идет к GPIO
В проекте phosphor-gpio-monitor есть программа monitor и present. rootее