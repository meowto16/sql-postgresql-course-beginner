# Оператор BETWEEN

Оператор SQL `BETWEEN` задает диапазон (включая начальное и конечное значения диапазона), 
в котором будет осуществляться проверка условия.
Оператор SQL `BETWEEN` имеет следующий синтаксис:

```sql
test_expression [NOT] BETWEEN begin_expression AND end_expression
```

- `test_expression` — задает объект для проверки по диапазону;
- `start_expression` — начальное значение диапазона;
- `end_expression` — конечное значение диапазона;

Пример
```postgresql
SELECT *
FROM Universities
WHERE Students BETWEEN 10000 AND 30000;

SELECT *
FROM Universities
WHERE Professores NOT BETWEEN 2000 AND 14000
```

Пример без использования `BETWEEN`

```postgresql
SELECT *
from orders
WHERE freight >= 20 AND freight <= 40;
```

Пример с использованием `BETWEEN` (эквивалент кода выше)
```postgresql
SELECT *
FROM orders
WHERE freight BETWEEN 20 AND 40
```

Пример с использованием даты
```postgresql
SELECT *
FROM orders
WHERE order_date BETWEEN '1998-03-30' AND '1998-04-03'
```
