# Оператор LIKE

Оператор SQL `LIKE` устанавливает соответствие символьной строки с шаблоном.

Оператор SQL LIKE имеет следующий синтаксис:

```sql
expression [ NOT ] LIKE pattern
```

где: 
- `expression` — любое символьное выражение
- `pattern` — шаблон, по которому будет происходить проверка выражения expression. Шаблон может включать в себя следующие спец. символы:

- `%` - Строка любой длины
- `_` - любой один символ

```postgresql
SELECT * FROM table
WHERE (
column_name LIKE 'U%' -- Строки, начинающиеся с U
OR column_name LIKE '%a' -- строки, кончающиеся на a
OR column_name LIKE '%John%' -- строки, содержащие John
OR column_name LIKE 'J%n' -- строки, начинающиеся на J, и кончающиеся на n
OR column_name LIKE '_oh_' -- строки, где 2, 3 символы - oh, а первый (1) и последний (4) - любые
)
```

```postgresql
SELECT last_name, first_name
FROM employees
WHERE (
	last_name LIKE 'Buch%'
	AND first_name LIKE 'Stev%'
)
```

```postgresql
-- первая буква доменного имени сайта содержит буквы из диапазона [k-o]:
SELECT * FROM Universities WHERE Site LIKE '[k-o]%'
```

```postgresql
-- вторая буква названия города которых, не входит в диапазон [e-o]:
SELECT * FROM Universities WHERE Location LIKE '_[^e-o]%'
```
