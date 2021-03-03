# Домашняя работа 3. Простые выборки (ответы)

1. (**Решено верно**) Выбрать все записи заказов в которых наименование страны отгрузки начинается с 'U'
```postgresql
SELECT *
FROM orders
WHERE ship_country LIKE 'U%'
```
2. (**Решено верно**) Выбрать записи заказов (включить колонки идентификатора заказа, идентификатора заказчика, веса и страны отгузки), которые должны быть отгружены в страны имя которых начинается с 'N', отсортировать по весу (по убыванию) и вывести только первые 10 записей.
```postgresql
SELECT order_id, customer_id, freight, ship_country
FROM orders
WHERE ship_country LIKE 'N%'
ORDER BY freight DESC
LIMIT 10
```
3. (**Решено верно**) Выбрать записи работников (включить колонки имени, фамилии, телефона, региона) в которых регион неизвестен
```postgresql
SELECT first_name, last_name, home_phone, region -- region не нужен
FROM employees
WHERE region IS NULL
```
4. (**Решено верно**) Подсчитать кол-во заказчиков регион которых известен
```postgresql
SELECT COUNT(*)
FROM customers
WHERE region IS NOT NULL
```
5. (**Решено неверно**) Подсчитать кол-во поставщиков в каждой из стран и отсортировать результаты группировки по убыванию кол-ва
```postgresql
SELECT country, COUNT(*)
FROM customers -- FROM suppliers, неправильно прочитал задание
WHERE region IS NOT NULL -- WHERE не нужен
GROUP BY country
ORDER BY COUNT(*) DESC
```
6. (**Решено верно**) Подсчитать суммарный вес заказов (в которых известен регион) по странам, затем отфильтровать по суммарному весу (вывести только те записи где суммарный вес больше 2750) и отсортировать по убыванию суммарного веса.
```postgresql
SELECT ship_region, SUM(freight)
FROM orders
WHERE ship_region IS NOT NULL
GROUP BY ship_region
HAVING SUM(freight) > 2750
ORDER BY SUM(freight) DESC
```
7. (**Решено верно**) Выбрать все уникальные страны заказчиков и поставщиков и отсортировать страны по возрастанию
```postgresql
SELECT country
FROM customers
UNION
SELECT country
FROM suppliers
ORDER BY country
```
8. (**Решено верно**) Выбрать такие страны в которых "зарегистрированы" одновременно и заказчики и поставщики и работники.
```postgresql
SELECT country
FROM customers
INTERSECT
SELECT country
FROM suppliers
INTERSECT
SELECT country
FROM employees
```
9. (**Решено верно**) Выбрать такие страны в которых "зарегистрированы" одновременно заказчики и поставщики, но при этом в них не "зарегистрированы" работники.
```postgresql
SELECT country
FROM customers
INTERSECT
SELECT country
FROM suppliers
EXCEPT
SELECT country
FROM employees
```

