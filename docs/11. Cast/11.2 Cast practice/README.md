# Приведение типов данных на практике

```sql
CREATE OR REPLACE FUNCTION type_testing(money_val float8) RETURNS void AS $$
BEGIN
    RAISE NOTICE 'ran %', money_val;
END
$$ LANGUAGE plpgsql;

SELECT type_testing(0.5);
SELECT type_testing(0.5::float4);
SELECT type_testing(1);
```

```sql
CREATE OR REPLACE FUNCTION type_testing2(money_val int) RETURNS void AS $$
BEGIN
    RAISE NOTICE 'ran %', money_val;
END
$$ LANGUAGE plpgsql;

SELECT type_testing2(1);
SELECT type_testing2(0.5);
SELECT type_testing2(0.5::int);
SELECT type_testing2(0.4::int);
SELECT type_testing2(CAST(0.5 AS int));

SELECT type_testing2('1.5');
SELECT type_testing2('1.5'::int); -- так не будет работать
SELECT type_testing2('1.5'::numeric::int); - а так будет работать
```
