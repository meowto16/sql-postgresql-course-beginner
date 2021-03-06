# Домашняя работа 1. Подзапросы

1. Вывести продукты количество которых в продаже меньше самого 
   малого среднего количества продуктов в деталях заказов 
   (группировка по `product_id`). Результирующая таблица 
   должна иметь колонки `product_name` и `units_in_stock`.
```sql
SELECT product_name, units_in_stock
FROM products
WHERE units_in_stock < (
	SELECT AVG(quantity) as average_quantity
	FROM order_details
	GROUP BY product_id
	ORDER BY average_quantity
	LIMIT 1
)
ORDER BY units_in_stock
```

2. Напишите запрос, который выводит общую сумму фрахтов 
   заказов для компаний-заказчиков для заказов, стоимость 
   фрахта которых больше или равна средней величине 
   стоимости фрахта всех заказов, а также дата отгрузки 
   заказа должна находится во второй половине июля 1996 года. 
   Результирующая таблица должна иметь колонки `customer_id`
   и `freight_sum`, строки которой должны быть 
   отсортированы по сумме фрахтов заказов.
```sql
SELECT customer_id, SUM(o.freight) as freight_sum
FROM customers as c
JOIN orders as o USING(customer_id)
WHERE o.shipped_date BETWEEN '1996-06-16' AND '1996-06-30'
GROUP BY customer_id
HAVING SUM(o.freight) >= (
	SELECT AVG(freight)
	FROM orders
)
ORDER BY freight_sum;
```

3. Напишите запрос, который выводит 3 заказа с наибольшей 
   стоимостью, которые были созданы после 1 сентября 1997 года 
   включительно и были доставлены в страны Южной Америки. 
   Общая стоимость рассчитывается как сумма стоимости 
   деталей заказа с учетом дисконта. Результирующая 
   таблица должна иметь колонки customer_id, ship_country и 
   order_price, строки которой должны быть отсортированы по 
   стоимости заказа в обратном порядке.
```sql
SELECT o.customer_id, 
       o.ship_country, 
	   od.unit_price * od.quantity * (1 - od.discount) AS order_price
FROM orders as o
JOIN order_details as od USING(order_id)
WHERE o.order_date > '1997-09-01'
AND ship_region = ANY(
    SELECT state_abbr
	FROM us_states
	WHERE state_region = 'south'
)
ORDER BY order_price DESC

```
4. Вывести все товары (уникальные названия продуктов), 
   которых заказано ровно 10 единиц (конечно же, 
   это можно решить и без подзапроса).

```sql
SELECT DISTINCT product_name
FROM products AS p
JOIN order_details AS od USING(product_id)
WHERE od.quantity = 10
```

Или используя подзапрос
```sql
SELECT DISTINCT product_name
FROM products as p
WHERE EXISTS (
  SELECT product_id
  FROM order_details as od
  WHERE p.product_id = od.product_id
  AND od.quantity = 10
)
```
