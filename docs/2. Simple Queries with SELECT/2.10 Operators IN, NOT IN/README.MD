# Операторы IN, NOT IN

Оператор SQL `IN` позволяет определить, совпадает ли значение объекта со значением в списке.

Оператор SQL `IN` имеет следующий синтаксис:
```postgresql
-- 1
SELECT *
FROM Universities
WHERE Location IN ('Novosibirsk', 'Perm')
```

```postgresql
-- 2
SELECT *
FROM customers
WHERE Location IN ('Mexico', 'Germany', 'USA', 'Canada')
```

Аналогичен:
```postgresql
-- 1
SELECT *
FROM Universities
WHERE Location = 'Novosibirsk'
OR Location = 'Perm'
```

```postgresql
-- 2
SELECT *
FROM customers
WHERE country = 'Mexico' OR country = 'Germany' OR country = 'USA' OR country = 'Canada'
```

Можно использовать числа

```postgresql
SELECT *
FROM products
WHERE category_id IN (1, 3, 5, 7)
```

Используя `NOT`
```postgresql
SELECT *
FROM customers
WHERE country NOT IN ('Mexico', 'Germany', 'USA', 'Canada')
```

```postgresql
SELECT *
FROM products
WHERE category_id NOT IN (1, 3, 5, 7)
```
