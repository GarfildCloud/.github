# Облачное хранилище My Cloud

## Описание проекта

**My Cloud** — это веб-приложение для хранения файлов с возможностью загрузки, скачивания, удаления, публичного доступа по ссылке, а также администрирования пользователей. Проект разработан как дипломная работа по направлению "Fullstack-разработчик на Python" и включает:

- Бэкенд на **Django + DRF + PostgreSQL**
- Фронтенд на **React + TypeScript + Material UI**
- Развёртывание в **Docker-контейнерах** с использованием **Nginx** в качестве прокси-сервера

## Используемые технологии

### Backend:
- Python 3.12
- Django 4+
- Django REST Framework
- PostgreSQL
- Gunicorn
- Docker

### Frontend:
- React (Vite)
- TypeScript
- Redux Toolkit
- Material UI

### Инфраструктура:
- Docker & Docker Compose
- Nginx

---

## Развёртывание в продакшене на сервере (Ubuntu 24.04)

На площадке **Reg.ru** был развёрнут виртуальный сервер с ОС Ubuntu 24.04 LTS. На сервер были установлены `docker` и `docker-compose`.

### Установка зависимостей:
```bash
sudo apt update
sudo apt install -y docker.io docker-compose unzip
```

### Команды для деплоя:
```bash
mkdir MyCloud
cd MyCloud
git clone https://github.com/GarfildCloud/.github.git
git clone https://github.com/GarfildCloud/MyCloud_Backend.git
git clone https://github.com/GarfildCloud/MyCloud_Frontend.git
cd .github/
docker-compose -f docker-compose.prod.yml up -d --build
```

Проект автоматически запустится в контейнерах.

Для создания суперпользователя внутри контейнера backend выполните:
```bash
docker exec -it mycloud_backend python manage.py createsuperuser
```

---

## Развёртывание локально (без контейнеров)

Для запуска проекта вне контейнеров, потребуется установить:
- PostgreSQL
- Python 3.12+
- Node.js & npm

### 1. Подготовка базы данных
Создайте локальную базу данных `mycloud_db` с пользователем `mycloud_admin` и паролем `5tr0ngP@ss` (или измените соответствующие параметры в `.env`).

### 2. Конфигурация `.env`
В директории `MyCloud_Backend` создайте файл `.env` со следующим содержанием:
```env
DEBUG=True
SECRET_KEY=ваш_секретный_ключ
ALLOWED_HOSTS=localhost,127.0.0.1
DB_NAME=mycloud_db
DB_USER=mycloud_admin
DB_PASSWORD=5tr0ngP@ss
DB_HOST=localhost
DB_PORT=5432
```

### 3. Запуск backend
```bash
cd MyCloud_Backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python manage.py migrate
python manage.py runserver
```

### 4. Запуск frontend
```bash
cd MyCloud_Frontend
npm install
npm run dev
```

#### Важно:
- В `src/api/axios.ts` изменить базовый URL с `http://backend:8000` на `http://localhost:8000`.
- Убедитесь, что настроен CORS в backend для `localhost:3000`.

---

## Развёртывание контейнеров локально

Для запуска проекта в контейнерах с доступом к каждому сервису (бэк, фронт, база данных), необходимо раскомментировать или добавить проброс портов в `docker-compose.prod.yml`:

```yaml
  db:
    ports:
      - "5432:5432"

  backend:
    ports:
      - "8000:8000"

  nginx:
    ports:
      - "80:80"
```

После чего:
```bash
docker-compose -f docker-compose.prod.yml up -d --build
```

Фронт будет доступен по `http://localhost:80`, бэк — по `http://localhost:8000`, база данных — по `localhost:5432`.

---

## Структура репозиториев

- `.github/` — инфраструктура и конфигурации для сборки проекта (Docker, docker-compose)
- `MyCloud_Backend/` — Django-приложение с API
- `MyCloud_Frontend/` — интерфейс пользователя на React

---

## Контакты
Автор: Алексей Марулиди  
GitHub: [https://github.com/GarfildCloud](https://github.com/GarfildCloud)

---

Проект завершён и полностью соответствует требованиям дипломной работы.

