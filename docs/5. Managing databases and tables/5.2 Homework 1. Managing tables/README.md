# Домашняя работа 1. Управляем таблицами

1. Создать таблицу teacher с полями teacher_id serial, first_name varchar, last_name varchar, birthday date, phone varchar, title varchar
```sql
CREATE TABLE teacher
(
    teacher_id serial,
    first_name varchar,
    last_name varchar,
    birthday date,
    phone varchar,
    title varchar
)
```

2. Добавить в таблицу после создания колонку middle_name varchar
```sql
ALTER TABLE teacher
ADD COLUMN middle_name varchar
```

3. Удалить колонку middle_name
```sql
ALTER TABLE teacher
DROP COLUMN middle_name
```

4. Переименовать колонку birthday в birth_date
```sql
ALTER TABLE teacher
RENAME birthday TO birth_date
```

5. Изменить тип данных колонки phone на varchar(32)
```sql
ALTER TABLE teacher
ALTER COLUMN phone SET DATA TYPE varchar(32)
```

6. Создать таблицу exam с полями exam_id serial, exam_name varchar(256), exam_date date
```sql
CREATE TABLE exam
(
    exam_id serial,
    exam_name varchar(256),
    exam_date date
)
```

7. Вставить три любых записи с автогенерацией идентификатора
```sql
INSERT INTO exam (exam_name, exam_date)
VALUES
('Exam 1', '1995-01-01'),
('Exam 2', '1996-01-01'),
('Exam 3', '1997-01-01')
```

8. Посредством полной выборки убедиться, что данные были вставлены нормально и идентификаторы были сгенерированы с инкрементом
```sql
SELECT *
FROM exam
```

9. Удалить все данные из таблицы со сбросом идентификатор в исходное состояние
```sql
TRUNCATE exam RESTART IDENTITY;
```
