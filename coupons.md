# Работа с клиентами

* [Погашение расшаренного купона](#Погашение-расшаренного-купона)

## Погашение расшаренного купона
* **URL**

`/ext/api/v1/coupons/share/redeem/`

* **Метод**

`PUT`

* **URL параметры**

Название поля | Описание
------------- | --------
phone | Номер телефона. Необязательный параметр. Одновременно email и номер телефона не могут отсутствовать.
email | Email клиента. Необязательный параметр. Одновременно email и номер телефона не могут отсутствовать.
name | Имя калиента. Необязательный параметр.
token | Токен расшаривания купона

* **Параметры данных**

Отсутствуют

* **Пример ответа**
```
{
    "client" : {
        "id" : 1234567,
        "email" : "test@test.ru",
        "phone" : "7912345678",
        "name" : "Client First",
        "avatarUrl": "https://test.com/image/123.jpg",
        "paidPurchases" : 1,
        "totalPurchases" : 2,
        "averageBill" : 100.5,
        "totalPurchasesAmount" : 1234.4,
        "paidPurchasesAmount": 123.5,
        "positiveReviews": 1,
        "neutralReviews": 0,
        "negativeReviews": 2,
        "token": "12345667890abcdef"
    },
    "coupons" : {
        "id": 123,
        "title":"Internal title",
        "header":"Заголовок",
        "subHeader": "Подзаголовок",
        "shortDescription": "Короткое описание",
        "longDescription" : "Длинное описание",
        "expireDate": 1571408400000,
        "token" : "12345667890abcdef",
        "shareToken" :  "12345667890abcdef",
        "merchantToken": "123456",
        "merchantShareToken" : "654321"
    }
}
     
```

* **Описание параметров ответа**

см. получения купона клиента