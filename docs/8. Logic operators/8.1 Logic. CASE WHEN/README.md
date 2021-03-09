# Логические операторы. CASE WHEN

```sql
CASE
    WHEN condition_1 THEN result_1
    WHEN condition_2 THEN result_2
    [WHEN...]
    [ELSE result_n]
END
```

- `condition` - условие, возвращающее `bool`
- `result1` - результат или действие в случае с PL\pgSQL

```sql
SELECT product_name, unit_price, units_in_stock,
	CASE WHEN units_in_stock >= 100 THEN 'lots of'
		 WHEN units_in_stock >= 50 AND units_in_stock < 100 THEN 'average'
		 WHEN units_in_stock < 50 THEN 'low number'
		 ELSE 'unknown'
	END AS amount
FROM products
ORDER BY units_in_stock DESC;

SELECT order_id, order_date,
	CASE WHEN date_part('month', order_date) BETWEEN 3 AND 5 THEN 'spring'
		 WHEN date_part('month', order_date) BETWEEN 6 AND 8 THEN 'summer'
		 WHEN date_part('month', order_date) BETWEEN 9 AND 11 THEN 'autumn'
		 ELSE 'winter'
	END AS season
FROM orders;
```
