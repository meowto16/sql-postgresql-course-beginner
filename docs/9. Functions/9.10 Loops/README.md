# Циклы PL/pgSQL

## Синтаксис 

`WHILE`
```sql
WHILE expression
LOOP
    -- logic
END LOOP;
```

`LOOP`
```sql
LOOP
    EXIT WHEN expression
    --logic
END LOOP;
```

`FOR`
```sql
-- [BY x] -- на сколько инкрементировать
FOR counter IN a..b [BY x]
LOOP
    logic
END LOOP;    
```

```sql
-- Прервать исполнение цикла, перейти к следующей итерации
CONTINUE WHEN expression
```

```sql
-- Break, прервать исполнение цикла
EXIT WHEN expression
```

## Примеры

#### Последовательность Фибоначчи

- Используя `WHILE`
```sql
CREATE FUNCTION fib(n int) RETURNS int AS $$
DECLARE
    counter int = 0;
    i int = 0;
    j int = 1;
BEGIN
    IF n < 1 THEN
        RETURN 0;
    END IF;

    WHILE counter < n
    LOOP
        counter = counter + 1;
        SELECT j, i + j INTO i, j;
    END LOOP;

    RETURN i;
END;
$$ LANGUAGE plpgsql;

SELECT fib(5);
```

- Используя `EXIT WHEN`
```sql
CREATE FUNCTION fib(n int) RETURNS int AS $$
DECLARE
    counter int = 0;
    i int = 0;
    j int = 1;
BEGIN
    IF n < 1 THEN
        RETURN 0;
    END IF;

    LOOP
        EXIT WHEN counter >= n;
        counter = counter + 1;
        SELECT j, i + j INTO i, j;
    END LOOP;

    RETURN i;
END;
$$ LANGUAGE plpgsql;

SELECT fib(5);
```

#### Анонимный блок кода
```sql
DO $$
BEGIN
	FOR counter IN 1..5 
	LOOP
		RAISE NOTICE 'Counter: %', counter;
	END LOOP;
END$$;
```

Если надо декрементировать
```sql
DO $$
BEGIN
	FOR counter IN REVERSE 5..1 
	LOOP
		RAISE NOTICE 'Counter: %', counter;
	END LOOP;
END$$;
```

Если надо задать шаг инкремента
```sql
DO $$
BEGIN
	FOR counter IN 1..11 BY 2
	LOOP
		RAISE NOTICE 'Counter: %', counter;
	END LOOP;
END$$;
```
