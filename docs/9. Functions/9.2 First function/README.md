# Первая функция

Создадим функцию, которая заменяет NULL в столбце region на 'unknown' в таблице tmp_customers
```sql
CREATE OR REPLACE FUNCTION fix_customer_region() RETURNS void AS $$
	UPDATE tmp_customers
	SET region = 'unknown'
	WHERE region IS NULL
$$ LANGUAGE SQL;
```

Используем функцию
```sql
SELECT fix_customer_region();
```
