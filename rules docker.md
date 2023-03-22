# Правила по работе с докером
## 1. Старший разраб фронтенда должен создать Dockerfile для веб- и мобильного приложения.
## 2. Затем, следуя содержанию Dockerfile, создать Docker-контейнеры для каждого приложения.
## 3. Бэкэнд-разработчику следует сконфигурировать Docker-контейнеры, установив нужные пакеты, установку Node.js, Nginx, MySQL/PostgreSQL/MongoDB/Redis/etc., SSH-ключи, SSL-сертификаты, environment variables (ввод/вывод) etc.
## 4. Cледующим шагом является build image: docker build -t <image_name> .
## 5. Run image: docker run -d -p <port>:<port> —name <container_name> <image_name>
## 6. Push image to repository: docker push <repository_name>/<image_name>:<tag>
