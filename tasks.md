# 1. Старший разраб фронтенда должен создать Dockerfile для веб- и мобильного приложения.
## 1.1Затем, следуя содержанию Dockerfile, создать Docker-контейнеры для каждого приложения.


# 2. Бэкэнд-разработчику следует сконфигурировать Docker-контейнеры, установив нужные пакеты, установку Node.js, Nginx, MySQL/PostgreSQL/MongoDB/Redis/etc., SSH-ключи, SSL-сертификаты, environment variables (ввод/вывод) etc.
## 2.1 Cледующим шагом является build image: docker build -t <image_name> .
## 2.2 Run image: docker run -d -p <port>:<port> —name <container_name> <image_name>
## 2.3 Push image to repository: docker push <repository_name>/<image_name>:<tag>
