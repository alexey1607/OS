# Лабораторная работа №2

Requirements: 
- 2x VM with debian like OS, i.e. debian\ubuntu
- 1х VM with rhel like OS, i.e. fedora\centos
- Internet connection
- Ansible

**help**

Для ускорения поднятия VM рекомендую установить ПО:
- virtualbox: https://www.virtualbox.org/wiki/Downloads
- vagrant: https://www.vagrantup.com/downloads.html

после установки создать для каждой вм отдельную папку(сделано для упрощения) и в каждой папке выполнить команду*:
```
vagrant init debian/buster64
vagrant up
```
\* подстроку debian/buster64 подобрать в зависимости от необходимой ОС. Список доступных ОС смотри тут: https://app.vagrantup.com/boxes/search

после чего у вас будет скачаны образа и развернуты виртуальные машины. Если вы работаете под ОС Windows, вам нужно будет развернуть дополнительную VM с которой вы будете работать программой ansible.

## Задача 1: Написать playbook, который выполняет следующие действия

1. Устанавливает пакет ntpdate и выполняет синхронизацию с некоторым сервером времени (например time.windows.com)
2. Устанавливает пакет ntp на обе виртуальные машины
3. Настраивает конфиги пакета ntp через модуль template, при этом на ОС разных семейств
должны распространяться разные наборы серверов времени.

\* Сделать плейбук идемпотентным

\** Дополнительно к идемпотентности сделать так чтобы при повторном запуске при отсутствии изменений их реально не было.

## Задача 2: Написать playbook, который выполняет следующие действия

1. Устанавливает docker на один из серверов
2. Запускает сервис prometheus на сервере с docker. для этого необходимо:
- "взять" конфиг отсюда: https://github.com/prometheus/prometheus/blob/master/documentation/examples/prometheus.yml
- "взять" способ запуска отсюда: https://prometheus.io/docs/prometheus/latest/installation/
- на основании двух предыдущих пунктов написать и  "принести" unit-файл, который будет запускать сервис
3. Установить на все оставшиеся сервера node-exporter:
- прочитать документацию: https://prometheus.io/docs/guides/node-exporter/
- выполнить написание unit-файла, так чтобы сервис стартовал автоматически при старте системы
4. Добавить все сервера в конфигурационный файл prometheus
5. Зайти на веб-интерфейс prometheus и построить график использования оперативной памяти на всех серверах.

\* Сделать плейбук идемпотентным

\** Дополнительно к идемпотентности сделать так чтобы при повторном запуске при отсутствии изменений их реально не было.

## Задача 3: Написать playbook, который выполняет следующие действия

1. Настраивает hostname на каждой виртуальной машине, значение взять из названия хоста в ansible
2. Записывает для всех VM в локальную базу dns записи о каждой другой VM. т.е. таким образом с любой VM по доменному имени должна быть доступна другая VM.

## Задача 4: Написать набор ролей, которые выполняют следующие действия

Вспомогательные материалы:
- https://habr.com/ru/post/346634/
- https://docs.docker.com/registry/deploying/

1. Поднять свой docker registry на любом из серверов
2. Выполнить сборку необходимых образов из статьи во вспомогательных материалах
3. Развернуть RMQ сервер на одном и серверов, а на оставшихся развернуть воркеры
4. Продемонстрировать работоспособность системы с помощью постановки задач в очередь

## Задача 5: Написать playbook, который выполняет следующие действия

Доп условия: найти на просторах интернета некоторый готовый сайт\шаблон

1. Разворачивает указанный сайт на нескольких серверах, при этом каждый сервер должен использовать разные веб-сервера.
для примера можно взять apache, nginx, lighttpd итп

## Задача 6: Написать playbook

**Исходные данные:**
1. 3 виртуальные машины (ОС не важна)
2. 5 конфигурационных файлов (с любым содержимым), c именами:
* 00-common.conf
* 10-common.conf
* 20-vm-1.conf
* 20-vm-2.conf
* 20-vm-3.conf

**Задача:**
Положить 2 конфига *-common.conf на все 3 виртуальных машины, а остальные конфиги положить по одному на соответствующие машины (20-vm-1.conf на первую виртуалку, 20-vm-2.conf на вторую и 20-vm-3.conf на третью)

**Особое условие:** уложиться ровно в один task, при этом учесть, что количество конфигов для разных хостов может быть разным и их имена должны легко контролироваться. так, например, плейбук должен поддерживать добавление конфигов 30-vm-2.conf, 40-vm-2.conf, 50-vm-3.conf, 60-vm-2.conf итп. А также следует учесть что некоторые конфиги могут просто находиться в репозитории и не распространяться ни на какую VM, например файлы 70-vm-2.conf и 90-vm-3.conf.
