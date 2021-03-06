![test_and_deploy](https://github.com/AlukardPetrovich/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

***Проект YaMDb***
---
### Описание:
Проект YaMDb собирает отзывы пользователей на произведения («Книги», «Фильмы», «Музыка»).
Пользователь может оставить развернутый отзыв с выставлением индивидуальной оценки.
У пользователей есть возможность комментировать отзывы

С помощью YaMDb возможно:
* Создавать отзыв на произведение
* Оставить комментарий на  отзыв автора
* Получение/добавление жанров, категорий и наименований произведений

---

### Стэк технологий:
Python, Django, Django Rest Framework, JWT, Swagger, Docker, Nginx, Gunicorn

---

### Установка:

В проекте настроена система CI/CD для автоматического развертывания проекта.

1. Создать удаленный виртуальный сервер в одном из сервисов предлагающих соответствующие услуги.

2. Установить на сервер Docker:
```
sudo apt install docker.io
```
и Docker compose согласно [официальной документации](https://docs.docker.com/compose/install/).

3. Склонировать репозиторий выполнив в терминале:
```
git clone git@github.com:AlukardPetrovich/yamdb_final.git
```

4. Скопировать из папки infra репозитория папку nginx с файлом конфигурации внутри и файл docker-compose.yaml в домашнюю папку пользователя на удаленном сервере.

5. Для запуска, в репозитории необходимо добавить следующие Secrets:

DOCKER_PASSWORD # Пароль для доступа к DockerHub, для обновления образа

DOCKER_USERNAME # Имя пользователя для доступа к DockerHub, для обновления образа

ALLOWED_HOSTS # Список разрешенных IP адресов

DB_ENGINE # Какая база данных используется в проекте

DB_HOST # Хост базы данных

DB_NAME # Имя базы данных проекта

DB_PORT # Порт для обращения к базе данных

POSTGRES_PASSWORD # Пароль пользователь базы данных

POSTGRES_USER # Имя пользователь базы данных

HOST # IP адрес удаленного сервера на котором разворачивается проект

USER # Имя пользователя на удаленном сервери, от имени которого разворачивается проект

SSH_KEY # Приватная часть SSH - ключа для доступа к удаленному серверу

PASSPHRASE # Пароль для SSH - ключей(если используется)

TELEGRAM_TO # id получателя уведомления telegram

TELEGRAM_TOKEN # id telegram - бота отправляющего уведомления

6. Выполнить 
```
git push
```
или вручную запустить githab actions.

7. По завершении выполнения githab actions, подключиться к удаленному серверу через ssh и выполнить миграции:
```
sudo docker compose exec web python manage.py migrate
```
создать суперпользователя при необходимости:
```
sudo docker compose exec web python manage.py createsuperuser
```
собрать статику:
```
docker-compose exec web python manage.py collectstatic --no-input
```

---

### Доступные методы API запросов:
метод                                            | GET | POST | PUT | PATCH | DEL |
-------------------------------------------------|-----|------|-----|-------|-----|
/api/v1/auth/signup/ | - | V | - | - | - |
/api/v1/auth/token/ | - | V | - | - | - |
/api/v1/categories/  | V | V | - | - | - |
/api/v1/categories/{slug}/  | - | - | - | - | V |
/api/v1/genres/ | V | V | - | - | - |
/api/v1/genres/{slug}/  | - | - | - | - | V |
/api/v1/titles/ | V | V | - | - | - |
/api/v1/titles/{title_id}/ | V | - | - | V | V |
/api/v1/titles/{title_id}/reviews/ | V | V | - | - | - |
/api/v1/titles/{title_id}/reviews/{reviews_id} | V | - | - | V | V |
/api/v1/titles/{title_id}/reviews/comment/ | V | V | - | - | - |
/api/v1/titles/{title_id}/reviews/comment/{comment_id}/ | V | - | - | V | V |
/api/v1/users/ | V | V | - | - | - |
/api/v1/users/{username}/ | V | - | - | V | V |
/api/v1/users/me/ | V | - | - | V | - |

---

### Примеры:
Получение данных о категории

Request:
```
http://<URL>/api/v1/categories/
```
Response:
```
[
{
"count": 0,
"next": "string",
"previous": "string",
"results": []
}
]
```
Полная документация доступна в Swagger-документации проекта:
```
http://<URL>/swagger/
```

---
