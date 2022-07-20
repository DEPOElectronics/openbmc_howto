#ПИД - регулятор
Позволяет настроить группу вентиляторов и сенсоров. Необходимо создать файл config.json
* Сервис `phosphor-pid-control.service`
* Программа `swampd`
* Для настройки [тюнинг](https://github.com/openbmc/phosphor-pid-control/blob/master/tuning.md). Не забудь выключить сервис. `swampd -t -l /tmp -c /temp.json` при этом программа будет пытьться поддерживать обороты вентилятора, указанные в `/etc/thermal.d/setpoint`


Коэффициенты при настройке
* `samplePeriod` - 0.1 для fan, 1 для temp
* `proportionalCoeff` - %/RPM (я делаю 100/7400 (максимум для этого вентилятора)). Прямой коэффициент относительно ошибки
* `integralCoeff` - %/RPM sec. Я сделал `proportionalCoeff*60`
* `feedFwdOffsetCoeff` - Добавок к SetPoint
* `feedFwdGainCoeff` - Коэффициент усиления относительно Setpoint
* `integralLimit_min` - минимальное значение интегрирующей составляющей
* `integralLimit_max` - максимальное значение интегрирующей составляющей
* `outLim_min` - минимальное выходное значение (в процентах?)
* `outLim_max` - максимальное выходное значение (в процентах?)
* `slewNeg` - ограничение скорости снижения оборотов
* `slewPos` - ограничение скорости возрастания оборотов
