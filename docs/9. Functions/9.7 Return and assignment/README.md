# Возврат и присвоение

```sql
CREATE OR REPLACE FUNCTION get_total_number_of_goods() RETURNS bigint AS $$
BEGIN
    RETURN SUM(units_in_stock)
    FROM products;
END
$$ LANGUAGE plpgsql;

SELECT get_total_number_of_goods();
```

```sql
CREATE OR REPLACE FUNCTION get_max_price_from_discontinued() RETURNS real AS $$
BEGIN
    RETURN MAX(unit_price)
    FROM products
    WHERE discontinued = 1;
END
$$ LANGUAGE plpgsql;

SELECT get_max_price_from_discontinued();
```

```sql
CREATE OR REPLACE FUNCTION get_price_boundaries(OUT max_price real, OUT min_price real) AS $$
BEGIN
    SELECT MAX(unit_price), MIN(unit_price)
    INTO max_price, min_price
    FROM products;
END;
$$ LANGUAGE plpgsql;

-- BEGIN
    -- max_price := MAX(unit_price) FROM products; -- По сути это присвоение переменной
    -- min_price := MIN(unit_price) FROM products;
    -- max_price = MAX(unit_price) FROM products; -- Присвоение будет работать и через =
-- END;

SELECT get_price_boundaries();
```

```sql
CREATE OR REPLACE FUNCTION get_sum(x int, y int, OUT result int) AS $$
BEGIN
	result := x + y;
	RETURN; -- Возврат из функции, явный
END
$$ LANGUAGE plpgsql;
```

```sql
CREATE OR REPLACE FUNCTION get_customers_by_country(customer_country varchar) RETURNS SETOF customers AS $$
BEGIN
	RETURN QUERY
	SELECT *
	FROM customers
	WHERE country = customer_country;
END;
$$ LANGUAGE plpgsql;

SELECT * FROM get_customers_by_country('USA');  
```
