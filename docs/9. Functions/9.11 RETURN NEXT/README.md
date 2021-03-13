# RETURN NEXT

- Иногда необходимо накапливать записи в результирующем наборе (построчный процессинг)
- `RETURN NEXT` expression;

### Пример 
```sql
CREATE FUNCTION return_ints() RETURNS SETOF int AS $$
BEGIN
    RETURN NEXT 1;
    RETURN NEXT 2;
    RETURN NEXT 3;
END;
$$ LANGUAGE plpgsql;
```

### Пример с northwind
```sql
-- Лучше таких примеров, на самом деле, избегать
-- Такое не используют
CREATE FUNCTION after_christmas_sale() RETURNS SETOF products AS $$
DECLARE
    product record;
BEGIN
FOR product IN SELECT * FROM products
     LOOP
        IF product.category_id IN (1,4,8) THEN
            product.unit_price = product.unit_price * 0.8;
        ELSEIF product.category_id IN (2,3,7) THEN
			product.unit_price = product.unit_price * 0.75;
        ELSE
			product.unit_price = product.unit_price * 1.1;
        END IF;
		RETURN NEXT product;
    END LOOP;
END;
$$ LANGUAGE plpgsql;

SELECT * FROM after_christmas_sale();
```

***
***Декларативный стиль чистого SQL в 99% случаев более читабелен, чем процедурный стиль***
***
