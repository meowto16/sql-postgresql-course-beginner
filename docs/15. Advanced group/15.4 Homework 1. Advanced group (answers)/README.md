# Домашняя работа 1. Продвинутая группировка

## Задание 1
### Постановка задачи (решено частично верно)
Вывести сумму продаж (цена * кол-во) по каждому сотруднику с 
подсчётом полного итога (полной суммы по всем сотрудникам) 
отсортировав по сумме продаж (по убыванию).
### Решение
```sql
SELECT
    CASE WHEN (GROUPING(CONCAT(e.first_name, ' ', e.last_name)) = 1) THEN 'Total'
         ELSE CONCAT(e.first_name, ' ', e.last_name)
        END AS fullname,
    SUM(od.unit_price * quantity) AS orders_sum
FROM orders AS o
     JOIN order_details AS od USING (order_id)
     JOIN products AS p USING (product_id)
     JOIN employees AS e USING (employee_id)
GROUP BY ROLLUP (CONCAT(e.first_name, ' ', e.last_name))
ORDER BY orders_sum DESC;
```
### Решение учителя
```sql
SELECT employee_id, SUM(unit_price * quantity)
FROM orders
LEFT JOIN order_details USING (order_id) -- Надо было использовать LEFT JOIN
GROUP BY ROLLUP(employee_id)
ORDER BY SUM(unit_price * quantity) DESC;
```

## Задание 2
### Постановка задачи
Вывести отчёт показывающий сумму продаж по сотрудникам и 
странам отгрузки с подытогами по сотрудникам и общим итогом.
### Решение
```sql
SELECT
    CONCAT(e.first_name, ' ', e.last_name) AS fullname,
    o.ship_country,
    SUM(od.unit_price * quantity) AS orders_sum
FROM orders AS o
     JOIN order_details AS od USING (order_id)
     JOIN products AS p USING (product_id)
     JOIN employees AS e USING (employee_id)
GROUP BY CUBE (fullname, ship_country)
ORDER BY orders_sum DESC NULLS FIRST;
```
### Решение учителя (решено неверно)
```sql
SELECT employee_id, ship_country, SUM(unit_price * quantity)
FROM orders
LEFT JOIN order_details USING (order_id) -- Надо было использовать LEFT JOIN
GROUP BY ROLLUP(employee_id, ship_country) -- Надо было ROLLUP
ORDER BY SUM(unit_price * quantity) DESC;
```

## Задание 3
### Постановка задачи
Вывести отчёт показывающий сумму продаж по сотрудникам, 
странам отгрузки, сотрудникам и странам отгрузки с подытогами 
по сотрудникам и общим итогом.
### Решение (решено неверно)
```sql
SELECT
    CONCAT(e.first_name, ' ', e.last_name) AS fullname,
    o.ship_country,
    SUM(od.unit_price * quantity) AS orders_sum
FROM orders AS o
     JOIN order_details AS od USING (order_id)
     JOIN products AS p USING (product_id)
     JOIN employees AS e USING (employee_id)
GROUP BY CUBE (fullname), (ship_country), (fullname, ship_country)
ORDER BY employee_id, orders_sum DESC NULLS FIRST;
```
### Решение учителя
```sql
SELECT employee_id, ship_country, SUM(unit_price * quantity)
FROM orders
LEFT JOIN order_details USING (order_id) -- Надо было использовать LEFT JOIN
GROUP BY CUBE(employee_id, ship_country) -- Надо было ROLLUP
ORDER BY employee_id, SUM(unit_price * quantity) DESC;
```
