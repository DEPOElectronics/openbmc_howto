# Автоматическое тестирование
Для автоматического тестирования существует проект phosphor-test-automation
Перед запуском использую (без этого у меня не работает)
```
virtualenv -p /usr/bin/python3 vchitrali
source vchitrali/bin/activate
```

Для запуска теста
```
robot -v OPENBMC_HOST:xx.xx.xx.xx [путь к тесту]
```

Параметры теста можно вынести в отдельный файл и запускать
```
robot --argumentfile test_lists/dacn469535003v2_shorttest .
```

Частые параметры
```
-v OPENBMC_HOST:bmc
-v OS_USERNAME:root
-v OS_PASSWORD:root  

# Включить только следующий тест
--include Test_SSH_And_IPMI_Connections
# Отключить следующий тест из набора
--exclude Check_For_Application_Failures 
```

При одновременном использовании  противоречивых параметров, приоритет за последним