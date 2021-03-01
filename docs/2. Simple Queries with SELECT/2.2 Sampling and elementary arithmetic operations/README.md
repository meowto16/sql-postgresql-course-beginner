# Выборка и элементарные арифметические операции

- `+` сложение
- `-` вычитание
- `*` умножение
- `/` деление
- `^` степень
- `|/` квадратный корень
- множество др. операторов и функций

Сформировать динамическую третью колонку `total_price` как результат умножения `unit_price` на `units_stock`
```postgresql
SELECT product_id, product_name, unit_price * units_in_stock AS total_price
FROM products
```
