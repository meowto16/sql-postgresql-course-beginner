# Проверка ON NULL

- `IS NULL` - ключевое слово, выполняющее логическое сравнение. Возвращает `true` если представленное значение является `NULL`
- `NOT NULL` - ключевое слово, выполняющее логическое сравнение. Возвращает `true` если представленное значение **НЕ** является `NULL`

```postgresql
SELECT ship_city, ship_region, ship_country
FROM orders
WHERE ship_region IS NULL;

SELECT ship_city, ship_region, ship_country
FROM orders
WHERE ship_region IS NOT NULL;
```
