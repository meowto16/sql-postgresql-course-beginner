# Соединение. AS

Оператор SQL `AS` используется для именования результирующих столбцов при выборке элементов.

Оператор SQL `AS` имеет следующий синтаксис:
```sql
SELECT column_name AS new_column_name FROM table_name
```

Примеры: 
```sql
SELECT COUNT(*) AS employees_count
FROM employees
```

```sql
SELECT category_id, SUM(units_in_stock) AS units_in_stock
FROM products
GROUP BY category_id
ORDER BY units_in_stock DESC
LIMIT 5
```

```sql
SELECT category_id, SUM(unit_price * unit_stock) as total_price
FROM products
WHERE discontinued <> 1
GROUP BY category_id
HAVING total_price > 5000
ORDER BY total_price DESC
```

- Нельзя пользоваться в `WHERE`
- Нельзя пользоваться в `HAVING`
- Стараться не дублировать выражения и назначать им псевдонимы
- Вы обязаны использовать псевдонимы в подзапросах
