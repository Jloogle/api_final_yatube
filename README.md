# Описание
Api для проекта yatube<br>
Позволяет обращаться к базе и получать ответ в формате JSON
# Установка
1. Склонировать репозиторий и перейти в папку с проектом командами :
 ```
 git clone git@github.com:Jloogle/api_final_yatube.git
 ```
2. Установить виртуальное окружение и активировать его:
```
 python3 -m venv venv
 ```
 ```
 source venv/bin/activate
 ```
3. Установить зависимости из файла requirements.txt
```
pip install -r requirements.txt
```
4. Выполнить миграции:
```
python3 manage.py migrate
```
5. Запустить сервер командой
```
python3 manage.py runserver
```
# Примеры
Получить данные можно по запросу http://127.0.0.1:8000/api/v1/{url необходимой модели}

1. Пример запросов к постам:<br>
Запрос на создание поста:
```
POST-запрос:

http://127.0.0.1:8000/api/v1/posts/
```
В теле должны быть указаны данные:
```
{
"text": "string",
"image": "string",
"group": 0
}
```
Ответ:
```
{
"id": 0,
"author": "string",
"text": "string",
"pub_date": "2019-08-24T14:15:22Z",
"image": "string",
"group": 0
}
```
Запросы на получение информации о постах:<br>
С указанием количества постов на странице:
```
GET-запрос

http://127.0.0.1:8000/api/v1/posts?limit=2&offset=0
```
В ответ получит данные в формате JSON:
```
{
    "count": 3,
    "next": "http://127.0.0.1:8000/api/v1/posts/?limit=2&offset=2",
    "previous": null,
    "results": [
        {
            "id": 1,
            "author": "Joe",
            "text": "First Post",
            "pub_date": "2022-03-18T14:30:26.650585Z",
            "image": null,
            "group": null
        },
        {
            "id": 2,
            "author": "admin",
            "text": "Second Post",
            "pub_date": "2022-03-18T14:30:42.615272Z",
            "image": null,
            "group": 2
        }
    ]
}
```
При обращении без параметров limit и offset, вернется:
```
[
    {
        "id": 1,
        "author": "Joe",
        "text": "First Post",
        "pub_date": "2022-03-18T14:30:26.650585Z",
        "image": null,
        "group": null
    },
    {
        "id": 2,
        "author": "admin",
        "text": "Second Post",
        "pub_date": "2022-03-18T14:30:42.615272Z",
        "image": null,
        "group": 2
    },
    ....
]
```
Пример обращения к определенному посту:
```
GET-запрос

http://127.0.0.1:8000/api/v1/posts/1/
```
```
{
    "id": 1,
    "author": "Joe",
    "text": "First Post",
    "pub_date": "2022-03-18T14:30:26.650585Z",
    "image": null,
    "group": null
}
```

2. Примеры обращения к группам: <br>
Без пагинации:
```
GET-запрос

http://127.0.0.1:8000/api/v1/groups/
```
Ответ:
```
[
    {
        "id": 1,
        "title": "First Group",
        "slug": "First",
        "description": "First group"
    },
    {
        "id": 2,
        "title": "Second Group",
        "slug": "Second",
        "description": "Second group"
    }
]
```
С пагинацией:
```
GET-запрос

http://127.0.0.1:8000/api/v1/groups/?limit=2&offset=0
```
```
{
    "count": 2,
    "next": null,
    "previous": null,
    "results": [
        {
            "id": 1,
            "title": "First Group",
            "slug": "First",
            "description": "First group"
        },
        {
            "id": 2,
            "title": "Second Group",
            "slug": "Second",
            "description": "Second group"
        }
    ]
}
```
Получение информации об определенной группе:
```
GET-запрос

http://127.0.0.1:8000/api/v1/groups/1/
```
```
{
    "id": 1,
    "title": "First Group",
    "slug": "First",
    "description": "First group"
}
```
3. Пример обращения к комментариям: <br>
Создание комментария:
```
POST-запрос:

http://127.0.0.1:8000/api/v1/posts/1/comments/
```
В теле запроса должен быть указан текст комментария:
```
{
    "text": "Текст комментария"
}
```
```
{
    "id": 1,
    "post": 1,
    "author": "admin",
    "text": "Текст комментария",
    "created": "2022-03-18T14:44:48.350870Z"
}
```
Получение информации о комментариях:

```
GET-запрос

http://127.0.0.1:8000/api/v1/posts/1/comments/1/
```
```
{
    "id": 1,
    "post": 1,
    "author": "admin",
    "text": "Текст комментария",
    "created": "2022-03-18T14:44:48.350870Z"
}
```
4. Примеры отправки запросов к подпискам:<br>
Подписка на автора:
```
POST-запрос:

http://127.0.0.1:8000/api/v1/follow/
```
В теле запроса должен быть указан пользователь, на которого хотим подписаться
```
{
    "following": "Joe"
}
```
В ответ получаем:
```
{
    "id": 1,
    "user": "admin",
    "following": "Joe"
}
```
Пример запроса на получение информации об авторах, на которых подписаны:
```
GET-запрос

http://127.0.0.1:8000/api/v1/follow/
```
Ответ:
```
[
    {
        "id": 2,
        "user": "admin",
        "following": "jloogle"
    },
    {
        "id": 1,
        "user": "admin",
        "following": "Joe"
    }
]
```
5. Пример запросов на получения токена аутентификации:
```
POST-запрос

http://127.0.0.1:8000/api/v1/jwt/create/
```
В теле толжны быть указаны:
```
{
"username": "string",
"password": "string"
}
```
Ответ:
```
{
"refresh": "string",
"access": "string"
}
```
<b>Более подробную информацию можно найти по ссылке:</b>
```
http://127.0.0.1:8000/redoc/
```