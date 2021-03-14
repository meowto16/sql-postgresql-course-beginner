# VARIADIC и FOREACH

- Чтобы передавать n-параметров (одного типа) в функцию,
нужно объявить аргумент как `VARIADIC`:
  
```sql
VARIADIC arg_name data_type[]
```

Пример использования `VARIADIC`
```sql
CREATE FUNCTION filter_even(VARIADIC numbers int[]) RETURNS SETOF int AS $$
BEGIN
	FOR counter IN 1..array_upper(numbers, 1)
	LOOP
		CONTINUE WHEN counter % 2 != 0;
		RETURN NEXT counter;
	END LOOP;
END;
$$ LANGUAGE plpgsql;

SELECT * FROM filter_even(1, 2, 3, 4, 5, 6, 7, 8); -- Вернет 2, 4, 6, 8
```

Пример использования `FOREACH`
```sql
CREATE FUNCTION filter_even(VARIADIC numbers int[]) RETURNS SETOF int AS $$
DECLARE
    counter int;
BEGIN
	FOREACH counter IN ARRAY numbers
	LOOP
		CONTINUE WHEN counter % 2 != 0;
		RETURN NEXT counter;
END LOOP;
END;
$$ LANGUAGE plpgsql;

SELECT * FROM filter_even(1, 2, 3, 4, 5, 6, 7, 8); -- Вернет 2, 4, 6, 8
```
