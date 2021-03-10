# Скалярные функции

Создадим скалярную функцию, которая возвращает сумму `units_in_stock` в таблице `products`
```sql
CREATE OR REPLACE FUNCTION get_total_number_of_goods() RETURNS bigint AS $$
	SELECT SUM(units_in_stock)
	FROM products
$$ LANGUAGE SQL
```

Вызовем скалярную функцию
```sql
SELECT get_total_number_of_goods() AS total_goods
```

Создадим скалярную функцию, которая возвращает среднее `unit_price` в таблице `products`
```sql
CREATE OR REPLACE FUNCTION get_avg_price() RETURNS float8 AS $$
	SELECT AVG(unit_price)
	FROM products
$$ LANGUAGE SQL
```

Вызовем скалярную функцию
```sql
SELECT get_avg_price() AS avg_price
```
