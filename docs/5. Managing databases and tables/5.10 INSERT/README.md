# INSERT

Когда хотим вставить данные по абсолютно всем столбцам
```sql
INSERT INTO author
VALUES (10, 'John Silver', 4.5)
```

Если хотим вставить данные только в конкретные столбцы
```sql
INSERT INTO author (author_id, full_name)
VALUES (11, 'Bob Gray')
```

Если хотим вставить несколько строчек
```sql
INSERT INTO author (author_id, full_name)
VALUES
(12, 'Name 1'),
(13, 'Name 2'),
(14, 'Name 3')
```

Создание новое таблицы на основании уже имеющейся
```sql
SELECT *
INTO best_authors
FROM author
WHERE rating >= 4.5
```

Если необходимо вставить в уже существующую таблицу новые значения, используя подзапрос
```sql
INSERT INTO best_authors
SELECT *
FROM author
WHERE rating < 4.5
```
