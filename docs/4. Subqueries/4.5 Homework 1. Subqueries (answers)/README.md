# Домашняя работа 1. Подзапросы (ответы)

1. (**Решено верно**) Вывести продукты количество которых в продаже меньше самого 
   малого среднего количества продуктов в деталях заказов 
   (группировка по `product_id`). Результирующая таблица 
   должна иметь колонки `product_name` и `units_in_stock`.
```sql
-- Преподаватель решил через ALL, но как по мне лучше LIMIT 1
-- Просто у меня было ощущение, что ALL имеет большую алгоритмическую сложность
-- А так тут сравнение идет только с одной строкой, которая приходит
-- Из подзапроса. В отличие от ALL, где сравнение идет постоянно со всеми строками
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

2. (**Решено неверно**) Напишите запрос, который выводит общую сумму фрахтов 
   заказов для компаний-заказчиков для заказов, стоимость 
   фрахта которых больше или равна средней величине 
   стоимости фрахта всех заказов, а также дата отгрузки 
   заказа должна находится во второй половине июля 1996 года. 
   Результирующая таблица должна иметь колонки `customer_id`
   и `freight_sum`, строки которой должны быть 
   отсортированы по сумме фрахтов заказов.

Мое решение:
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

Правильное решение:
```sql
SELECT customer_id, SUM(freight) as freight_sum
FROM orders
-- Следовало учесть, что JOIN можно делать не только с существующими таблицами
-- Но и с динамически созданными таблицами, с помощью подзапроса
-- Решение преподавателя более правильное и лаконичное + работает
INNER JOIN (
	SELECT customer_id, AVG(freight) as freight_avg
	FROM orders
	GROUP BY customer_id
) oa 
USING (customer_id)
WHERE freight > freight_avg 
AND shipped_date BETWEEN '1996-07-16' AND '1996-07-31'
GROUP BY customer_id
ORDER BY freight_sum
```

3. (**Решено неверно**) Напишите запрос, который выводит 3 заказа с наибольшей 
   стоимостью, которые были созданы после 1 сентября 1997 года 
   включительно и были доставлены в страны Южной Америки. 
   Общая стоимость рассчитывается как сумма стоимости 
   деталей заказа с учетом дисконта. Результирующая 
   таблица должна иметь колонки customer_id, ship_country и 
   order_price, строки которой должны быть отсортированы по 
   стоимости заказа в обратном порядке.
   
Мое решение:
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
    -- Неправильно понял задание, подумал про регион штата, а надо про страны Южной Америки
	WHERE state_region = 'south'
)
ORDER BY order_price DESC
-- Забыл поставить LIMIT 3
```

Правильное решение:
```sql
SELECT customer_id, ship_country, order_price
FROM orders
-- Как и в прошлом задании, забыл что можно использовать JOIN с динамической таблицой
-- составленной из подзапроса
JOIN (
    SELECT order_id, SUM(unit_price * quantity - unit_price * quantity * discount) as order_price
    FROM order_details
    GROUP BY order_id
) AS od
USING(order_id)
WHERE ship_country IN('Argentina', 'Bolivia', 'Brazil', 'Chile', 'Colombia', 
                      'Ecuador', 'Guyana', 'Paraguay', 'Peru', 'Suriname', 
                      'Uruguay', 'Venezuela')
AND order_date >= '1997-09-01'
ORDER BY order_price DESC
LIMIT 3
```


4. (**Решено верно**) Вывести все товары (уникальные названия продуктов), 
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
