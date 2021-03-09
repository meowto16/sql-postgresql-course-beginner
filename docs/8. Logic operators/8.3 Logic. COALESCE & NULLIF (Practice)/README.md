# Логические операторы. COALESCE & NULLIF (практика)

Подставит вместо `NULL` в `ship_region` -> `unknown`
```sql
SELECT order_id, order_date, COALESCE(ship_region, 'unknown') AS ship_region
FROM orders
LIMIT 10;
```

Подставит вместо `NULL` в `region` -> `N/A`
```sql
SELECT last_name, first_name, COALESCE(region, 'N/A') AS region
FROM employees;
````

***`NULL IF` возвращает `NULL` если два значения одинаковы***

Исправим пустые строки '';
```sql
SELECT contact_name, COALESCE(NULLIF(city, ''), 'Unknown city') AS city
FROM customers
```
