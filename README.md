# Kittygram

### Ссылка на Kittygram: https://kittygramich.ddnsking.com/

## Описание

Kittygram - социальная сеть для любителей котиков, которые хотят делиться увлекательными фотографиями своих пушистых компаньонов. Этот проект включает в себя полностью функциональное бэкэнд-приложение на Django и фронтэнд-приложение на React.

Является альтернативным варинатом [Kittygram](https://github.com/YaStirayuLaskoy/infra_sprint1) с использованием Docker контейнеров

Целью проекта является практическое погружение в развертывание проекта на сервере с помощью контейнеров Docker.

## Возможности проекта

- Регистрация и авторизация пользователей
- Добавление и изменение профилей котиков
- Просмотр и взаимодействие с публикациями других пользователей

## Технологии и инструменты

- Python (Бэкенд)
- React (Фронтенд)
- WSGI-сервер [Gunicorn](https://gunicorn.org/)
- WEB-сервер [Nginix](https://nginx.org/ru/docs/)
- Зарегистрированное доменное имя [No-ip](https://www.noip.com/)
- Шифрование через HTTPS [Let's Encrypt](https://letsencrypt.org/)
- Мониторинг доступности и сбор ошибок [UptimeRobot](https://uptimerobot.com/)
- Для обеспечения безопасности, секреты подгружаются из файла .env. В файле .env содержатся важные константы, которые строго исключены из хранения в коде проекта. Настройка находится в блоке "Как запустить Kittygram".
- [Docker](https://www.docker.com/products/docker-desktop/)
- Автоматизирровано тестирование и деплой проекта Kittygram с помощью GitHub Actions


## Как запустить Kittygram на сервере

#### Создать директорию kittygram на сервере
```
cd
```

```
mkdir kittygram
```
```
cd kittygram
```

#### Установить Docker Compose на сервер

```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt-get install docker-compose-plugin 
```

#### Перенести *docker-compose.production.yml* на сервер

```
scp -i path_to_SSH/SSH_name docker-compose.production.yml \
    username@server_ip:/home/username/kittygram/docker-compose.production.yml
```
#### Перенести *.env* на сервер, вставив туда значения из .env.template

```
scp -i path_to_SSH/SSH_name .env \
    username@server_ip:/home/username/kittygram/.env
```

- path_to_SSH — путь к файлу с SSH-ключом;
- SSH_name — имя файла с SSH-ключом (без расширения);
- username — ваше имя пользователя на сервере;
- server_ip — IP вашего сервера.


#### Запустить демона

```
sudo docker compose -f docker-compose.production.yml up -d
```

#### Выполнить миграции

```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```

## Как запустить Kittygram локально

#### Клонировать репозиторий

```
git clone git@github.com:YaStirayuLaskoy/kittygram_final.git
```

#### Создать *.env* в корневой директории, вставив туда значения из .env.template

#### Запустить

```
sudo docker compose -f docker-compose.yml up
```
#### Собрать статику

```
docker compose -f docker-compose.production.yml exec backend python manage.py migrate
docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```