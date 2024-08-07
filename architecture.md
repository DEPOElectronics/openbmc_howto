OpenBMC - основан на [Linux ядре](kernel), на нём с помощью systemd запускаются основные программы. Большинство из них называются phosphor-...
Межпроцессорное взаимодействие осуществляется с помощью dbus. Интерфейсы описаны [здесь](https://github.com/openbmc/phosphor-dbus-interfaces)
В качестве http-сервера выступает [bmcweb](https://github.com/openbmc/bmcweb). Он как и остальные программы получает данные от других программ через dbus, а сетевые запросы по спецификации [redfish](redfish)
В качестве web-морды по-умолчанию используется [webui-vue](https://github.com/openbmc/webui-vue)