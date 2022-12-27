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