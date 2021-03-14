# Построение простого индекса и индекса по двум колонкам

Создадим тестовую таблицу
```sql
CREATE TABLE perf_test(
	id int,
	reason text COLLATE "C",
	annotation text COLLATE "C"
);
```

Забьем ее фейковыми данными
```sql
INSERT INTO perf_test(id, reason, annotation)
SELECT s.id, md5(random()::text), null
FROM generate_series(1, 10000000) AS s(id)
ORDER BY random();

UPDATE perf_test
SET annotation = UPPER(md5(random()::text));
```

## Пример 1

Попробуем сделать SELECT
```sql
-- Запрос исполнялся 5 сек
SELECT *
FROM perf_test
WHERE id = 3700000
```

Выясним, почему так долго с помощью EXPLAIN
```postgresql
-- Понимаем, что работал sequential scan метод сканирования
EXPLAIN
SELECT *
FROM perf_test
WHERE id = 3700000
```

Для ускорения создадим индекс
```postgresql
-- Индексы создавались 11 сек.
CREATE INDEX idx_perf_test_id ON perf_test(id);
```

Смотрим теперь еще раз метод сканирования
```postgresql
--  Теперь у нас Index scan using idx_perf_test_id
EXPLAIN
SELECT *
FROM perf_test
WHERE id = 3700000
```

Теперь этот же запрос занимает меньше времени
```postgresql
-- 61 ms
SELECT *
FROM perf_test
WHERE id = 3700000
```

## Пример 2

Сделаем запрос с `LIKE`
```postgresql
-- Такой запрос отрабатывал 8 сек
SELECT *
FROM perf_test
WHERE reason LIKE 'bc%' AND annotation LIKE 'AB%'
```

Выясним, почему так долго
```postgresql
-- Видим опять sequential scan, 
EXPLAIN ANALYZE
SELECT *
FROM perf_test
WHERE reason LIKE 'bc%' AND annotation LIKE 'AB%'
```

Построим еще один индекс, по двум колонкам
```sql
-- Заняло 27 сек
CREATE INDEX idx_perf_test_reason_annotation ON perf_test(reason, annotation);
```

Снова проверим метод сканирования
```sql
-- Теперь index scan using idx_perf_test_reason_annotation
EXPLAIN ANALYZE
SELECT *
FROM perf_test
WHERE reason LIKE 'bc%' AND annotation LIKE 'AB%'
```

Построение индекса сразу по двум колонкам, дает возможность искать и по одной колонке
```sql
-- Bitmap heap scan
EXPLAIN
SELECT *
FROM perf_test
WHERE reason LIKE 'bc%'
```

Но, если сделать поиск **ТОЛЬКО** по другой колонке, то получим
```sql
-- Получаем Seq Scan
EXPLAIN
SELECT *
FROM perf_test
WHERE annotation LIKE 'bc%'
```
