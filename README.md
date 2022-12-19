# Проект Foodgram
![example workflow](https://github.com/ZubovEvgeniy/foodgram-project-react/actions/workflows/foodgram_workflow.yml/badge.svg)

«Foodgram - продуктовый помощник»: сайт, на котором пользователи публикуют рецепты, добавляют чужие рецепты в избранное и подписываются на публикации других авторов. А перед походом в магазин скачивают сводный список продуктов, необходимых для приготовления одного или нескольких выбранных блюд.

## Подготовка и запуск проекта
### Склонировать репозиторий на локальную машину:
```
git@github.com:ZubovEvgeniy/foodgram-project-react.git
```
## Для работы с удаленным сервером (на ubuntu):
- Выполните вход на свой удаленный сервер

- Установите docker на сервер:
```
sudo apt install docker.io 
```
- Установите docker-compose на сервер:
```
sudo apt install curl
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo apt-get install docker-compose-plugin
```
- Локально отредактируйте файл infra/nginx.conf и в строке server_name впишите свой IP
- Скопируйте файлы docker-compose.yml и nginx.conf из директории infra на сервер:
```
scp docker-compose.yml <username>@<host>:/home/<username>/docker-compose.yml
scp nginx.conf <username>@<host>:/home/<username>/nginx.conf
```
- В директории infra файл example.env переименуйте в .env и заполните своими данными:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY='секретный ключ Django'
```
* Для работы с Workflow добавьте в Secrets GitHub переменные окружения для работы:
    ```
    DB_ENGINE=<django.db.backends.postgresql>
    DB_NAME=<имя базы данных postgres>
    DB_USER=<пользователь бд>
    DB_PASSWORD=<пароль>
    DB_HOST=<db>
    DB_PORT=<5432>
    
    DOCKER_PASSWORD=<пароль от DockerHub>
    DOCKER_USERNAME=<имя пользователя>
    
    SECRET_KEY=<секретный ключ проекта django>
    USER=<username для подключения к серверу>
    HOST=<IP сервера>
    PASSPHRASE=<пароль для сервера, если он установлен>
    SSH_KEY=<ваш SSH ключ (для получения команда: cat ~/.ssh/id_rsa)>
    TELEGRAM_TO=<ID чата, в который придет сообщение>
    TELEGRAM_TOKEN=<токен вашего бота>
    ```
    Workflow состоит из четырёх шагов:
     - Проверка кода на соответствие PEP8
     - Сборка и публикация образов бекенда и фронтэнда на DockerHub.
     - Автоматический деплой на удаленный сервер.
     - Отправка уведомления в телеграм-чат.
  
* На сервере соберите docker-compose:
```
sudo docker-compose up -d --build
```
* После успешной сборки на сервере выполните команды (только после первого деплоя):
    - Соберите статические файлы:
    ```
    sudo docker-compose exec backend python manage.py collectstatic --noinput
    ```
    - Примените миграции:
    ```
    sudo docker-compose exec backend python manage.py migrate --noinput
    ```
    - Загрузите ингридиенты в базу данных:  
    ```
    sudo docker compose exec backend python manage.py loaddata ingredients.json
    ```
    - Создать суперпользователя Django:
    ```
    sudo docker-compose exec backend python manage.py createsuperuser
    ```
    - Проект будет доступен по вашему IP



Суперюзер:
username: admin
email: admin@admin.com
password: admin