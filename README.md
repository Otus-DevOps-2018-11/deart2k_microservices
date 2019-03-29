# deart2k_microservices
deart2k microservices repository


Выполнено ДЗ 14
Технология контейнеризации. Введение в Docker

[+ ] Основное ДЗ

В процессе сделано:
Настроена интегерация с Slack и Travis CI
Установлен Docker
Запущен первый контейнер "Hello world!"
Ознакомился с командами (контейнеры + образы).
Создан образ из запущенного контейнера.

Задание со *:
сравнение вывода команд docker inspect <u_container_iddocker inspect <u_image_id>


Выполнено ДЗ 15

[+] Основное ДЗ


- В GCP cоздан новый проект docker-235015 
- Сконфигурирован gcloud для нового проекта docker
- Помощью docker-mashine создан докер хост в GCP и настроен docker-cli на работу с удаленной машиной
- Добавлен Dockerfile - сценарий для автоматизации сборки образа
- Добавлен mongod.conf - подготовленный конфигурационый файл для mongodb
- Добавлен db_config - содержит переменную окружения со ссылкой на mongodb
- Добавлен start.sh - скрипт запуска приложения
- Собран образ и запущен контейнер
- Настроен firewall в GCP
- Образ загружен на Docker hub
- Сделана проверка на локальной машине возможности загрузки образа из Docker hub и запуск контейнера.
- Сделаны различные проверки образа и контейнера


# Выполнено ДЗ № 16

* [x]  Основное ДЗ
* [x]  Задание со * 1
* [x]  Задание со * 2

## В процессе сделано:
* Установлен линтер hadolint для Dockerfile
* Подключился к docker-host
* Загружен архив reddit-microservice и распакован в директорию /src
* Созданы файлы Dockerfile в папках /src/post-py/, /src/ui/ и /src/comment/
* Файлы Dockerfile поправлены по рекомендации hadolint (кроме жестко прописаных версий)
* Собраны образы
  docker build -t deart/post:1.0 ./post-py
  docker build -t deart/comment:1.0 ./comment
  docker build -t deart/ui:1.0 ./ui
  
  Замечание №1: при создании образа deart/post:1.0 при выполнении команды pip install возникла ошибка, для устранения которой были установлены пакеты gcc=5.3.0-r0 musl-dev=1.1.14-r16
  Замечание №2: при создании образов deart/comment:1.0 и deart/ui:1.0 возникла ошибка jessie-updates/main/binary-amd64/Packages  404  Not Found. Как вычнилось Debian Jessie, jessie-updates и jessie-backports были удалены 2019-03-24. Для решения проблемы был создан sources.list с ссылками на репозиторий archive.debian.org

* Создана сеть для приложения 
  docker network create reddit
* Запущены контейнеры mongo, comment, ui, post
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit --network-alias=post  deart/post:1.0
docker run -d --network=reddit --network-alias=comment  deart/comment:1.0
docker run -d --network=reddit -p 9292:9292 deart/ui:1
* Проверил доступность и работоспособность приложения по адресу http://docker-host:9292

### Заданиче со * 1
* Остановлены запущенные контейнеры коммандой docker kill ${docker ps -q}
* Запустил контейнеры с измененными network-alias и дополнительно переданными значениями переменных
docker run -d --network=reddit --network-alias=post_db1 --network-alias=comment_db1  mongo:latest
docker run -d --network=reddit --network-alias=post1 --env POST_DATABASE_HOST=post_db1 deart/post:1.0
docker run -d --network=reddit --network-alias=comment1 --env COMMENT_DATABASE_HOST=comment_db1 deart/comment:1.0
docker run -d --network=reddit  --env COMMENT_DATABASE_HOST=comment_db1 --env POST_SERVICE_HOST=post1 -p 9292:9292 deart/ui:1.0

* Проверил доступность и работоспособность приложения по адресу http://docker-host:9292

### Образы приложений
* Для получений образа меньшего размера был изменен /ui/Dockerfile. Образ теперь строится на базе ubuntu:16.04
* Собрана новая версия образа docker build -t deart/ui:2.0 ./ui
* Новый образ получился значительно меньше предыдущего

docker images
### HW16: Задание со * 2
* Подготовил новый образ для ui. За счет использования alpine в качестве основного образа удалось дополнительно уменьшить образ работоспособности

./ui/Dockerfile.alpine

### Docker volume
* Создан docker volume - docker volume create reddit_db
* Контейнеры перезапущены, к mongodb подключен docker volume
* Добавлен новый пост, контенеры перезапущены, созданный пост остался на месте 

# Выполнено ДЗ № 17

* [x]  Основное ДЗ
* [ ]  Задание со *

## В процессе сделано:
* На хосте созданном с помощью docker-machine в GCP запущены контейнеры с различными сетевывыми драйверами: none, host.
* Запустили наш проект с использованием bridge сети с использованием сетевых алиасов. Затем запустили наш проект в 2-х bridge сетях.
* Был установлен docker-compose. 
* Собран и запущен образ приложения с помощью docker-compose.
* docker-compose.yml был изменен для работы в нескольких сетях, 
* создан файл .env в который были вынесли переменные, в docker-compose.yml были добавлены эти переменные.
* Имя проекта при запуске контейнеров изменено с помощью переменной COMPOSE_PROJECT_NAME

## Как запустить проект:
* Запустить команду docker-compose up -d

## Как проверить работоспособность:
* Перейти по ссылке  http://ip_address:9292

