# Управляем таблицами

DDL - Data Definition Language

- `CREATE TABLE table_name` - создание таблицы
- `ALTER TABLE table_name` - изменить что-либо в таблице
  - `ADD COLUMN column_name data_type` - добавить столбцу
  - `RENAME TO new_table_name` - переименовать таблицу
  - `RENAME old_column_name TO new_column_name` - переименовать столбец
  - `ALTER COLUMN column_name SET DATA TYPE data_type` - задать новый тип данных столбцу.
- `DROP TABLE table_name` - удаляет таблицу
- `TRUNCATE TABLE table_name` - удаляет все данные внутри таблицы
- `DROP COLUMN column_name` - Удаляет столбец

Создаем таблицы

```sql
CREATE TABLE student
(
    student_id serial,
    first_name varchar,
    last_name varchar,
    birthday date,
    phone varchar
);

CREATE TABLE cathedra
(
    cathedra_id serial,
    cathedra_name varchar,
    dean varchar;
);
```

Добавим новые столбцы таблицам
```sql
ALTER TABLE student
ADD COLUMN middle_name varchar;

ALTER TABLE student
ADD COLUMN rating float;

ALTER TABLE student
ADD COLUMN enrolled date;
```

Удаляем столбец
```sql
ALTER TABLE student
DROP COLUMN middle_name;
```

Переименуем таблицу
```sql
ALTER TABLE cathedra
RENAME TO chair;
```

Переименуем колонки таблицы
```sql
ALTER TABLE chair
RENAME cathedra_id TO chair_id;

ALTER TABLE chair
RENAME cathedra_name TO chair_name;
```

Изменим тип колонок таблицы
```sql
ALTER TABLE student
ALTER COLUMN first_name SET DATA TYPE varchar(64);

ALTER TABLE student
ALTER COLUMN last_name SET DATA TYPE varchar(64);

ALTER TABLE student
ALTER COLUMN phone SET DATA TYPE varchar(30);
```

Создадим новую таблицу
```sql
CREATE TABLE faculty
(
    faculty_id serial,
    faculty_name varchar
);

INSERT INTO faculty (faculty_name)
VALUES
('faculty 1'),
('faculty 2'),
('faculty 3');
```

Очищаем таблицу (удаляем все данные внутри таблицы)
```sql
TRUNCATE TABLE faculty;
TRUNCATE TABLE faculty RESTART IDENTITY; -- сбросит автоинкременты
```

Удалим таблицу
```sql
DROP TABLE faculty;
```
