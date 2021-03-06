# Primary Key. Первичный ключ

`PRIMARY KEY` гарантирует уникальность колонки, которая помечена как первичный ключ. 

Является ограничением (constraint), которое не дает вставить дубликат.

Также не дает вставлять `NULL` в колонку, которая помечена `PRIMARY KEY`

Создадим таблицу с `PRIMARY KEY`
```sql
CREATE TABLE chair
(
    chair_id serial PRIMARY KEY,
    chair_name varchar,
    dean varchar

    -- CONSTRAINT PK_chair_chair_id PRIMARY KEY(chair_id)
);

INSERT INTO chair
VALUES (1, 'name', 'dean');
```

Рассмотрим почти эквивалент
```sql
CREATE TABLE chair
(
    chair_id serial UNIQUE NOT NULL,
    chair_name varchar,
    dean varchar
);

INSERT INTO chair
VALUES (1, 'name', 'dean');
```

Разница
- `PRIMARY KEY` может быть только один для
- `PRIMARY KEY` используется для идентификации строки (например при джоинах)
- Существенной теоретической разницы нет
- просто `UNIQUE` не накладывает `NOT NULL`


```sql
SELECT constraint_name
FROM information_schema.key.column_usage
WHERE table_name = 'chair'
    AND table_schema = 'public'
    AND column_name = 'chair_id'
```

Удалим `CONSTRAINT`
```sql
ALTER TABLE
DROP CONSTRAIN chair_chair_id_key
```

Добавим `CONSTRAINT` существующей таблице
```sql
ALTER TABLE chair
-- ADD CONSTRAINT
ADD PRIMARY KEY(chair_id)
```
