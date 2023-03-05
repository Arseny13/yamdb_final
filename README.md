
<h1 align="center"> Проект: запуск docker-compose </h1>

![example workflow](https://github.com/github/docs/actions/workflows/main.yml/badge.svg)

___
<h4>Автор:</h4>

**Изимов Арсений**  - студент Яндекс.Практикума Когорта 17+
https://github.com/Arseny13


<h2>Описание проекта</h2>

Результаты тестовых заданий для Python-разработчиков часто просят отправлять в контейнерах. Это делается для того, чтобы человеку, который будет проверять ваше тестовое, не пришлось настраивать окружение на своём компьютере.
В этом финальном проекте ревьюер сыграет роль работодателя, а проект api_yamdb будет результатом вашего тестового задания. Ваша задача — отправить проект «работодателю» «вместе с компьютером» — в контейнере.

<h2>шаблон env-файла</h2>

- SECRET_KEY=код приложения
- DB_ENGINE=django_db
- DB_NAME=имя_бд
- DB_HOST=бд
- DB_PORT=порт_бд

<h2>Описание команд для запуска приложения в контейнерах</h2>

- 1)Запустите Docker
- 2)Перейти в /api_yamdb/ и выполнить docker build -t <имя образа> . 
- 3)Перейти в ../infra/ и выполнить 
  - docker-compose up -d --build
  - docker-compose exec web python manage.py makemigrations
  - docker-compose exec web python manage.py migrate
  - docker-compose exec web python manage.py createsuperuser
  - docker-compose exec web python manage.py collectstatic --no-input


<h2>Алгоритм регистрации пользователей</h2>

Регистрация пользователей происходит через направление пользователем POST-запроса на добавление нового пользователя с параметрами email и username на эндпоинт /api/v1/auth/signup/. Проект отправляет письмо с кодом подтверждения на адрес электронной почты, после чего пользователь отправляет POST-запрос с параметрами username и кодом подтверждения на на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит JWT-токен. В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом. Пользователь может отправить PATCH-запрос на эндпоинт /api/v1/users/me/ для заполнения своего профайла.

<h2>Ресурсы API YaMDb</h2>

- Ресурс **AUTH**: аутентификация.

- Ресурс **USERS**: пользователи.

- Ресурс **TITLES**: произведения, к которым пишут отзывы (определённый фильм, книга или песенка).

- Ресурс **CATEGORIES**: категории (типы) произведений («Фильмы», «Книги», «Музыка»).

- Ресурс **GENRES**: жанры произведений. Одно произведение может быть привязано к нескольким жанрам.

- Ресурс **REVIEWS**: отзывы на произведения. Отзыв привязан к определённому произведению.

- Ресурс **COMMENTS**: комментарии к отзывам. Комментарий привязан к определённому отзыву.


<h2>Техническая документация</h2>

Для того чтобы получить, описанные понятным языком эндпоинты и настройки, да ещё с примерами запросов, да ещё с образцами ответов! Читай ReDoc, документация в этом формате доступна по ссылке:

_http://127.0.0.1:8000/redoc/_

<h2>Как работать с API проекта YaMDb</h2>

**1. Вначале надо пользователю надо зарегистрироваться**

POST localhost/api/v1/signup/
Content-Type: application/json

{
  "username": "name_of_user",
  "email": "name@yamdb.ru"
}


**2. На указанную почту придет письмо с кодом подтверждения**

**3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт /api/v1/auth/token/, в ответе на запрос ему приходит JWT-токен.**

*Пример такого запроса:*

POST localhost/api/v1/auth/token/
Content-Type: application/json

{
  "username": "example_user",
  "password": "123456"
}

*Пример ответ от сервера:*

"token: eyJ0eXAiOiJKV1Q..."

**4. Если вы хотите добавить отзыв:**

POST http://localhost/api/v1/titles/{title_id}/reviews/
Content-Type: application/json
Authorization: Bearer "eyJ0eXAiOi..."

{
    "text": "Мой отзыв",
    "score": 1
}

<h2>Используемые технологии</h2>

- Doker=20.10.22
- nginx=1.21.3
- Python 3.7.9
- asgiref==3.2.10
- Django==2.2.16
- django-filter==2.4.0
- djangorestframework==3.12.4
- djangorestframework-simplejwt==4.8.0
- gunicorn==20.0.4
- psycopg2-binary==2.8.6
- PyJWT==2.1.0
- pytz==2020.1
- sqlparse==0.3.1
- python-dotenv==0.21.0
- pytest==6.2.4
- pytest-django==4.4.0
- pytest-pythonpath==0.7.3