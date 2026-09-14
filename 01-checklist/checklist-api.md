# Чек-лист тестирования API Яндекс.Прилавок

**Версия API:** v3.1.1
**Инструмент:** Postman

---

## Работа с наборами: добавление продуктов (POST /api/v1/kits/{id}/products)

### ID продукта

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Проверка работы URL | 200 OK | Passed | |
| 2 | Добавить 1 продукт (id=1, quantity=1) в набор (id=2) | 200 OK | Passed | |
| 3 | Добавить несколько продуктов (2-3) | 200 OK | Passed | |
| 4 | Добавить 30 продуктов (productsCount = 30) | 200 OK | Passed | |
| 5 | Несуществующий id набора (999999) | 404 Not Found | Passed | |
| 6 | Несуществующий id продукта (999999) | 400 Bad Request | Failed | BUG1 |
| 7 | Превысить лимит (31-й продукт при 30) | 400 Bad Request | Failed | BUG2 |
| 8 | id продукта неверного формата (буквы) | 400 Bad Request | Failed | BUG29 |
| 9 | id продукта отрицательное (-5) | 400 Bad Request | Failed | BUG29 |
| 10 | id продукта очень большое (>2147483647) | 400 Bad Request | Failed | BUG29 |
| 11 | id продукта = null | 400 Bad Request | Passed | |

### Quantity

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 12 | Добавить продукт, который уже есть в наборе | 200 OK | Passed | |
| 13 | Добавить 30 уникальных продуктов | 400 Bad Request | Passed | |
| 14 | quantity = null | 400 Bad Request | Passed | |
| 15 | quantity = 0 | 400 Bad Request | Failed | BUG3 |
| 16 | quantity = -1 | 400 Bad Request | Failed | BUG4 |
| 17 | Пустой productsList ({«productsList»: []}) | 400 Bad Request | Failed | BUG5 |
| 18 | Невалидный JSON (лишняя запятая) | 400 Bad Request | Passed | |
| 19 | id = 0 | 404 Not Found | Failed | BUG20 |

### Структура запроса

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 20 | quantity = 1.5 (дробное) | 400 Bad Request | Failed | BUG21 |
| 21 | quantity = «два» (строка) | 400 Bad Request | Failed | BUG21 |
| 22 | Отсутствует ключ id | 400 Bad Request | Failed | BUG22 |
| 23 | Отсутствует ключ quantity | 400 Bad Request | Failed | BUG21 |
| 24 | quantity > 2147483647 | 400 Bad Request | Failed | BUG24 |

---

## Работа с курьерами «Привезём быстро» (POST /fast-delivery/v3.1.1/calculate-delivery.xml)

### productsCount

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 25 | Вес 0.1 кг, кол-во 1, время 7 ч | 200 OK, hostDeliveryCost=23 | Passed | |
| 26 | Вес 2.5 кг, кол-во 7, время 21 ч | 200 OK, hostDeliveryCost=23 | Passed | |
| 27 | Вес 0.1 кг, кол-во 8, время 15 ч | 200 OK, hostDeliveryCost=43 | Passed | |
| 28 | Вес 2.6 кг, кол-во 5, время 15 ч | 200 OK, hostDeliveryCost=43 | Passed | |
| 29 | Вес 0.1 кг, кол-во 7, время 15 ч | 200 OK, hostDeliveryCost=23 | Passed | |
| 30 | productsCount > 2147483647 | 400 Bad Request | Failed | BUG24 |
| 31 | productsCount = 0 | 400 Bad Request | Failed | BUG24 |
| 32 | productsCount = null | 400 Bad Request | Failed | BUG24 |
| 33 | productsCount буквами («два») | 400 Bad Request | Failed | BUG24 |
| 34 | productsCount отрицательное (-5) | 400 Bad Request | Failed | BUG24 |

### deliveryTime

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 35 | deliveryTime=6 (до начала рабочего дня) | 200 OK, isItPossibleToDeliver=false | Failed | BUG6 |
| 36 | Вес 0.1 кг, кол-во 7, время 15 ч | 200 OK, hostDeliveryCost=23 | Passed | |
| 37 | Вес 0.1 кг, кол-во 8, время 15 ч | 200 OK, hostDeliveryCost=43 | Passed | |
| 38 | Вес 0.1 кг, кол-во 14, время 15 ч | 200 OK, hostDeliveryCost=43 | Passed | |
| 39 | Вес 2.5 кг, кол-во 1, время 15 ч | 200 OK, hostDeliveryCost=23 | Passed | |
| 40 | Вес 2.6 кг, кол-во 1, время 15 ч | 200 OK, isItPossibleToDeliver=false | Passed | |
| 41 | deliveryTime=22 (после рабочего дня) | 200 OK, isItPossibleToDeliver=false | Failed | BUG7 |
| 42 | deliveryTime буквами | 400 Bad Request | Failed | BUG37 |
| 43 | deliveryTime отрицательное (-5) | 400 Bad Request | Failed | BUG37 |
| 44 | deliveryTime > 2147483647 | 400 Bad Request | Failed | BUG37 |
| 45 | deliveryTime = null | 400 Bad Request | | BUG37 |

