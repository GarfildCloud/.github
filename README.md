# Облачное хранилище My Cloud

## Краткое описание

**My Cloud** — веб-приложение для хранения файлов с поддержкой загрузки, доступа по ссылке, удаления, переименования, редактирования комментариев. Управлять файлами можно через свое хранилище, а администратор видит хранилища всех.

## Стек

### Backend:
- Python 3.12
- Django + Django REST Framework
- PostgreSQL
- Gunicorn
- Docker

### Frontend:
- React + Vite
- TypeScript + Redux Toolkit + Material UI

### Инфраструктура:
- Docker & Docker Compose
- Nginx

## Функционал

- Регистрация и вход
- Разделение ролей: пользователи и админы
- Сессионная авторизация
- Загрузка/скачивание/удаление файлов
- Переименование и редактирование комментария
- Публичная ссылка для обмена
- Админка: управление ролями, просмотр файлов любого пользователя

## Установка на Ubuntu 24.04 (Docker)

```bash
sudo apt update
sudo apt install -y docker.io docker-compose
```

### Скачать и развернуть:

```bash
mkdir MyCloud
cd MyCloud
git clone https://github.com/GarfildCloud/.github.git
git clone https://github.com/GarfildCloud/MyCloud_Backend.git
git clone https://github.com/GarfildCloud/MyCloud_Frontend.git
cd .github
docker-compose -f docker-compose.prod.yml up -d --build
```


## Структура репозитория

- `.github/` — Docker 
- `MyCloud_Backend/` — Django API
- `MyCloud_Frontend/` — React-приложение + nginx