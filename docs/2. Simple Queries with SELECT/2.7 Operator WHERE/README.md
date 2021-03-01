# Оператор WHERE

Оператор SQL `WHERE` служит для задания дополнительного условия выборки, операций вставки, редактирования и удаления записей.

Оператор SQL `WHERE` имеет следующий синтаксис:
```postgresql
SELECT *
FROM table
WHERE condition
```

Условие (condition) может включать в себя предикаты `AND`, `OR`, `NOT`, `LIKE`, `BETWEEN`, `IS`, `IN`, ключевое слово NULL, операторы сравнения и равенства (<, >, =).

### Операторы сравнения

- a = b
- a > b
- a >= b
- a < b
- a <= b
- a <> b или a != b

Выбрать всех покупателей, у которых страна = `USA`
```postgresql
SELECT company_name, contact_name, phone, country
FROM customers
WHERE country = 'USA'
```

Выбрать все продукты, цена которых больше 20 у.е.
```postgresql
SELECT *
FROM products
WHERE unit_price > 20
```

Посчитать количество продуктов, где цена меньше 20 y.e.
```postgresql
SELECT COUNT(*)
FROM products
WHERE unit_price < 20
```

Выбрать продукты, которые уже НЕ продаются
```postgresql
SELECT *
FROM products
WHERE discontinued = 1
```

Выбрать заказчиков, которые НЕ в Берлине
```postgresql
SELECT *
FROM customers
WHERE city <> 'Berlin'
```

Выбрать заказы, которые были совершены после 01.03.1998
```postgresql
SELECT *
FROM orders
WHERE order_date >= '1998-03-01'
```
