Отправка своих патчей в общий репозиторий через [gerrit](https://gerrit.openbmc.org/). Настройка описана в репозитории [docs](https://github.com/openbmc/docs/blob/master/development/gerrit-setup.md)
В нем найти нужный репозиторий и далее по инструкциям.
Мне удобно скачать репозиторий с github далее git remote add gerrit <адрес>. Далее scp в соответствии с тем что написано в gerrit.
Правим репозиторий, далее
```
git add
git commit -s
```
Для отправки патча

`git push origin HEAD:refs/for/master` или
`git push gerrit HEAD:refs/for/master` в случае remote 
`git push gerrit <Имя ветки на отправку>:refs/for/master` в случае если разработка не в главной ветке

В случае если понадобились изменения, gerrit ориентируется по `Change-Id:`, которое добавлено в коммент

[Правила написания коммита](https://cbea.ms/git-commit/)