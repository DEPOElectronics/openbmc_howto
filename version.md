# Версия прошивки
## Ручное управление
Версия прошивки хранится в файле `/etc/os-release`
Проект в `./meta-phosphor/recipes-core/os-release/os-release.bbappend`
По-минимому надо сделать свой файл `os-release.bbappend` в котором определить переменную
```
PHOSPHOR_OS_RELEASE_DISTRO_VERSION = "DEPO_dacn469535003v2_0.0.1"
```
## Автоматическое управление
По-умолчанию информация о версии берется с помощью команды `git describe --dirty` и состоит из следующих частей.
- Последний аннотированный тэг (с комментом)
- Количество коммитов после этого тэга (если больше 0)
- 10 символов хэша этого коммита
- В случае если в дереве есть не залитые изменения, то добавляется `-dirty`
```
$ git tag -a MySuperBMC_0.1 -m "My first release"
$ git describe --dirty
MySuperBMC_0.1
$ git commit --allow-empty -m "Empty-Commit"
MySuperBMC_0.1-1-g208efbd9f
$ //Изменить любой файл, но не заливать
MySuperBMC_0.1-1-g208efbd9f-dirty
```