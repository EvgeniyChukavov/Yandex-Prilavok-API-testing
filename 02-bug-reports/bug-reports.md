# Баг-репорты — API Яндекс.Прилавок

**Инструмент:** Postman
**Версия API:** v3.1.1

| ID | Название | Приоритет | Метод и ручка | ОР | ФР |
| :--- | :--- | :--- | :--- | :--- | :--- |
| BUG1 | Ответ 200 OK при добавлении несуществующего продукта (id=999999) в набор | Стандартный | POST /api/v1/kits/{id}/products | 404 Not Found | 200 OK |
| BUG2 | Ответ 200 OK при превышении лимита (31-й продукт) | Критический | POST /api/v1/kits/{id}/products | 400 Bad Request | 200 OK |
| BUG3 | Ответ 200 OK при quantity = 0 | Стандартный | POST /api/v1/kits/{id}/products | 400 Bad Request | 200 OK |
| BUG4 | Ответ 200 OK при quantity = -1 | Стандартный | POST /api/v1/kits/{id}/products | 400 Bad Request | 200 OK |
| BUG5 | Ответ 200 OK при пустом productsList | Стандартный | POST /api/v1/kits/{id}/products | 400 Bad Request | 200 OK |
| BUG6 | Ответ 200 OK с пустым XML при deliveryTime = 6 | Желательный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 200 OK, isItPossibleToDeliver=false | 200 OK, XML пустой |
| BUG7 | Ответ 200 OK с пустым XML при deliveryTime = 22 | Желательный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 200 OK, isItPossibleToDeliver=false | 200 OK, XML пустой |
| BUG8 | Ответ isItPossibleToDeliver=true при productsWeight = 6.1 кг | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | isItPossibleToDeliver=false | isItPossibleToDeliver=true |
| BUG9 | Ответ 200 OK при productsWeight = -0.1 кг | Желательный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | isItPossibleToDeliver=false | 200 OK, hostDeliveryCost=23 |
| BUG10 | Ответ isItPossibleToDeliver=true при productsCount = 15 шт. | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | isItPossibleToDeliver=false, clientDeliveryCost=99 | isItPossibleToDeliver=true |
| BUG11 | Ответ 500 при невалидном XML (нет закрывающего тега) | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 500 Internal Server Error |
| BUG12 | Ответ 500 при пустом XML | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 500 Internal Server Error |
| BUG13 | Ответ 200 OK при productsCount = «три» | Желательный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 200 OK |
| BUG14 | Ответ 500 при отсутствии обязательного параметра | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 500 Internal Server Error |
| BUG15 | Ответ 500 при Content-Type = text/plain | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 500 Internal Server Error |
| BUG16 | Продукт не добавляется при id=3 | Критический | PUT /api/v1/orders/{id} | 200 OK, продукт появился | 200 OK, продукта нет |
| BUG17 | Ответ 404 при удалении существующей корзины | Критический | DELETE /api/v1/orders/{id} | 200 OK, {«ok»: true} | 404 Not Found |
| BUG18 | Ответ 200 OK при quantity = 0 | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 200 OK |
| BUG19 | Ответ 200 OK с уменьшением количества при quantity = -1 | Критический | PUT /api/v1/orders/{id} | 400 Bad Request | 200 OK, количество уменьшилось |
| BUG20 | Ответ 200 OK при id продукта = 0 | Стандартный | POST /api/v1/kits/{id}/products | 404 Not Found | 200 OK |
| BUG21 | Ответ 500 при отсутствии ключа quantity | Стандартный | POST /api/v1/kits/{id}/products | 400 Bad Request | 500 Internal Server Error |
| BUG22 | Ответ 200 OK при отсутствии поля id | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 200 OK |
| BUG23 | Ответ 200 OK с quantity = null при отсутствии quantity | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 200 OK |
| BUG24 | Ответ 500 при productsCount = 0, null, «два», -5, >2147483647 | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 500 Internal Server Error |
| BUG25 | Ответ 200 OK с quantity = 0 при quantity = null или id = @#$ | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 200 OK |
| BUG26 | Ответ 500 при productsList как объект | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 500 Internal Server Error |
| BUG27 | Ответ 500 при id корзины = «two» (строка) | Стандартный | GET /api/v1/orders/{id} | 400 Bad Request | 500 Internal Server Error |
| BUG28 | Ответ 500 при productsList как объект | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 500 Internal Server Error |
| BUG29 | Ответ 500 при id продукта = «abc», -5, 2147483647 | Стандартный | POST /api/v1/kits/{id}/products | 400 Bad Request | 500 Internal Server Error |
| BUG30 | Ответ 500 при id продукта = «abc», «@#$», 2147483648 | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 500 Internal Server Error |
| BUG31 | Ответ 409 при id продукта = null | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 409 Conflict |
| BUG32 | Ответ 200 OK при quantity = «два», 2147483648, null | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 200 OK |
| BUG33 | Ответ 409 при отсутствии ключа id | Стандартный | PUT /api/v1/orders/{id} | 400 Bad Request | 409 Conflict |
| BUG34 | Ответ 200 OK при quantity = 100 (превышение остатков) | Стандартный | PUT /api/v1/orders/{id} | 409 Conflict | 200 OK |
| BUG35 | Ответ 200 OK с заменой количества при повторном добавлении | Стандартный | PUT /api/v1/orders/{id} | 200 OK, товар увеличился | 200 OK, товар не увеличился |
| BUG36 | Ответ 500 при productsWeight = «два», -0.5, 2147483648, 0, null | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 500 Internal Server Error |
| BUG37 | Ответ 500 при deliveryTime = «да», -5, 2147483648, null | Стандартный | POST /fast-delivery/v3.1.1/calculate-delivery.xml | 400 Bad Request | 500 Internal Server Error |

## Итоги

| Приоритет | Количество |
| :--- | :--- |
| Критический | 4 |
| Стандартный | 26 |
| Желательный | 7 |
| **Всего** | **37** |
