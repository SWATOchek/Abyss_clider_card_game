# Введение 

Основное предназначение Docker - заключается в создании и развертывании приложений в среде, изолированной от других приложений. Docker-контейнеры являются удобным средством, с помощью которого разработчики могут «укладывать» свои проекты, уменьшая эффект «луж». Docker-контейнеры – это «малые», «легкие» контейнеры, которым нужно минимум ресурсов.

# Создание серверного приложения 

1. Создание папки, в которой будет складываться Dockerfile.
2. Создание Dockerfile со следующими командами:
```
FROM ubuntu:16.04 - импорт образа 
RUN apt-get update && apt-get install -y \
python3 \
python3-pip \
nginx \ 
supervisor \
pip3 install uwsgi flask - Установка необходимых пакетов и зависимостей в контейнер
EXPOSE 80 443 - открытие HTTP/HTTPS портов 
COPY ./ /app/ - копирование проекта в контейнер 
WORKDIR /app/ - создание рабочего пути
RUN chmod +x start.sh - меняем права на файл
CMD ["./start.sh"]  - запуск серверного приложения 
``` 

3. Настройка start.sh, который будет запускать uWSGI, Nginx, Flask: 

```#!/bin/bash 

# Start the first process 

nginx 

# Start the second process 

uwsgi --ini /app/uwsgi.ini & 

# Start the third process 

python3 /app/main.py & 

# Naive check runs checks once a minute to see if either of the processes exited. 

while sleep 60; do 

ps aux |grep nginx |grep -q -v grep 

PROCESS_1_STATUS=$? 

ps aux |grep uwsgi |grep -q -v grep 

PROCESS_2_STATUS=$? 

ps aux |grep main.py |grep -q -v grep 

PROCESS_3_STATUS=$? 

# If the greps above find anything, they exit with 0 status # If they are not both 0, then something is wrong if [ $PROCESS_1_STATUS -ne 0 -o $PROCESS_2_STATUS -ne 0 -o $PROCESS_3_STATUS -ne 0 ]; then echo "One of the processes has already exited." exit 1 fi done ```

4. Выполнить docker build, указав имя image: docker build -t server_image 

5. Запустить Docker-контейнер:
```docker run -d -p <host_port>:<container_port>  server_image```

6. Проверить, что контейнер успешно запущен:
```docker ps```

7. Установить нужные для работы приложения связки (bindings):
```docker exec -it  server  <command>```,
где `<command>` — команды, нужные для установки bindings.
 

# Создание веб приложения 

1. Создание Dockerfile со следующим содержимым:
```
FROM ubuntu:18.04

# Установка необходимых пакетов и зависимостей
RUN apt-get update && apt-get -y install \
wget \
curl \
git \ 
build-essential \ 
gcc \ 
make \ 
libssl-dev \ 
libffi-dev \ 
python3.6 \ 
python3-pip &&\ 

# Установка приложения 

WORKDIR /usr/src/app

COPY . .

RUN pip3 install --no-cache-dir -r requirements.txt

EXPOSE 8080
# Запуск приложения 
CMD ["python3", "app.py"] 
``` 

2. Выполнить docker build, указав имя image: `docker build -t image_app .`

3. Запустить container c image: `docker run -d -p 8080:8080 image_app`
