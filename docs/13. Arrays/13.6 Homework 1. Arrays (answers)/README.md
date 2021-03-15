# Домашняя работа 1. Массивы (ответы)

Создать функцию, которая вычисляет средний фрахт по заданным странам
(функция принимает список стран).

### Решение (решено верно)

```sql
CREATE OR REPLACE FUNCTION get_avg_freight_by_countries(VARIADIC countries varchar[]) RETURNS float8 AS $$
    SELECT AVG(freight)
    FROM orders
    WHERE ship_country = ANY(countries);
$$ LANGUAGE SQL;

SELECT get_avg_freight_by_countries('France', 'German', 'Brazil');
SELECT get_avg_freight_by_countries(VARIADIC ARRAY['France', 'German', 'Brazil']); -- Можно еще так
```
