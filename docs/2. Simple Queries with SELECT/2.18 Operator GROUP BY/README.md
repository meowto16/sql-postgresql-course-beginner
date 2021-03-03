# Оператор GROUP BY

Оператор SQL GROUP BY используется для объединения результатов выборки по одному или нескольким столбцам.

Оператор SQL GROUP BY имеет следующий синтаксис:

```sql
GROUP BY column_name
```

Располагается между `WHERE` и `ORDER BY`

```postgresql
-- Сколько в каждую страну отправляется посылок, вес которых превышает 50
SELECT ship_country, COUNT(*)
FROM orders
WHERE freight > 50
GROUP BY ship_country
ORDER BY COUNT(*) DESC
```

```postgresql
-- Сумма количества всех продуктов в продаже, сгрупированных по категории продукта
SELECT category_id, SUM(units_in_stock)
FROM products
GROUP BY category_id
ORDER BY SUM(units_in_stock) DESC
LIMIT 5
```
