# Возврат множества строк

- `RETURNS SETOF data_type` - возврат n-значений типа `data_type`
- `RETURNS SETOF table` - если нужно вернуть все столбцы из таблицы или пользовательского типа
- `RETURNS SETOF record` - (только когда типы колонок в результирующем наборе заранее известны)
- `RETURNS TABLE (column_name data_type, ...)` - тоже что и `setof table`, но имеем возможность явно указать
возвращаемые столбцы
- Возврат через out-параметры

Вернуть средние цены по категории продуктов
```sql
CREATE OR REPLACE FUNCTION get_average_prices_by_prod_categories()
    RETURNS SETOF double precision 
AS $$
        SELECT AVG(unit_price) AS average_prices
        FROM products
        GROUP BY category_id
$$ LANGUAGE SQL;
```

Вызовем функцию
```sql
SELECT * FROM get_average_prices_by_prod_categories()
```

```sql
CREATE OR REPLACE FUNCTION get_avg_prices_by_prod_cats(OUT sum_price real, OUT avg_price float8)
    RETURNS SETOF RECORD -- Если убрать эту строку, будет возвращать только одно значение
AS $$
        SELECT SUM(unit_price), AVG(unit_price)
        FROM products
        GROUP BY category_id
$$ LANGUAGE SQL;
```

```sql
SELECT sum_price FROM get_avg_prices_by_prod_cats();
SELECT sum_price, avg_price FROM get_avg_prices_by_prod_cats();
SELECT sum_price AS sum_of, avg_price AS in_avg FROM get_avg_prices_by_prod_cats();

-- Если не прописаны OUT параметры
-- Лучше по возможности всегда прописывать OUT параметры
SELECT * FROM get_avg_prices_by_prod_cats() AS (sum_price real, avg_price float8);
```

Функция, которая возвращает клиентов по стране
```sql
CREATE OR REPLACE FUNCTION get_customers_by_country(customer_country varchar)
    RETURNS TABLE(char_code char, company_name varchar)
AS $$
    SELECT customer_id, company_name
    FROM customers
    WHERE country = customer_country
$$ LANGUAGE SQL;
```

Вызовем функцию
```sql
SELECT * FROM get_customers_by_country();
```

Или так, вернем все поля таблицы `customers`
```sql
CREATE OR REPLACE FUNCTION get_customers_by_country(customer_country varchar)
    RETURNS SETOF customers
AS $$
    SELECT *
    FROM customers
    WHERE country = customer_country
$$ LANGUAGE SQL;

SELECT * FROM get_customers_by_country('USA');
```

 
