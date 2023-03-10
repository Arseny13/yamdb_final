
<h1 align="center"> CI и CD проекта api_yamdb</h1>

![workflow](https://github.com/Arseny13/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)

___
<h4>Автор:</h4>

**Изимов Арсений**  - студент Яндекс.Практикума Когорта 17+
https://github.com/Arseny13

<h4>Cайт</h4>

- сайт: arsenyxiii.ddns.net
- ip: 51.250.88.11

<h4>Cуперпользователь</h4>

{
  "username": "admin",
  "confirmation_code": "28ba91ea-3e26-4be5-8812-ac022ddb88b7"
}

<h4>Админ</h4>

{
  "username": "admin1",
  "confirmation_code": "9339b635-b290-4f07-ae5f-efc189c5620c"
}

<h4>Модератор</h4>

{
  "username": "moder",
  "confirmation_code": "c2309392-28f1-4de8-8a7c-ac1eaf45e509"
}


<h4>Пользователь</h4>

{
  "username": "user",
  "confirmation_code": "f652715e-6aae-4444-acf6-b37933203564"
}



<h2>Описание проекта</h2>

В финальном задании прошлого спринта вы упаковали приложение api_yamdb в контейнеры. На этот раз вы настроите для приложения Continuous Integration и Continuous Deployment, а это значит, что реализуете: 

- автоматический запуск тестов,
- обновление образов на Docker Hub,
- автоматический деплой на боевой сервер при пуше в главную ветку main.

<h2>Что нужно сделать</h2>

1. Клонируйте репозиторий `yamdb_final` и скопируйте в него проект `api_yamdb` . Для этого проекта у вас уже настроен docker-compose для трёх контейнеров — `web`, `db` и `nginx`.
2. Создайте workflow для репозитория `yamdb_final` на GitHub Actions и дайте ему название yamdb_workflow.yml.
3. Проверьте настройки файла docker-compose.yaml: он должен разворачивать контейнер web, используя образ, который вы создали на Docker Hub.
4. Опишите workflow в файле .github/workflows/yamdb_workflow.yml.

    В workflow должно быть четыре задачи (job):

    - проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8) и запуск pytest из репозитория yamdb_final;
    - сборка и доставка докер-образа для контейнера web на Docker Hub;
    - автоматический деплой проекта на боевой сервер;
    - отправка уведомления в Telegram о том, что процесс деплоя успешно завершился.

5. В файле settings.py укажите значения по умолчанию для переменных из env-файла. Это нужно для того, чтобы тесты на платформе прошли успешно. Обратите внимание, что значения по умолчанию должны быть валидными.
  
  Хороший пример:
  ```
  'ENGINE': os.getenv('DB_ENGINE', default='django.db.backends.postgresql')
  ```  

6. Добавьте в файл README.md бейдж, который будет показывать статус вашего workflow.
7. Перед отправкой проекта на ревью создайте копию файла `yamdb_workflow.yml` и сохраните её в корневой директории проекта. Это нужно сделать для того, чтобы автоматические тесты смогли найти и проверить этот файл, так как по умолчанию он хранится в скрытой папке.

<h2>Подготовьте сервер</h2>

1. Войдите на свой удаленный сервер в облаке.
  ```
    ssh username@ip_server
  ```
2. Остановите службу nginx:
  ```
    sudo systemctl stop nginx 
  ```
3. Установите docker:
  ```
      sudo apt install docker.io 
  ```
4. Установите docker-compose, с этим вам поможет официальная документация.
5. Скопируйте файлы `docker-compose.yaml` и `nginx/default.conf` из вашего проекта на сервер в `home/<ваш_username>/docker-compose.yaml` и `home/<ваш_username>/nginx/default.con` соответственно.
Эти файлы всегда копируются вручную. Обычно они настраиваются один раз и изменения в них вносятся крайне редко.
6. Добавьте в Secrets GitHub Actions переменные окружения для работы базы данных; отредактируйте инструкции workflow для задачи deploy:
7. Запустите workflow и убедитесь, что всё работает.
8. Так же на сервере надо сделать миграции и собрать статику. 
    - docker-compose exec web python manage.py makemigrations
    - docker-compose exec web python manage.py migrate --fake reviews
    - docker-compose exec web python manage.py migrate
    - docker-compose exec web python manage.py collectstatic --no-input

<h2>Чек-лист для самопроверки</h2>

- В файле docker-compose.yaml описаны инструкции для трёх контейнеров: `web`, `db`, `nginx`.
- Настроены volumes для базы данных, статики и медиа (файлов, загружаемых пользователями).
- Директория .github/workflows содержит корректный workflow в файле yamdb_workflow.yaml.
- Проект развёрнут и запущен на боевом сервере.
- При пуше в ветку main код автоматически проверяется, тестируется, деплоится на сервер.
- В репозитории в файле README.md установлен бейдж о статусе работы workflow.
- В файле settings.py для переменных из env-файла указаны валидные значения по умолчанию.

<h2>шаблон env-файла</h2>

- SECRET_KEY=код приложения
- DB_ENGINE=django_db
- DB_NAME=имя_бд
- DB_HOST=бд
- DB_PORT=порт_бд


<h2>Алгоритм регистрации пользователей</h2>

Регистрация пользователей происходит через направление пользователем POST-запроса на добавление нового пользователя с параметрами email и username на эндпоинт `/api/v1/auth/signup/`. 
Проект отправляет письмо с кодом подтверждения на адрес электронной почты, после чего пользователь отправляет POST-запрос с параметрами username и кодом подтверждения на на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит JWT-токен. 
В результате пользователь получает токен и может работать с API проекта, отправляя этот токен с каждым запросом. 
Пользователь может отправить PATCH-запрос на эндпоинт `/api/v1/users/me/` для заполнения своего профайла.

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

_http://arsenyxiii.ddns.net/redoc/_

<h2>Как работать с API проекта YaMDb</h2>

**1. Вначале надо пользователю надо зарегистрироваться**

POST _http://arsenyxiii.ddns.net/api/v1/signup/_
Content-Type: application/json

{
  "username": "name_of_user",
  "email": "name@yamdb.ru"
}


**2. На указанную почту придет письмо с кодом подтверждения**
(на почту ничего не придет, на сервере появится файл в папке sent_emails)

**3. Пользователь отправляет POST-запрос с параметрами username и confirmation_code на эндпоинт `/api/v1/auth/token/`, в ответе на запрос ему приходит JWT-токен.**

*Пример такого запроса:*

POST _http://arsenyxiii.ddns.net/api/v1/auth/token/_ 
Content-Type: application/json

{
  "username": "example_user",
  "password": "123456"
}

*Пример ответ от сервера:*

"token: eyJ0eXAiOiJKV1Q..."

**4. Если вы хотите добавить отзыв:**

POST _http://arsenyxiii.ddns.net/api/v1/titles/{title_id}/reviews/_ 

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