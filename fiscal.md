# Работа с фискализацией

* [Получение информации для запроса на фискализацию](#Получение-информации-для-запроса-на-фискализацию)
* [Получение заказов](#Получение-заказов)

## Получение информации для запроса на фискализацию
* **URL**

`/ext/api/v1/fiscal/auth/`

* **Метод**

`GET`

* **URL параметры**

Отсутствуют

* **Параметры данных**

Отсутствуют

* **Пример ответа**

```json
{
    "accountType" : "employee",
    "email" : "test@test.ru",
    "firstName" : "Cashier",
    "fiscalApiKey" : "2965f5329e0e97da957ca88c4ada5578",
    "fiscalLogin": "79123456789",    
    "inn" :  "123 123 123 321",
    "lastName" : "Employee",
    "role" : "cashier"
}
     
```

* **Описание параметров ответа**

Название поля | Описание
------------- | --------
accountType | Строка 'merchant' или 'employee'
email | Email пользователя. Может быть null
firstName | Имя пользователя. Может быть null
fiscalApiKey | API ключ, который необходимо передавать при запросе на фискальный хаб. HEX, максимум 256 символов
fiscalLogin | Логин мерчанта, который необходимо передавать при запросе на фискальный хаб. Строка, максимум 64 символа
inn | ИНН пользователя. Может быть null
lastName | Фамилия пользователя. Может быть null
role | Роль пользователя. Строка 'cashier' или 'manager'. Передается только, если accountType == 'employee'. Может быть null.


## Получение заказов

Метод для получения списка всех заказов, которые оформил пользователь, переданный в авторизации. Если пользователь - "мерчант", то будут переданы все заказы мерчанта. Если пользователь - "кассир", то будут переданы только те заказы, которые оформил кассир. Заказы отсортированы по дате в обраном порядке.

* **URL**

`/ext/api/v1/fiscal/orders/`

* **Метод**

`GET`

* **URL параметры**

Параметр | Описание
-------- | --------
from | Timestamp. Время заказов будет больше, либо равно переданному значению. Необязательный параметр.
to | Timestamp. Время заказов будет меньше, либо равно переданному значению. Необязательный параметр.
fn | FN заказа. Строка. Часть строки может быть заменена на %. поиск осуществляется like.
fd | FD заказа. Строка. Часть строки может быть заменена на %. поиск осуществляется like.
offset | Число. Порядковый номер с которого будет произведена выдача(начинается с 0). Необязательный параметр.
limit | Число. Ограничение на кол-во записей в выдаче. Необязательный параметр.

* **Параметры данных**

Отсутствуют

* **Пример ответа**

```json
[{
    "cashbackAmount": 12.34,
    "cashbackCurrency": "RUB",
    "orderNumber" : "TEST-0001-00000069",
    "externalOrderNumber" : "12345678",
    "orderDate": 1548968400000,
    "externalOrderDate": 1548968400000,
    "receiptType": "sale",
    "customerPhone": "791234567890",
    "customerEmail": "customer@email.com",
    "customer": {
        "avatarUrl": null, 
        "averageBill": 407.79, 
        "email": "test@test.com", 
        "id": 1, 
        "name": "First Last", 
        "negativeReviews": 0, 
        "neutralReviews": 0, 
        "paidPurchases": 1, 
        "paidPurchasesAmount": null, 
        "phone": "+71234567890", 
        "positiveReviews": 0, 
        "token": "HXbvvQzHbPlrV9Bh2iZNw6j1Mu2E6Hr", 
        "totalPurchases": 5, 
        "totalPurchasesAmount": null
    },
    "employee": {
        "email": "manager@gmail.com", 
        "firstName": "Manager", 
        "id": 1, 
        "inn": "1234567890", 
        "lastName": "First", 
        "login": "923217", 
        "role": "manager"
    }, 
    "publicURL": "/internal_fiscal_check/xiar06JNfTB1N3N0Jq3k9CNoTiR3mY21/",
    "reaction": "positive",
    "customerComment": "Comment",
    "totalAmount": 123.45,
    "status": "printing"
}]
     
```

* **Описание параметров ответа**

Название поля | Описание
------------- | --------
orderNumber | Номер заказа внутри системы.
externalOrderNumber | Номер заказа переданный мерчантов.
orderDate | Дата заказа внутри системы.
externalOrderDate | Дата заказа переданная мерчантом.
receiptType | Строка. "sale" - оплата или "refund" - возврат.
customerPhone | Телефон покупателя, который был указан для заказа. Может быть null.
customerEmail | Email покупателя, который был указан для заказа. Может быть null.
totalAmount | Сумма заказа.
status | Строка. "inQueue" - в очереди на печать, "printing" - печатается, "printed" - успешно распечатан, "error" - ошибка печати.
reaction | Реакция пользователя. negative, neutral, positive. Может быть null.