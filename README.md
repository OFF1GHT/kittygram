# Руководство по установке и запуску проекта "Kittygram"

Этот проект позволяет пользователям добавлять информацию о своих питомцах - котиках, включая их кличку, год рождения, цвет и достижения. Проект использует Gunicorn и Nginx для обеспечения стабильности и высокой производительности.

[![Main Kittygram workflow](https://github.com/OFF1GHT/kittygram_final/actions/workflows/main.yml/badge.svg)](https://github.com/OFF1GHT/kittygram_final/actions/workflows/main.yml)

#  Как работать с репозиторием финального задания

## Установка проекта
1. Клонируйте репозиторий
```
git@github.com:OFF1GHT/kittygram_final.git
```

2. Переименуйте файл '.env.example' на 'env' и подставте свои данные.


3. Добавьте секреты в GitHub Action:
- DOCKER_USERNAME - ваш логин на DockerHub
- DOCKER_PASSWORD - ваш пароль на DockerHub
- SSH_PASSPHRASE - пароль от удаленного сервера
- HOST - IP адрес сервера
- USER - имя пользователя на удаленном сервере
- SSH_KEY - приватный ключ для подключения к удаленному серверу
- TELEGRAM_TO - id пользователя в Telegram
- TELEGRAM_TOKEN - токен Telegram-бота

## На удаленном сервере

4. Запустите проект:
```
sudo docker compose -f docker-compose.production.yml up -d
```

5. Подключитесь к удаленному серверу и выполните эти команды:
```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin
```

6. В директорию 'kityygram' добавте файл docker-compose.production.yml.

7. Запустите Docker Compose в режиме демона:
```
sudo docker compose -f docker-compose.production.yml up -d
```

8. Выполните миграции, соберите статику, скопируйте ее в /backend_static/static/:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

9. Обновите настройки Nginx. Откройте файл конфигурации: 
```
sudo nano /etc/nginx/sites-enabled/default
```

10. Добавьте новые настройки:
```
location / {
    proxy_set_header Host $http_host;
    proxy_pass http://127.0.0.1:9000;
}
```

11. Проверьте работоспособность:
```
sudo nginx -t
```

12. Перезагрузите конфигурацию Nginx:
```
sudo systemctl reload nginx
```

## Запуск проекта на локальном хосте

1. Клонируйте репозиторий:
```
git@github.com:OFF1GHT/kittygram_final.git
```

2. Запустите контейнеры:
```
docker-compose up -d
```

3. Остановка контейнера и повторный запуск:
```
docker container stop имя_контейнера
docker container start имя_контейнера
```

4. Проект доступен по адресу 'http://localhost:9000'

## Ссылка на проект
```
https://infrasprint1.hopto.org
```
## Чем отличается продакшн-версия от обычной и в каком случае нужно использовать ту или иную верисию.

### Продакшн-версия
Продакшн версию стоит использоват когда вы хотите предоставить доступ реальным пользователям.

### Обычная версия
Обычныую верисию стоит использовать для тестирования и разработки новых функций.

## Автор проекта: OFF1GHT

## Используемые технолгии:
- Python
- Postgres
- Docker
- Nginx
- Gunicorn
- Django
