Для управлением репозиторием используется система git
# Скачивание
git clone <название репозитория>
Предпочтительно использовать ssh путь, а не http
# Объединение с основным репозиторием
Основной репозиторий OpenBMC - `git@github.com:openbmc/openbmc.git`
Для того чтобы получить с него обновления нужно
1) Переключиться на ветку для обновления `git checkout -b update`
2) Подключить этот репохиторий как удаленный `git remote add openbmc git@github.com:openbmc/openbmc.git`
3) Обновить удаленный репозиторий `git fetch openbmc`
4) Объединить изменения с удаленным репозиторием `git merge openbmc/master`
5) Собрать проект, исправив его при необходимости `bitbake obmc-phosphor-image`
6) При необходимости закомитить изменения `git add . && git commit`
7) Переключиться назад в основную ветку `git checkout master`
8) Объединить изменения `git merge update`
9) Удалить ветку обновления `git branch -d update`