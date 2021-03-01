# Оператор ORDER BY

Оператор SQL `ORDER BY` выполняет сортировку выходных значений. 
Оператор SQL `ORDER BY` можно применять как к числовым столбцам, так и к строковым. 
В последнем случае, сортировка будет происходить **по алфавиту**.

Оператор SQL `ORDER BY` имеет следующий синтаксис:
```sql
ORDER BY column_name [ASC | DESC]
```

- Параметр `ASC` (по умолчанию) устанавливает порядок сортирования во возрастанию, от меньших значений к большим.
- Параметр `DESC` устанавливает порядок сортирования по убыванию, от больших значений к меньшим.

```postgresql
SELECT DISTINCT country
FROM customers
ORDER BY country
```

```postgresql
SELECT DISTINCT country
FROM customers
ORDER BY country DESC;
```

Можно сортировать по нескольким столбцам:
```postgresql
SELECT DISTINCT country, city
FROM customers
ORDER BY country DESC, city DESC
```