### productsWeight

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 46 | Вес 2.5 кг (время 15:00) | 200 OK, hostDeliveryCost=23 | Passed | |
| 47 | Вес 2.6 кг (время 15:00) | 200 OK, hostDeliveryCost=43 | Passed | |
| 48 | Вес 6.0 кг (время 15:00) | 200 OK, hostDeliveryCost=43 | Passed | |
| 49 | Вес 6.1 кг (время 15:00) | isItPossibleToDeliver=false | Failed | BUG8 |
| 50 | Вес 0 кг (время 15:00) | 200 OK, hostDeliveryCost=23 | Passed | |
| 51 | Вес -0.1 кг (время 15:00) | isItPossibleToDeliver=false | Failed | BUG9 |
| 52 | productsWeight буквами | 400 Bad Request | Failed | BUG36 |
| 53 | productsWeight отрицательное (-0.5) | 400 Bad Request | Failed | BUG36 |
| 54 | productsWeight > 2147483647 | 400 Bad Request | Failed | BUG36 |
| 55 | productsWeight = 0 | 400 Bad Request | Failed | BUG36 |
| 56 | productsWeight = null | 400 Bad Request | Failed | BUG36 |

### Структура запроса

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 57 | Проверка работы URL | 200 OK | Passed | |
| 58 | Невалидный XML (нет закрывающего тега) | 400 Bad Request | Failed | BUG11 |
| 59 | Пустой XML | 400 Bad Request | Failed | BUG12 |
| 60 | productsCount строкой | 400 Bad Request | Failed | BUG13 |
| 61 | productsWeight строкой | 400 Bad Request | Failed | BUG13 |
| 62 | deliveryTime строкой | 400 Bad Request | Failed | BUG13 |
| 63 | Отсутствует обязательный параметр | 400 Bad Request | Failed | BUG14 |
| 64 | Невалидный Content-Type (text/plain) | 400 Bad Request | Failed | BUG15 |
| 65 | Невалидный метод (GET вместо POST) | 405 | Passed | |

### clientDeliveryCost

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 66 | Превышение максимального количества (15 шт.) | clientDeliveryCost = 99 | Failed | BUG10 |
| 67 | Нормальные значения | clientDeliveryCost = 0 | Passed | |

---

## Работа с корзиной: получение (GET /api/v1/orders/:id)

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 68 | Получение списка продуктов из корзины | 200 OK, productsList | Passed | |
| 69 | Несуществующая корзина (999999) | 404 Not Found | Passed | |
| 70 | Отсутствует ключ id в объекте продукта | 400 Bad Request | Failed | BUG23 |
| 71 | id продукта = 0 | 400 Bad Request | Passed | |
| 72 | quantity = null | 400 Bad Request | Failed | BUG25 |
| 73 | id корзины строкой two | 400 Bad Request | Failed | BUG27 |
| 74 | productsList как объект | 400 Bad Request | Failed | BUG26 |
| 75 | id корзины спецсимволами (@#$) | 400 Bad Request | | BUG25 |

---

## Работа с корзиной: добавление (PUT /api/v1/orders/:id)

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 76 | Увеличение количества продукта на 2 | 200 OK, кол-во увеличилось | Passed | |
| 77 | Добавление продукта с ID 3 | 200 OK, продукт появился | Failed | BUG16 |
| 78 | Несуществующий продукт (999999) | 409 Conflict | Passed | |
| 79 | quantity = 0 | 400 Bad Request | Failed | BUG18 |
| 80 | quantity = -1 | 400 Bad Request | Failed | BUG19 |
| 81 | Пустой productsList | 200 OK, ничего не меняется | Passed | |
| 82 | Невалидный JSON | 400 Bad Request | Passed | |
| 83 | Несуществующая корзина (999999) | 404 Not Found | Passed | |
| 84 | productsList невалидное (объект, строка, число, null) | 400 Bad Request | Failed | BUG28 |
| 85 | productsList строкой | 400 Bad Request | Failed | BUG28 |
| 86 | productsList числом | 400 Bad Request | Failed | BUG28 |
| 87 | productsList = null | 400 Bad Request | Failed | BUG28 |
| 88 | productsList отсутствует | 400 Bad Request | Failed | BUG28 |
| 89 | id продукта буквами («abc») | 400 Bad Request | Failed | BUG30 |
| 90 | id продукта спецсимволами (@#$) | 400 Bad Request | Failed | BUG30 |
| 91 | id продукта > 2147483647 | 400 Bad Request | Failed | BUG30 |
| 92 | id продукта = null | 400 Bad Request | Failed | BUG31 |
| 93 | quantity буквами («два») | 400 Bad Request | Failed | BUG32 |
| 94 | quantity > 2147483647 | 400 Bad Request | Failed | BUG32 |
| 95 | quantity = null | 400 Bad Request | Failed | BUG32 |
| 96 | Отсутствует ключ id | 400 Bad Request | Failed | BUG33 |
| 97 | Отсутствует ключ quantity | 400 Bad Request | Failed | BUG32 |
| 98 | Превышение остатков на складе | 409 Conflict | Failed | BUG34 |
| 99 | Повторное добавление товара (суммирование) | 200 OK | | BUG35 |

---

## Работа с корзиной: удаление (DELETE /api/v1/orders/:id)

| № | Описание | ОР | Статус | Баг |
| :--- | :--- | :--- | :--- | :--- |
| 100 | Удаление существующей корзины | 200 OK, {«ok»: true} | Failed | BUG17 |
| 101 | Удаление несуществующей корзины (999999) | 404 Not Found | Passed | |

---

## Итоги

| Статус | Количество |
| :--- | :--- |
| Passed | 64 |
| Failed | 37 |
| **Всего** | **101** |
