# Домашняя работа 1. Продвинутая группировка

## Задание 1
### Постановка задачи
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

## Задание 3
### Постановка задачи
Вывести отчёт показывающий сумму продаж по сотрудникам, 
странам отгрузки, сотрудникам и странам отгрузки с подытогами 
по сотрудникам и общим итогом.
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
GROUP BY CUBE (fullname), (ship_country), (fullname, ship_country)
ORDER BY orders_sum DESC NULLS FIRST;
```
