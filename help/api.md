Ниже приведены методы HTTP API для работы с данными Point.im.

> Для повышения читаемости все ответы были отформатированы и прокомментированы. Реальный ответ сервера будет в минифицированном виде

[TOC]

# Аутентификация #

### Вход ###

**Запрос**

POST `/api/login`

**Параметры**

`login` — имя пользователя. Обязательный параметр.

`password` — пароль. Обязательный параметр.

Далее полученный token нужно передавать в заголовках запросов:

`Authorization: <token>`

Как альтернативу этому заголовку можно использовать cookie "user", которая должна содержать этот же token.

**Ответ**

Пример запроса:
```
http://point.im/api/login

login=xxx&password=yyy
```
Пример ответа:
```js
{
  /* Авторизационный токен */
  "token": "b444ac06613fc8d63795be9ad0beaf55011936ac",
  /* CSRF-токен */
  "csrf_token": "109f4b3c50d7b0df729d299bc6f8e9ef9066971f"
}
```

### Выход ###

**Запрос**

POST `/api/logout`

**Параметры**

`csrf_token` — Обязательный параметр. Получается в `/api/login`.

**Ответ**

Пример запроса:
```
http://point.im/api/logout

csrf_token=109f4b3c50d7b0df729d299bc6f8e9ef9066971f
```

Пример ответа:
```js
{ "ok": true }
```


# Посты #

## Обычные посты ##

