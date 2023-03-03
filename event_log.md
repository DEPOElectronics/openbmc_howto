Для того чтобы события отображались в браузере нужна настройка  phosphor-sel-logger и bmcweb
В phosphor-sel-logger настраивается на какие события нужна реакция
В bmcweb из каких мест события выводить?
Есть 2 глобыльных типа журнала из syslog и dbus.
Syslog лучше отображается, но не поддерживает "решение" и удаление по-одному.
dbus для нормального отображения требует патчи

# Ручное создание ошибок


xyz.openbmc_project.Logging.Entry.Level.Warning

- Emergency:
```
dbus-send --system --print-reply --type=method_call \
--dest=xyz.openbmc_project.Logging \
/xyz/openbmc_project/logging \
xyz.openbmc_project.Logging.Create.Create \
string:"Emergency error" \
string:"xyz.openbmc_project.Logging.Entry.Level.Emergency" \
dict:string:string:"ID","1","TEST","TRUE"
```
- Alert

```
dbus-send --system --print-reply --type=method_call \
--dest=xyz.openbmc_project.Logging \
/xyz/openbmc_project/logging \
xyz.openbmc_project.Logging.Create.Create \
string:"Alert error" \
string:"xyz.openbmc_project.Logging.Entry.Level.Alert" \
dict:string:string:"ID","2","TEST","Alert_TRUE"
```

Critical
```
dbus-send --system --print-reply --type=method_call \
--dest=xyz.openbmc_project.Logging \
/xyz/openbmc_project/logging \
xyz.openbmc_project.Logging.Create.Create \
string:"Critical error" \
string:"xyz.openbmc_project.Logging.Entry.Level.Critical" \
dict:string:string:"ID","2","TEST","Alert_TRUE"
```

Error
```
dbus-send --system --print-reply --type=method_call \
--dest=xyz.openbmc_project.Logging \
/xyz/openbmc_project/logging \
xyz.openbmc_project.Logging.Create.Create \
string:"xyz.openbmc_project.Common.Error.InternalFailure" \
string:"xyz.openbmc_project.Logging.Entry.Level.Error" \
dict:string:string:"_PID","666"
```

Warning
```
dbus-send --system --print-reply --type=method_call \
--dest=xyz.openbmc_project.Logging \
/xyz/openbmc_project/logging \
xyz.openbmc_project.Logging.Create.Create \
string:"small warning" \
string:"xyz.openbmc_project.Logging.Entry.Level.Warning" \
dict:string:string:"",""
```

Notice
```
dbus-send --system --print-reply --type=method_call \
--dest=xyz.openbmc_project.Logging \
/xyz/openbmc_project/logging \
xyz.openbmc_project.Logging.Create.Create \
string:"Notice" \
string:"xyz.openbmc_project.Logging.Entry.Level.Notice" \
dict:string:string:"_PID","6","a","b"
```

Informational

Debug
```
dbus-send --system --print-reply --type=method_call \
--dest=xyz.openbmc_project.Logging \
/xyz/openbmc_project/logging \
xyz.openbmc_project.Logging.Create.Create \
string:"Debug string" \
string:"xyz.openbmc_project.Logging.Entry.Level.Debug" \
dict:string:string:"TEST","TRUE","DEBUG","TRUE"
```
