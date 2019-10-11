# Работа с фискализацией

* [Создание hold для списания бонусов](#Создание-hold-для-списания-бонусов)
* [Списание hold](#Списание-hold)
* [Отмена hold](#Отмена-hold)
* [Возврат](#Возврат)

## Создание hold для списания бонусов
* **URL**

`/ext/api/v1/fiscal_hub/cash_back/hold/`

* **Метод**

`POST`

* **URL параметры**

Название поля | Описание
------------- | --------
login | Логин пользователя. Email или номер телефона
api_key | API key мерчанта

* **Параметры данных**

```json
{
    "clientId" : 1,
    "currency" : "points",
    "amount" : 12.34,
    "couponIds": [1, 2]
}
    
```

Возможные значения currency - points, rubles

id для couponsIds берутся из [Получение купонов клиента](clients.md/#Получение-купонов-клиента)

* **Пример ответа**

```json
{
    "amount": 12.34, 
    "balanceId": 1, 
    "clientId": 1, 
    "currency": "points", 
    "expirationDate": 1572532181669, 
    "fiscalOrderId": null, 
    "id": 1, 
    "orderId": null, 
    "transactionDate": 1569940181669, 
    "transactionId": "1f1f1f1f-1f1f-1e1e-d1d1-1a1b1c1d1e1f00"
}
     
```

* **Описание параметров ответа**

Название поля | Описание
------------- | --------
transactionId | Идентификатор транзакции. По нему происходит списание и отмена

## Списание hold 
* **URL**

`/ext/api/v1/fiscal_hub/cash_back/hold/write_off/`

* **Метод**

`POST`

* **URL параметры**

Название поля | Описание
------------- | --------
login | Логин пользователя. Email или номер телефона
api_key | API key мерчанта

* **Параметры данных**

```json
{
    "transactionId": "1f1f1f1f-1f1f-1e1e-d1d1-1a1b1c1d1e1f00",
    "fiscalOrderId" : 1
}
    
```

* **Пример ответа**

200 ОК

## Отмена hold 
* **URL**

`/ext/api/v1/fiscal_hub/cash_back/hold/cancel/`

* **Метод**

`POST`

* **URL параметры**

Название поля | Описание
------------- | --------
login | Логин пользователя. Email или номер телефона
api_key | API key мерчанта

* **Параметры данных**

```json
{
    "transactionId": "1f1f1f1f-1f1f-1e1e-d1d1-1a1b1c1d1e1f00"
}
    
```

* **Пример ответа**

200 ОК

## Возврат 
* **URL**

`/ext/api/v1/fiscal_hub/cash_back/refund/`

* **Метод**

`POST`

* **URL параметры**

Название поля | Описание
------------- | --------
login | Логин пользователя. Email или номер телефона
api_key | API key мерчанта

* **Параметры данных**

```json
{
    "orderItems": [1, 2, 3]
}
    
```

Идентификаторы из базы

* **Пример ответа**

200 ОК