> Примеры запросов, требующих авторизации сделаны, от имени [@skobkin-ru](http://skobkin-ru.point.im/)

### Список последних записей в ленте ###

**Запрос**

GET `/api/recent`

**Параметры**

`before` — идентификатор записи, от которой нужно начать выборку. Не включает запись.

Необходима аутентификация.

**Ответ**

Пример запроса:
```
http://point.im/api/recent
```
Пример ответа:
```js
{
  /* Наличие кнопки "Предыдущие */
  "has_next": true,
  /* Массив постов */
  "posts": [
    /* Объект метапоста для обычного поста */
    {
      /* Добавлен ли пост в закладки */
      "bookmarked": false,
      /* Уникальный числовой идентификатор в выборке, используется для пагинации (параметр before) */
      "uid": 1291267,
      /* Статус подписки на пост (комментарии) */
      "subscribed": true,
      /* Разрешено ли редактирование */
      "editable": false,
      /* Рекомендован ли пост */
      "recommended": true,
      /* Объект поста */
      "post": {
        /* Массив тегов */
        "tags": [
          "lh",
          "q"
        ],
        /* Количество комментариев */
        "comments_count": 1,
        /* Объект юзера, который написал пост */
        "author": {
          /* Логин автора поста */
          "login": "Daemon",
          /* ID автора поста */
          "id": 195,
          /* Аватарка автора */
          "avatar": "daemon.jpg?r=4717",
          /* Имя автора поста */
          "name": "Daemon"
        },
        /* Текст поста */
        "text": "1) \u0424\u043e\u0442\u043a\u0430\u0435\u0448\u044c \u0444\u0438\u0433\u0443\u0440\u0443 2) \u0414\u0438\u0447\u0430\u0439\u0448\u0435 \u0436\u0440\u0435\u0448\u044c \u043f\u043e\u043b\u0433\u043e\u0434\u0430 3) \u0424\u043e\u0442\u043a\u0430\u0435\u0448\u044c \u0435\u0449\u0435 \u0440\u0430\u0437 4) \u041c\u0435\u043d\u044f\u0435\u0448\u044c \u0434\u043e \u0438 \u043f\u043e\u0441\u043b\u0435 \u043c\u0435\u0441\u0442\u0430\u043c\u0438 5) \u041f\u043e\u0441\u0442\u0438\u0448\u044c 6) \u0421\u043e\u0431\u0438\u0440\u0430\u0435\u0448\u044c \u043b\u0430\u0439\u043a\u0438 7) \u0414\u0430\u0451\u0448\u044c \u0441\u043e\u0432\u0435\u0442\u044b",
        /* Дата написания */
        "created": "2014-03-29T15:29:41.561889",
        /* Тип поста */
        "type": "post",
        /* Символьный идентификатор (используется в URL) */
        "id": "lby",
        /* Является ли пост приватным */
        "private": false
      }
    },
    
    /* Объект метапоста для рекомендованного поста */
    {
      "bookmarked": false,
      "uid": 1309953,
      "subscribed": true,
      "editable": false,
      "recommended": false,
      /* Данные о рекомендации */
      "rec": {
        /* Текст рекомендации */
        "text": null,
        /* Идентификатор комментария */
        "comment_id": null,
        /* Пользователь, который рекомендовал пост или комментарий */
        "author": {
          "login": "arts",
          "id": 1,
          "avatar": "arts.jpg?r=4778",
          "name": "\u0410\u0440\u0442\u0451\u043c \u0421\u0430\u0436\u0438\u043d"
        }
      },
      /* Объект поста */
      "post": {
        "tags": [
          "api",
          "point",
          "android"
        ],
        "comments_count": 23,
        "author": {
          "login": "Tishka17",
          "id": 434,
          "avatar": "tishka17.png?r=6953",
          "name": "Tishka17"
        },
        "text": "\u041a\u0430\u043a-\u0442\u043e \u0442\u0430\u043a http://imgur.com/dUNiHeq",
        "created": "2014-03-31T11:30:28.277053",
        "type": "post",
        "id": "acih",
        "private": false
      }
    }
    {
      /* Следующий пост */
    }
  ]
}
```


### Список последних записей в блоге пользователя ###

**Запрос**

GET `/api/blog/<login>`

**Параметры**

`before` — уникальный числовой идентификатор записи `uid`, от которой нужно начать выборку. Не включает запись.

**Ответ**

Пример запроса:
```
http://point.im/api/blog/skobkin-ru
```
Структура ответа аналогична `/api/recent`


### Список комментируемых постов ###

**Запрос**

GET `/api/comments`

**Параметры**

`before` — уникальный числовой идентификатор записи `uid`, от которой нужно начать выборку. Не включает запись.

Необходима аутентификация.

**Ответ**

Пример запроса:
```
http://point.im/api/comments
```

## Личные сообщения ##

### Список входящих сообщений ###

**Запрос**

GET `/api/messages`
или
GET `/api/messages/incoming`

**Параметры**

`before` — уникальный числовой идентификатор записи `uid`, от которой нужно начать выборку. Не включает запись.

Необходима аутентификация.

**Ответ**

Пример запроса:
```
http://point.im/api/messages/incoming
```
Структура ответа аналогична `/api/recent`

### Список исходящих сообщений ###

**Запрос**

GET `/api/messages/outgoing`

**Параметры**

`before` — уникальный числовой идентификатор записи `uid`, от которой нужно начать выборку. Не включает запись.

Необходима аутентификация.

**Ответ**

Пример запроса:
```
http://point.im/api/messages/outgoing
```
Структура ответа аналогична `/api/recent`



## Просмотр поста ##

**Запрос**

GET `/api/post/<post>`

**Ответ**

Пример запроса:
```
http://point.im/api/post/llq
```
Пример ответа:
```js
{
  /* Объект поста */
  "post": {
    /* Массив тегов */
    "tags": [
      "point",
      "log",
      "web",
      "dev",
      "CSS",
      "point+"
    ],
    /* Количество комментариев */
    "comments_count": 5,
    /* Объект юзера, который написал пост */
    "author": {
      /* Логин автора поста */
      "login": "skobkin-ru",
      /* ID автора поста */
      "id": 230,
      /* Аватарка автора */
      "avatar": "skobkin-ru.jpg?r=5527",
      /* Имя автора поста */
      "name": "\u0410\u043b\u0435\u043a\u0441\u0435\u0439 Skobkin.ru"
    },
    /* Текст поста */
    "text": "\u0412\u0435\u0440\u043e\u043b\u043e\u043c\u043d\u043e, \u0431\u0435\u0437 \u043e\u0431\u044a\u044f\u0432\u043b\u0435\u043d\u0438\u044f \u0432\u043e\u0439\u043d\u044b, @arts \u043f\u043e\u0447\u0438\u043d\u0438\u043b \u043f\u043e\u043b\u044c\u0437\u043e\u0432\u0430\u0442\u0435\u043b\u044c\u0441\u043a\u0438\u0435 CSS \u0432 \u043f\u0440\u043e\u0444\u0430\u0439\u043b\u0435. \u0422\u0435\u043f\u0435\u0440\u044c \u043c\u043e\u0436\u043d\u043e \u0437\u0430\u043f\u0438\u043b\u0438\u0442\u044c \u0441\u0435\u0431\u0435 \u0440\u0435\u0437\u0438\u043d\u043e\u0432\u044b\u0439 \u043f\u043e\u0438\u043d\u0442 \u0434\u0430\u0436\u0435 \u0431\u0435\u0437 \u0440\u0430\u0441\u0448\u0438\u0440\u0435\u043d\u0438\u044f. \u041d\u0435\u0442, \u043a\u043e\u043d\u0435\u0447\u043d\u043e, \u0440\u0430\u0441\u0448\u0438\u0440\u0435\u043d\u0438\u0435 \u0434\u0435\u043b\u0430\u0435\u0442 \u0432\u0435\u0431\u043c\u043e\u0440\u0434\u0443 \u0437\u043d\u0430\u0447\u0438\u0442\u0435\u043b\u044c\u043d\u043e \u043a\u0440\u0443\u0447\u0435, \u043d\u043e \u0432\u0441\u0451 \u0440\u0430\u0432\u043d\u043e, \u0443\u0436\u0435 \u0432\u0435\u0441\u0435\u043b\u0435\u0435. \u041e\u0441\u043e\u0431\u0435\u043d\u043d\u043e \u0442\u0435\u043c, \u0443 \u043a\u043e\u0433\u043e \u043d\u0435 \u0437\u0430\u043f\u0443\u0441\u043a\u0430\u044e\u0442\u0441\u044f crx \u0438 nex \u0440\u0430\u0441\u0448\u0438\u0440\u0435\u043d\u0438\u044f.",
    /* Дата написания */
    "created": "2014-03-29T16:56:47.116799",
    /* Тип поста */
    "type": "post",
    /* Символьный идентификатор (используется в URL) */
    "id": "llq",
    /* Приватный ли пост */
    "private": false
  },
  /* Массив комментариев */
  "comments": [
    {
      /* Дата и время написания комментария */
      "created": "2014-03-29T16:57:35.688586",
      /* Текст */
      "text": "@skobkin-ru, \u0430 \u043a\u0438\u043d\u044c \u0441\u0432\u043e\u0439 CSS \u043f\u043e\u0433\u043e\u043d\u044f\u0442\u044c",
      /* Объект автора комментария */
      "author": {
        /* Логин автора комментария */
        "login": "dzhon",
        /* ID автора комментария */
        "id": 27,
        /* Аватарка автора комментария */
        "avatar": "dzhon.jpg?r=8839",
        /* Имя автора комментария */
        "name": "dzhon"
      },
      /* В каком посте оставлен этот комментарий */
      "post_id": "llq",
      /* Параметр id комментария, на который написан ответ. null - значит комментарий не является ответом (комментарий первого уровня) */
      "to_comment_id": null,
      /* Является ли комментарий рекомендацией */
      "is_rec": false,
      /* Идентификатор комментария в посте (уникален только в пределах родительского поста) */
      "id": 1
    },
    {
      /* Следующий комментарий */
    }
  ]
}
```

## Создание поста ##

**Запрос**

POST `/api/post`

**Параметры**

`text` — обязательный

`tag`

`private`

**Примечание**

В этом и других запросах, связанных с изменением контента, необходимо передавать HTTP-зоголовок:

`X-CSRF: <csrf_token>`

где `csrf_token` — токен, получаемый при [аутентификации](#_1).

## Редактирование поста ##

**Запрос**

PUT `/api/post/<id>`

**Параметры**

`text` — обязательный

`tag`

## Удаление поста ##

**Запрос**

DELETE `/api/post/<id>`

## Создание комментария ##

**Запрос**

POST `/api/post/<id>`

**Параметры**

`text` — обязательный

`comment_id`

## Удаление комментария ##

**Запрос**

DELETE `/api/post/<id>/<comment_id>`

## Рекомендация поста ##

**Запрос**

POST `/api/post/<id>/r`

**Параметры**

`text`

## Удаление рекомендации поста ##

DELETE `/api/post/<id>/r`

## Рекомендация комментария ##

**Запрос**

POST `/api/post/<id>/<comment_id>/r`

**Параметры**

`text`

## Удаление рекомендации комментария ##

DELETE `/api/post/<id>/<comment_id>/r`

## Подписка на пост ##

POST `/api/post/<id>/s`

## Отписка от поста ##

DELETE `/api/post/<id>/s`

## Добавление поста в закладки ##

POST `/api/post/<id>/b`

`text` — пометка к закладке

## Удаление поста из закладок ##

DELETE `/api/post/<id>/b`

## Добавление комментария в закладки ##

POST `/api/post/<id>/<comment_id>/b`

`text` — пометка к закладке

## Удаление комментария из закладок ##

DELETE `/api/post/<id>/<comment_id>/b`

# Теги #

### Список тегов пользователя ###

**Запрос**

GET `/api/tags/<login>`

**Ответ**

Примеры запроса:
```
http://point.im/api/tags/skobkin-ru
```
Пример ответа:
```js
[
  /* Объект тега */
  {
    /* Количество постов с этим тегом у пользователя */
    "count": 9,
    /* Текст тега */
    "tag": "?"
  },
  {
    /* Следующий тег */
  }
]
```

### Выборка постов по тегам ###

**Запрос**

GET `/api/tags`

**Параметры**

`tag` — тег. Обязательный параметр.

Можно указывать несколько тегов.

**Ответ**

Пример запроса:
```
http://point.im/api/tags?tag=guitar
```
или
```
http://point.im/api/tags/skobkin-ru?tag=guitar&tag=test
```
Структура ответа аналогична `/api/recent`

### Выборка постов по тегам пользователя ###

**Запрос**

GET `/api/tags/<login>`

**Параметры**

`tag` — тег. Обязательный параметр.

Можно указывать несколько тегов.

**Ответ**

Примеры запросов:
```
http://point.im/api/tags/skobkin-ru?tag=guitar
```
или
```
http://point.im/api/tags/skobkin-ru?tag=guitar&tag=test
```
Структура ответа аналогична `/api/recent`

# Пользователи #

### Информация о пользователе ###

**Запрос**
Существует два вида запросов к API для получения информации о пользователе:

*   по имени пользователя:

    GET `/api/user/login/<login>`

    **Параметры**
    
    `login` — имя пользователя. Обязательный параметр.

    Пример запроса:
    ```
    http://point.im/api/user/login/skobkin-ru
    ```

* по `id` пользователя:

    GET `/api/user/id/<user_id>`

    **Параметры**

    `user_id` — `id` пользователя. Обязательный параметр.

    Пример запроса:
    ```
    http://point.im/api/user/id/230
    ```

**Ответ**

Ответ на оба типа запроса одинаков. Пример ответа:

```js
{
  /* Заметка пользователя о себе */
  "about": "PHP, guitar, World of Tanks",
  /* Jabber ID */
  "xmpp": "x@skobkin.ru",
  /* Имя (не логин) */
  "name": "\u0410\u043b\u0435\u043a\u0441\u0435\u0439 Skobkin.ru",
  /* Статус подписки на пользователя */
  "subscribed": false,
  /* Дата и время регистрации пользователя */
  "created": "2014-02-03T08:47:45.109228+00:00",
  /* Находится ли пользователь в BL */
  "bl": false,
  /* Пол пользователя (мужской - true; женский - false; робот/Угнич - null) */
  "gender": true,
  /* Находится ли пользователь в WL */
  "wl": false,
  /* Дата рождения */
  "birthdate": "1991-08-09T00:00:00",
  /* ID пользователя */
  "id": 230,
  /* Статус подписки на рекомендации пользователя */
  "rec_sub": false,
  /* Имя файла аватара пользователя */
  "avatar": "skobkin-ru.jpg?r=5527",
  /* Skype пользователя */
  "skype": "skobkin-ru",
  /* Логин пользователя */
  "login": "skobkin-ru",
  /* ICQ пользователя */
  "icq": "",
  /* Сайт пользователя */
  "homepage": "http://skobkin.ru/",
  /* E-mail пользователя */
  "email": "x@skobkin.ru",
  /* Город пользователя */
  "location": "\u0410\u0440\u0445\u0430\u043d\u0433\u0435\u043b\u044c\u0441\u043a"
}
```

### Информация о себе ###

Частным случаем информации о пользователе является получение информации 
авторизованным пользователем о себе самом. 

**Запрос**
```
/api/me
```
**Ответ**
```js
{
  /* Заметка пользователя о себе */
  "about": "PHP, guitar, World of Tanks",
  /* Jabber ID */
  "xmpp": "x@skobkin.ru",
  /* Имя (не логин) */
  "name": "\u0410\u043b\u0435\u043a\u0441\u0435\u0439 Skobkin.ru",
  /* Дата и время регистрации пользователя */
  "created": "2014-02-03T08:47:45.109228+00:00",
  /* Пол пользователя (мужской - true; женский - false; робот/Угнич - null) */
  "gender": true,
  /* Дата рождения */
  "birthdate": "1991-08-09T00:00:00",
  /* Имя файла аватара пользователя */
  "avatar": "skobkin-ru.jpg?r=5527",
  /* Skype пользователя */
  "skype": "skobkin-ru",
  /* ICQ пользователя */
  "icq": "",
  /* Сайт пользователя */
  "homepage": "http://skobkin.ru/",
  /* E-mail пользователя */
  "email": "x@skobkin.ru",
  /* Город пользователя */
  "location": "\u0410\u0440\u0445\u0430\u043d\u0433\u0435\u043b\u044c\u0441\u043a"
```

Если пользователь не авторизован, ответ сервера имеет вид:

```js
{
  "error": "NotAuthorized"
}
```

### Аватар пользователя ###

**Запрос**
```
http(s)://i.point.im/a/<size>/<avatar_name.ext>
```

**Параметры**

`size` — размер аватара: 24, 40, 80

`avatar_name.ext` — имя файла аватара (параметр `avatar`), получаемое в объектах пользователей.

**Ответ**

Примеры запросов:

```
http://i.point.im/a/24/skobkin-ru.jpg
```
Ответ:

![24](http://i.point.im/a/24/skobkin-ru.jpg "24")


```
http://i.point.im/a/40/skobkin-ru.jpg
```
Ответ:

![40](http://i.point.im/a/40/skobkin-ru.jpg "40")


```
http://i.point.im/a/80/skobkin-ru.jpg
```
Ответ:

![80](http://i.point.im/a/80/skobkin-ru.jpg "80")

### Получение аватара по имени пользователя ###

**Запрос**

GET `/avatar/<login>/<size>`

**Параметры**

login - имя пользователя
size - размер аватара: 24, 40, 80

**Ответ**

Сервер произведет перенаправление по адресу, соответствующему аватару пользователя. Например, запрос `http://point.im/avatar/skobkin-ru/80` будет перенаправлен на `http://i.point.im/a/80/skobkin-ru.jpg?r=5527`
 
### Подписки пользователя ###

**Запрос**

GET `/api/user/<login>/subscriptions`

**Ответ**

Пример запроса:
```
http://point.im/api/user/skobkin-ru/subscriptions
```
Пример ответа:
```js
[
  /* Объект пользователя */
  {
    /* Логин пользователя */
    "login": "al1k",
    /* ID пользователя */
    "id": 171,
    /* Имя файла аватара пользователя */
    "avatar": "al1k.jpg?r=8993",
    /* Имя (не логин) */
    "name": "al1k"
  },
  {
    /* Следующий пользователь */
  }
]
```

### Подписчики пользователя ###

**Запрос**

GET `/api/user/<login>/subscribers`

Необходима аутентификация.

**Ответ**

Пример запроса:
```
http://point.im/api/user/skobkin-ru/subscribers
```
Структура ответа аналогична `/api/user/<login>/subscriptions`


### Белый список пользователей ###

**Запрос**

GET `/api/user/wl`

Необходима аутентификация.

**Ответ**

Пример запроса:
```
http://point.im/api/user/wl
```
Структура ответа аналогична `/api/user/<login>/subscriptions`

### Чёрный список пользователей ###

**Запрос**

GET `/api/user/bl`

Необходима аутентификация.

**Ответ**

Пример запроса:
```
http://point.im/api/user/bl
```
Структура ответа аналогична `/api/user/<login>/subscriptions`

# WebSocket #

Для получения уведомлений о событиях можно использовать WebSocket-сервер, расположенный по адресу

`ws://point.im/ws` или `wss://point.im/ws`

Для аутентификации нужно передать WS-серверу сообщение вида:

`Authorization: <token>`,

где token — ключ, полученный при входе через API.
