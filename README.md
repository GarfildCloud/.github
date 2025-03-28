# ☁️ My Cloud — Облачное хранилище

**My Cloud** — это облачный сервис для хранения и управления файлами. Пользователи могут загружать, скачивать и делиться файлами по публичной ссылке. Администраторы имеют расширенные возможности по управлению пользователями и просмотром их файлов.

---

## 🛠️ Используемые технологии

### Backend:
- Python 3.10+
- Django 4+
- Django REST Framework
- PostgreSQL
- Docker

### Frontend:
- Vite
- React
- TypeScript
- Material UI
- Redux Toolkit

---

## 📁 Структура проекта

```
├── MyCloud_Backend         # Бэкенд Django + API
│   └── Dockerfile
├── MyCloud_Frontend        # Фронтенд React + Vite
│   └── Dockerfile
├── .github/                # Конфигурация продакшн-сборки
│   ├── docker-compose.prod.yml
│   └── nginx.conf
```

---

## 🚀 Быстрый старт (локально)

### ⚙️ Установка (только для разработки)

```bash
# Установите зависимости в backend
cd MyCloud_Backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# Примените миграции и создайте суперпользователя
python manage.py migrate
python manage.py createsuperuser

# Запуск
python manage.py runserver
```

```bash
# Установка и запуск фронтенда
cd ../MyCloud_Frontend
npm install
npm run dev
```

---

## 📦 Развёртывание через Docker

### ✅ Минимальные требования к серверу

- Docker
- Docker Compose
- Порты 80 и 443 открыты
- Рекомендуемый объём памяти: 1 ГБ+

### 🧱 Сборка и запуск

```bash
# Перейдите в директорию .github (где лежит docker-compose.prod.yml)
cd .github

# Соберите и запустите всё
docker compose -f docker-compose.prod.yml up --build
```

### 👤 Создание суперпользователя

После запуска контейнеров:

```bash
docker compose exec backend python manage.py createsuperuser
```

---

## 🔑 Авторизация и роли

- Используется JWT (access + refresh токены)
- Авторизация через `/users/token/`
- Получение текущего пользователя: `GET /api/users/me/`
- Администраторы видят административные разделы (панель управления пользователями)

---

## 🧪 API (основные примеры)

### 🔐 Аутентификация

```http
POST /api/users/token/
Content-Type: application/json

{
  "username": "admin",
  "password": "password"
}
```

### 📄 Загрузка файла

```http
POST /api/storage/
Authorization: Bearer <access_token>
Content-Type: multipart/form-data
```

### 📄 Получение списка файлов

```http
GET /api/storage/
Authorization: Bearer <access_token>
```

### 🔗 Публичная ссылка

```http
GET /api/storage/<id>/download/
PATCH /api/storage/<id>/regenerate-link/
```

---

## ⚙️ Административные возможности

Доступны только администраторам (`is_admin: true`):

- `GET /api/users/all/` — список пользователей
- `DELETE /api/users/{id}/` — удалить пользователя
- `PATCH /api/users/{id}/admin/` — сменить роль
- `GET /api/storage/?user_id={id}` — посмотреть файлы конкретного пользователя

---

## 🧩 Дополнительно

- Все маршруты frontend защищены через `ProtectedRoute`
- Публичные ссылки можно копировать и перегенерировать
- Поддерживается работа с комментариями к файлам
- Интерфейс адаптирован под десктоп, визуально похож на Nextcloud