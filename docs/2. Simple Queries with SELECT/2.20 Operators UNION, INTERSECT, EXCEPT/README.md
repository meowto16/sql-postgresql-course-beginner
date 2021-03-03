# Операторы UNION, INTERSECT, EXCEPT

### UNION (объединение)
Оператор SQL `UNION` используется для объединения двух и более запросов оператора SQL SELECT.
Оператор SQL `UNION` имеет следующий синтаксис:
```sql
SELECT column_names FROM table1
UNION
SELECT column_names FROM table2
```

Вывести все страны, из которых все наши заказчики и работники
```postgresql
SELECT country
FROM customers
UNION -- по дефолту применяет DISTINCT, т.е убирает дубликаты.
-- UNION ALL - Если нужно с дубликатами
SELECT country
FROM employees
```

### INTERSECT (пересечение)
Оператор `INTERSECT` SQL используется для объединения двух инструкций `SELECT`, но возвращает 
только строки из первой инструкции `SELECT`, которые совпадают со строкой из второй инструкции `SELECT`. 
Это означает, что `INTERSECT` возвращает только строки общие для двух инструкций `SELECT`.

Вывести все страны, в которых есть и наши заказчики и поставщики
```postgresql
SELECT country
FROM customers
INTERSECT
SELECT country
FROM suppliers
```

### EXCEPT (исключением)
Из первого оператора `SELECT`, которые не возвращаются вторым оператором `SELECT`. 
Это означает, что `EXCEPT` возвращает только строки, которые не доступны во втором операторе `SELECT`.

Найти страны, где есть заказчики, **НО НЕТ** поставщиков
```postgresql
SELECT country
FROM customers
EXCEPT
-- EXCEPT ALL - если нужны дубликаты
SELECT country
FROM suppliers
```


