# Домашнее задание 2. Простые выборки

1. Выбрать все заказы из стран France, Austria, Spain
```postgresql
SELECT *
FROM orders
WHERE ship_country IN('France', 'Austria', 'Spain')
```
2. Выбрать все заказы, отсортировать по required_date (по убыванию) и отсортировать по дате отгрузке (по возрастанию)
```postgresql
SELECT *
FROM orders
ORDER BY required_date DESC, shipped_date ASC
```
3. Выбрать минимальное кол-во единиц товара среди тех продуктов, которых в продаже более 30 единиц.
```postgresql
SELECT MIN(units_in_stock)
FROM products
WHERE units_in_stock > 30
```
4. Выбрать максимальное кол-во единиц товара среди тех продуктов, которых в продаже более 30 единиц.
```postgresql
SELECT MAX(units_in_stock)
FROM products
WHERE units_in_stock > 30
```
5. Найти среднее значение дней уходящих на доставку с даты формирования заказа в USA
```postgresql
SELECT AVG(shipped_date - order_date)
FROM orders
WHERE ship_country = 'USA'
```
6. Найти сумму, на которую имеется товаров (кол-во * цену) причём таких, которые планируется продавать и в будущем (см. на поле discontinued)
```postgresql
-- discontinued - снято с производства
SELECT SUM(unit_price * units_in_stock)
FROM products
WHERE discontinued = 0
```
