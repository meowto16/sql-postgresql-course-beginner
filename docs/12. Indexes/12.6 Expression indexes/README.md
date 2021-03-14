# Индексы по выражениям

Вспомним прошлую проблему, запрос исполняется долго, метод сканирования Sequential scan
```sql
EXPLAIN
SELECT *
FROM perf_test
WHERE annotation LIKE 'AB%';
```

Можно конечно построить индекс
```sql
CREATE INDEX idx_perf_test_annotation ON perf_test(annotation);
```

Попробуем другой вариант (придется строить отдельный индекс)
```sql
EXPLAIN
SELECT *
FROM perf_test
WHERE LOWER(annotation) LIKE('ab%');
```

Можно и для такой ситуации, конечно, построить отдельный индекс
```sql
CREATE INDEX idx_perf_test_annotation_lower ON perf_test(LOWER(annotation))
```

По выражениям индекс не работает, поэтому надо строить либо нормальный запрос,
без использования выражений, либо строить отдельный индекс
