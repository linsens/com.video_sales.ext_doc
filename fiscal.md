# Работа с фискализацией

* [Получение информации для запроса на фискализацию](#Получение-информации-для-запроса-на-фискализацию)
* [Получение заказов](#Получение-заказов)
* [Получение состава чека](#Получение-состава-чека)
* [Создание тестового заказа](#Создание-тестового-заказа)


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
client_id | Идентификатор клиента в iBonus. Необязательынй параметр.
receipt_type | Тип чека. sale, refund, payout. Необязательынй параметр.
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
    "customerName": "Vasya",
    "customerAvatarUrl": "https://test.com/url/image.png",
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

## Получение состава чека

* **URL**

`/ext/api/v1/fiscal/orders/{id}/items/`

* **Метод**

`GET`

* **URL параметры**

Параметр | Описание
-------- | --------
{id} | id заказа в системе iBonus.

* **Параметры данных**

Отсутствуют

* **Пример ответа**

```json
[{
    "id": 12,
    "orderId": 1235,
    "name" : "Кофе",
    "price" : 100.5,
    "quantity": 2,
    "unit": "asd",
    "vat": 0.5,
    "discountType": "791234567890",
    "discountValue": 0
}]
     
```

* **Описание параметров ответа**

Название поля | Описание
------------- | --------
id | Id товара внутри системы.
orderId | Номер заказа внутри системы.
name | Название товара.
price | Цена товара.
quantity | Кол-во товара.
unit | В чем измеряется кол-во товара. piece, kg, g, L, ml
vat | Размер НДС
discountType | Тип указанной скидки. percent - в discountValue передан размер в процентах, amount - в discountValue передана сумма скидки
discountValue | Размер скидки

## Создание тестового заказа

Не сомтря на то, что заказ "тестовый", квитанция будет выслана на указанные адреса

* **URL**

`/ext/api/v1/fiscal/orders/create_test/`

* **Метод**

`POST`

* **URL параметры**

Отсутствуют

* **Параметры данных**

```json
{
    "receiptType": "sale", 
    "amountCard": 20,
    "amountCash": 10,
    "customerEmail": "test@test.com",
    "customerPhone": "791234567890",
    "externalId": "12345678",
    "items": [{
        "price": 10, 
        "quantity": 1, 
        "unit": "piece", 
        "vat": 20, 
        "name": "Капучино"
    }]
}

     
```

* **Описание параметров запроса**

Название поля | Описание
------------- | --------
externalId | Номер заказа во внешней системе.
receiptType | Тип чека. sale, refund.
amountCard | Сумма, оплаченная картой. Необязательный параметр.
amountCash | Сумма, оплаченная наличными. Необязательный параметр.
customerEmail | Email покупателя. Необязательный параметр.
customerPhone | Номер телефона покупателя. Необязательный параметр.
items.price | Цена товара
items.quantity | Количество товара
items.unit | В чем измеряется кол-во товара. piece, kg, g, L, ml 
items.vat | Размер НДС
items.name | Название товара

* **Пример ответа**

200 OK

## Создание заказа (пока не в проде)

* **URL**

`/ext/api/v1/fiscal/orders/`

* **Метод**

`POST`

* **URL параметры**

Отсутствуют

* **Параметры данных**

```json
{
  "order": {
     "receiptType": "sale", 
     "amountCard": 20,
     "amountCash": 10,
     "customerEmail": "test@test.com",
     "customerPhone": "791234567890",
     "externalId": "12345678",
     "items": [{
       "price": 10, 
         "quantity": 1, 
         "unit": "piece", 
         "vat": 20, 
         "name": "Капучино"
     }]
  },
  "amount": 10,
  "currency": "points",
  "coupons": [1, 2]
}

     
```

* **Описание параметров запроса**

***Поля запроса***

Название поля | Описание
------------- | --------
order | Описание заказа, обязательное поле
amount | Какую сумму надо списать с бонусного счета клиента
currency | Валютаа бонусного счета
coupons | Какие купоны надо погасить при добавлении заказа

***Поля order***

Название поля | Описание
------------- | --------
externalId | Номер заказа во внешней системе.
receiptType | Тип чека. sale, refund. Если будет передан refund, то ранее начисленные бонусы купоны будут отменены, а ранее списанны бонусы будут возвращены
amountCard | Сумма, оплаченная картой. Необязательный параметр.
amountCash | Сумма, оплаченная наличными. Необязательный параметр.
customerEmail | Email покупателя. Необязательный параметр.
customerPhone | Номер телефона покупателя. Необязательный параметр.
items.price | Цена товара
items.quantity | Количество товара
items.unit | В чем измеряется кол-во товара. piece, kg, g, L, ml 
items.vat | Размер НДС
items.name | Название товара

* **Пример ответа**

200 OK