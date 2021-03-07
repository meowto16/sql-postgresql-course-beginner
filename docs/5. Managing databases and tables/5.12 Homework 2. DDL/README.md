# Домашняя работа 2. DDL

1. Создать таблицу exam с полями:

- идентификатора экзамена - автоинкрементируемый, уникальный, запрещает NULL;
- наименования экзамена
- даты экзамена
```sql
CREATE TABLE exam
(
    exam_id int GENERATED ALWAYS AS IDENTITY NOT NULL UNIQUE,
    exam_name varchar(255),
    exam_date date
);
```

2. Удалить ограничение уникальности с поля идентификатора
```sql
ALTER TABLE exam
DROP CONSTRAINT exam_exam_id_key
```

3. Добавить ограничение первичного ключа на поле идентификатора
```sql
ALTER TABLE exam
ADD PRIMARY KEY(exam_id)
```

4. Создать таблицу person с полями
- идентификатора личности (простой int, первичный ключ)
- имя
- фамилия
```sql
CREATE TABLE persons
(
    person_id int,
    person_firstname varchar(70),
    person_lastname varchar(70),

    CONSTRAINT PK_persons_person_id PRIMARY KEY(person_id)
)
```

5. Создать таблицу паспорта с полями:
- идентификатора паспорта (простой int, первичный ключ)
- серийный номер (простой int, запрещает NULL)
- регистрация
- ссылка на идентификатор личности (внешний ключ)
```sql
CREATE TABLE passports(
	passport_id int,
	passport_serial int NOT NULL,
	passport_register varchar(60),
	person_id int,
	
	CONSTRAINT PK_passports_passport_id PRIMARY KEY(passport_id),
	CONSTRAINT FK_passports_person_id FOREIGN KEY(person_id) REFERENCES persons(person_id)
) 
```

6. Добавить колонку веса в таблицу book (создавали ранее) с ограничением, проверяющим вес (больше 0 но меньше 100)
```sql
ALTER TABLE book
ADD COLUMN book_weight real CHECK (book_weight > 0 AND book_weight < 100)
```

7. Убедиться в том, что ограничение на вес работает (попробуйте вставить невалидное значение)
```sql
INSERT INTO book (title, isbn, publisher_id, book_weight)
VALUES('Harry Potter', '9780747532743', 1, 125)
```

8. Создать таблицу student с полями:
- идентификатора (автоинкремент)
- полное имя
- курс (по умолчанию 1)
```sql
CREATE TABLE student
(
    student_id int GENERATED ALWAYS AS IDENTITY NOT NULL UNIQUE,
	student_name varchar(255),
	course_id int DEFAULT 1
)
```

9. Вставить запись в таблицу студентов и убедиться, что ограничение на вставку значения по умолчанию работает
```sql
INSERT INTO student(student_name)
VALUES
('Maxim Kirshin'),
('Kirill Ivanov')
```

10. Удалить ограничение "по умолчанию" из таблицы студентов
```sql
ALTER TABLE student
ALTER COLUMN course_id DROP DEFAULT
```

11. Подключиться к БД northwind и добавить ограничение на поле unit_price таблицы products (цена должна быть больше 0)
```sql
ALTER TABLE products
ADD CONSTRAINT product_unit_price_check CHECK(unit_price > 0)
```

12. "Навесить" автоинкрементируемый счётчик на поле product_id таблицы products (БД northwind). 
    Счётчик должен начинаться с числа следующего за максимальным значением по этому столбцу.
```sql
ALTER TABLE products
ALTER COLUMN product_id ADD GENERATED ALWAYS AS IDENTITY 
(START WITH 78) -- я хз почему subquery не работает
```

13. Произвести вставку в products (не вставляя идентификатор явно) и убедиться, что 
    автоинкремент работает.Вставку сделать так, чтобы в результате команды вернулось 
    значение, сгенерированное в качестве идентификатора.
```sql
INSERT INTO products (product_name, discontinued)
VALUES ('Fanta', 0)
RETURNING product_id
```
