# IN, OUT, DEFAULT

- `IN` - входящие аргументы
- `OUT` - исходящие аргументы
- `INOUT` - и входящий и исходящий аргументы (лучше не пользоваться, усложняет отладку)
- `VARIADIC` - массив входящих параметром
- `DEFAULT` value

Создадим функцию поиска продукта по имени
```sql
CREATE OR REPLACE FUNCTION get_product_price_by_name(prod_name varchar) RETURNS real AS $$
    SELECT unit_price
    FROM products
    WHERE product_name = prod_name
$$ LANGUAGE sql
```

Попробуем использовать функцию
```sql
SELECT get_product_price_by_name('Chocolade') AS price
```

Создадим функцию для вычисления границ цен от и до
```sql
CREATE OR REPLACE FUNCTION get_price_boundaries(OUT max_price real, OUT min_price real) AS $$
    SELECT MAX(unit_price), MIN(unit_price)
    FROM products
$$ LANGUAGE sql;
```

Используем функцию
```sql
SELECT get_price_boundaries() -- Вернет Record
SELECT * FROM get_price_boundaries() -- Если нужно в разных колонках
```

Создадим функцию, которая возвращает границы цены в зависимости от флага `discontinued`
```sql
CREATE OR REPLACE FUNCTION get_price_boundaries_by_discontinuity(is_discontinued int DEFAULT 1, OUT max_price real, OUT min_price real) AS $$
    SELECT MAX(unit_price), MIN(unit_price)
    FROM products
    WHERE discontinued = is_discontinued
$$ LANGUAGE sql;
```

```sql
SELECT get_price_boundaries_by_discontinuity(0); -- Вернет Record
SELECT * FROM get_price_boundaries_by_discontinuity(0); -- Если нужно в разных колонках
SELECT * FROM get_price_boundaries_by_discontinuity(); -- Возьмет DEFAULT 1 как аргумент
```
