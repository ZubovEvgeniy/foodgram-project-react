# praktikum_new_diplom
### Запуск проекта на локальной машине:

- Клонировать репозиторий:
```
git@github.com:ZubovEvgeniy/foodgram-project-react.git
```

- В директории infra файл example.env переименовать в .env и заполнить своими данными:
```
DB_ENGINE=django.db.backends.postgresql
DB_NAME=postgres
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
DB_HOST=db
DB_PORT=5432
SECRET_KEY='секретный ключ Django'
```

- Создать и запустить контейнеры Docker, выполнить команду из директории infra:
```
docker compose up -d
```

- После успешной сборки выполнить миграции:
```
docker compose exec backend python manage.py makemigrations
docker compose exec backend python manage.py migrate
```

- Создать суперпользователя:
```
docker compose exec backend python manage.py createsuperuser
```

- Собрать статику:
```
docker compose exec backend python manage.py collectstatic --noinput
```

- Наполнить базу данных содержимым из файла ingredients.json:
```
docker compose exec backend python manage.py loaddata ingredients.json
```