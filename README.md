# Интеграция с video_sales.com

## Введение

Для интеграции с video_sales.com используется REST API. Используемая кодировка UTF-8. Текущий url для отладки [https://sale-video.com/](https://sale-video.com/) или [https://lk.i-bonus.me/](https://lk.i-bonus.me/)

## Авторизация

Все завпросы выполняются с авторизацией. Авторизация производится с помощью Basic Auth. Для этого в каждом запросе к REST API необходимо передавать заголовок Authorization. Подробнее [Wiki](https://en.wikipedia.org/wiki/Basic_access_authentication#Client_side). 

## Формат данных

Все ответы возвращаются в формате JSON. Кодировка UTF-8.

## Коды ответов

Коды ответа | Описание
----------- | --------
2xx | Запрос выполнен успешно
4xx | Ошибка в данных передаваемых в запросе, либо в самом запросе
5xx | Ошибка на серверной стороне

## Формат вывода ошибок
В случае возникновения какой либо ошибки в ответ возвращается json с описанием самой ошибки.

```json
{
    "error": "Not Found", 
    "message": "Video with id=1 is not found.", 
    "path": "/ext/api/v1/videos/1/generate_link/", 
    "status": 404, 
    "timestamp": 1513521246746
}
```

## [Работа с клиентам](clients.md)

## [Работа с заказами, фискализацией](fiscal.md)

## [Погашение расшаренных купонов](coupons.md)

## [Списание баллов](fiscal_cash_back.md)

## [Настройка системы лояльности](loyalty.md)

## [Регистрация мерчанта](merchant.md)

## [Работа с видео](videos.md)

## [Работа с плейлистами](play_lists.md)

## [Работа со страницами оплаты](promo_pages.md)
