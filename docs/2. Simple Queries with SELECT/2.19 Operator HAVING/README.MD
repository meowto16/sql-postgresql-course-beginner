# Оператор HAVING

Оператор SQL `HAVING` является указателем на результат выполнения агрегатных 
функций.

Оператор SQL `HAVING` аналогичен оператору SQL `WHERE` за тем исключением, 
что применяется не для всего набора столбцов таблицы, а для 
набора созданного оператором SQL `GROUP BY` и **применяется всегда строго после него**.

```postgresql
SELECT category_id, SUM(unit_price * units_in_stock)
FROM products
WHERE discontinued <> 1
GROUP BY category_id
HAVING SUM(unit_price * units_in_stock) > 5000
ORDER BY SUM(unit_price * units_in_stock) DESC
```

** Применяется, когда нужно на результаты группировки снова наложить фильтр вроде `WHERE`
