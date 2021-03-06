# Подзапросы с квантификаторами ANY, ALL

Операторы `ANY` и `ALL` используются в `WHERE` и `HAVING`

Оператор `ANY` возвращает `true` если **ЛЮБОЕ** из значений подзапроса соответствует условию

Оператор `ALL` возвращает `true`, если **КАЖДОЕ** из значений подзапроса соответствует условию

Выбрать все уникальные компании, которые делали заказы на более чем 40 товаров

Используя `INNER JOIN`:
```sql
SELECT DISTINCT company_name
FROM customers
JOIN orders USING(customer_id)
JOIN order_details USING(order_id)
WHERE quantity > 40
```

Используя подзапрос:
```sql
SELECT DISTINCT company_name
FROM customers
WHERE customer_id = ANY(
    SELECT customer_id
    FROM orders
    JOIN order_details USING(order_id)
    WHERE quantity > 40
)
```

Выбрать продукты, количество которых больше среднего по заказу
```sql
SELECT DISTINCT product_name, quantity
FROM products
JOIN order_details USING(product_id)
WHERE quantity > (
    SELECT AVG(quantity)
    FROM order_details
)
ORDER BY quantity DESC
```

Выбрать все продукты, количество которых больше среднего значения из групп полученных группированием по product_id
```sql
SELECT DISTINCT product_name, quantity
FROM products
JOIN order_details USING(product_id)
WHERE quantity > ALL(
    SELECT AVG(quantity)
    FROM order_details
    GROUP BY product_id
)
ORDER BY quantity
```
