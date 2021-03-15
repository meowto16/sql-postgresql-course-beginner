# Составные типы

- Составной тип — тип данных, состоящий из нескольких полей имеющих свой тип
- Нельзя задавать ограничения (constraints). Поэтому не так часто используются в таблицах
- Зато часто используются для возврата данных из функций
- Чаще всего вам не нужен составной тип! Как правило, таблица лучше!

Синтаксис создания своего составного типа
```sql
CREATE TYPE type_name AS (
    field1 type,
    field2 type
)
```

- Чтобы создать экземпляр:
`'(1, 2, "text")` или `ROW(1, 2, 'text')`
  
Чтобы удалить
```sql
DROP IF EXISTS TYPE type_name [CASCADE]
```

Создадим функцию
```sql
DROP FUNCTION IF EXISTS get_price_boundaries;
CREATE OR REPLACE FUNCTION get_price_boundaries(out max_price real, out min_price real)
RETURNS SETOF record AS $$
    SELECT MAX(unit_price), MIN(unit_price)
    FROM products;
$$ LANGUAGE SQL;
```

```sql
SELECT * FROM get_price_boundaries();
```

Создадим функцию эквивалент, используя составные типы
```sql
CREATE TYPE price_bounds AS (
    max_price real,
    min_price real
);
```

```sql
CREATE FUNCTION get_price_boundaries() RETURNS SETOF price_bounds AS $$
    SELECT MAX(unit_price), MIN(unit_price)
    FROM products;
$$ LANGUAGE SQL;
```

Создадим таблицу, используя составной тип

```sql
CREATE TYPE complex AS (
    r float8,
    f float8
);
```

```sql
CREATE TABLE math_calcs (
    math_id serial,
    val complex
);
```

Заполняем колонки составного типа
```sql
INSERT INTO math_calcs(val)
VALUES
(ROW(3.0, 4.0)),
(ROW(2.0, 1.0))
```


```sql
SELECT * FROM math_calcs;

SELECT (val).r FROM math_calcs; -- Выбрать конкретное значение из составного типа
```

Обновить конкретное значение у составного типа
```sql
UPDATE math_calcs
SET val.r = 3.0
WHERE math_id = 1;
```
