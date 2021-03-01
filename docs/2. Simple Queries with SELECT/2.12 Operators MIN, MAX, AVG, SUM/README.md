# Операторы MIN, MAX, AVG, SUM

- `MIN()` — функция, возвращающая минимальное значение столбца
```postgresql
SELECT MIN(order_date)
FROM orders
WHERE ship_city = 'London';
```
- `MAX()` - функция, возвращающая максимальное значение столбца
```postgresql
SELECT MAX(order_date)
FROM orders
WHERE ship_city = 'London';
```
- `AVG()` - функция, возвращающая среднее значение столбца
```postgresql
SELECT AVG(unit_price)
FROM products
WHERE discontinued <> 1
```
- `SUM()` - функция возвращающая сумму значений столбца
```postgresql
SELECT SUM(units_in_stock)
FROM products
WHERE discontinued <> 1
```
