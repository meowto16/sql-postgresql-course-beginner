# Сложный индекс для поиска по тексту

Со строками есть особая история, поиск по паттернам сложнее.

Попробуем усложнить запрос
```sql
-- Sequential scan, 5 секунд
EXPLAIN
SELECT *
FROM perf_test
WHERE reason LIKE '%bc%'; -- ищем строку посередине
```

Подключаем расширение
```sql
CREATE EXTENSION pg_trgm;
```

Создаем индекс
```sql
CREATE INDEX trgm_idx_perf_test_reason ON perf_test USING gin (reason gin_trgm_ops);
```
