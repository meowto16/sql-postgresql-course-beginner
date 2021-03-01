# Операторы AND, OR

Операторы SQL `AND` и SQL `OR` — предикаты языка SQL, 
служащие для создания логических выражений. 
В SQL предикатами называются операторы, возвращающие значения `TRUE` или `FALSE`. 
Предикат SQL `AND` — эквивалент логического умножения (**конъюнкции**), 
предикат SQL `OR` — эквивалент логического сложения (**дизъюнкции**).

**Логическое**. Если оба условия вычисляются в `TRUE` - попадает в результирующий набор:
```postgresql
SELECT *
FROM table
WHERE condition1 AND condition2
```

**Логическое ИЛИ**. Если одно из условий вычисляется в `TRUE` - попадает в результирующий набор:
```postgresql
SELECT *
FROM table
WHERE condition1 OR condition2
```

**Комбинация предикатов** может быть какой-угодно длины
```postgresql
SELECT *
FROM table
WHERE condition1 AND condition2 AND condition3
```

Получаем продукты, где цена больше 25 у.е. и больше 40 шт. на складе
```postgresql
SELECT *
FROM products
WHERE unit_price > 25 AND units_in_stock > 40
```

Отфильтровать всех покупателей, которые проживают в Берлине, Лондоне или Сан-Франциско
```postgresql
SELECT *
FROM customers
WHERE city = 'Berlin' OR city = 'London' OR city = 'San Francisco'
```

Отфильтровать все заказы, отгруженные после 30 апреля 1998 года, где вес отгрузки был меньше 75 или больше 150
```postgresql
SELECT *
FROM orders
WHERE shipped_date > '1998-04-30' AND (freight < 75 OR freight > 150)
```
