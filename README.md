# Taski в Docker

Taski — трекер задач с Django REST API, React-интерфейсом, PostgreSQL и
Nginx. Все части приложения запускаются одной командой через Docker Compose.

## Локальный запуск

1. Создайте файл окружения: `cp .env.example .env`.
2. Запустите проект: `docker compose up -d --build`.
3. Примените миграции:
   `docker compose exec backend python manage.py migrate`.
4. Соберите статику Django:
   `docker compose exec backend python manage.py collectstatic --noinput`.
5. Скопируйте её в общий том:
   `docker compose exec backend cp -r /app/collected_static/. /backend_static/static/`.

Приложение будет доступно по адресу <http://localhost:8000/>.

Остановить проект: `docker compose down`. Данные PostgreSQL сохраняются в
именованном томе `pg_data`.

## Состав

- `db` — PostgreSQL;
- `backend` — Django и Gunicorn;
- `frontend` — собирает React-приложение в общий том;
- `gateway` — Nginx раздаёт интерфейс и проксирует API и админку.

`docker-compose.production.yml` использует готовые образы Docker Hub из
аккаунта `asemirkhan` вместо локальной сборки.
