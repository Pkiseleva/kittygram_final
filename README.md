# Kittygram — социальная сеть для любителей кошек

## Описание проекта

Kittygram — это веб-приложение, которое позволяет пользователям делиться фотографиями своих кошек. 
Каждый зарегистрированный пользователь может публиковать фото питомцев, указывать их имя, год рождения 
и добавлять достижения. Другие пользователи могут просматривать и оценивать опубликованных котов.

---

## Стек технологий

| Слой | Технологии |
|---|---|
| **Backend** | Python 3.11, Django, Django REST Framework, Gunicorn |
| **Frontend** | React, Node.js 20 |
| **База данных** | PostgreSQL 13 |
| **Веб-сервер** | Nginx |
| **Контейнеризация** | Docker, Docker Compose |
| **CI/CD** | GitHub Actions |
| **Уведомления** | Telegram Bot API |

---

## Развёртывание проекта

### 1. Клонировать репозиторий

```bash
git clone https://github.com/Pkiseleva/kittygram_final.git
cd kittygram_final
```

### 2. Создать файл `.env`

```bash
cp .env.example .env
```

```env
# PostgreSQL
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=your_password
DB_HOST=db
DB_PORT=5432

# Django
SECRET_KEY=секретный_ключ_django
DEBUG=False
ALLOWED_HOSTS=домен_или_ip,localhost
```

### 3. Запустить контейнеры

```bash
docker compose up -d --build
```

### 4. Применить миграции и собрать статику

```bash
docker compose exec backend python manage.py migrate
docker compose exec backend python manage.py collectstatic --no-input
```

### 5. Создать суперпользователя (опционально)

```bash
docker compose exec backend python manage.py createsuperuser
```

Проект будет доступен по адресу: `http://localhost`

---

## Описание переменных окружения

| Переменная | Описание |
|---|---|
| `POSTGRES_DB` | Имя базы данных PostgreSQL |
| `POSTGRES_USER` | Имя пользователя PostgreSQL |
| `POSTGRES_PASSWORD` | Пароль пользователя PostgreSQL |
| `DB_HOST` | Хост базы данных (обычно `db`) |
| `DB_PORT` | Порт базы данных (обычно `5432`) |
| `SECRET_KEY` | Секретный ключ Django |
| `DEBUG` | Режим отладки (`True` / `False`) |
| `ALLOWED_HOSTS` | Разрешённые хосты через запятую |

---

## CI/CD

При пуше в ветку `main` автоматически запускается GitHub Actions workflow, который:

1. Проверяет код бэкенда с помощью **ruff**
2. Запускает тесты бэкенда и фронтенда
3. Собирает Docker-образы и отправляет их на Docker Hub:
   - `username/kittygram_backend`
   - `username/kittygram_frontend`
   - `username/kittygram_gateway`
4. Отправляет уведомление в Telegram об успешном завершении

### Необходимые GitHub Secrets

| Секрет | Описание |
|---|---|
| `DOCKER_USERNAME` | Логин Docker Hub |
| `DOCKER_PASSWORD` | Пароль Docker Hub |
| `TELEGRAM_TO` | Telegram chat ID |
| `TELEGRAM_TOKEN` | Токен Telegram-бота |
