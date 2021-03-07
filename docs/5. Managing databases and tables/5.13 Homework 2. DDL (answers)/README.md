# Домашняя работа 2. DDL (ответы)

1. (**Решено верно**) Создать таблицу exam с полями:

- идентификатора экзамена - автоинкрементируемый, уникальный, запрещает NULL;
- наименования экзамена
- даты экзамена
```sql
CREATE TABLE exam
(
    exam_id int GENERATED ALWAYS AS IDENTITY NOT NULL UNIQUE, -- Преподаватель использовал serial
    -- Но сам говорил, что лучше использовать GENERATED ALWAYS AS IDENTITY
    -- Так как он не позволяет самостоятельно вставить любое значение в эту колонку
    -- А предупреждает человека, что эта колонка генерируется
    -- Можно обойти лишь используя USING OVERRIDE
    exam_name varchar(255),
    exam_date date
);
```

2. (**Решено верно**) Удалить ограничение уникальности с поля идентификатора
```sql
ALTER TABLE exam
DROP CONSTRAINT exam_exam_id_key
```

3. (**Решено верно**) Добавить ограничение первичного ключа на поле идентификатора
```sql
ALTER TABLE exam
ADD PRIMARY KEY(exam_id)
```

4. (**Решено верно, но можно лучше **) Создать таблицу person с полями
- идентификатора личности (простой int, первичный ключ)
- имя
- фамилия
```sql
CREATE TABLE persons
(
    person_id int, -- Стоило прописать NOT NULL
    person_firstname varchar(70), -- Стоило прописать NOT NULL
    person_lastname varchar(70), -- Стоило прописать NOT NULL

    CONSTRAINT PK_persons_person_id PRIMARY KEY(person_id)
)
```

5. (**Решено верно, но можно лучше**) Создать таблицу паспорта с полями:
- идентификатора паспорта (простой int, первичный ключ)
- серийный номер (простой int, запрещает NULL)
- регистрация
- ссылка на идентификатор личности (внешний ключ)
```sql
CREATE TABLE passports(
	passport_id int,
	passport_serial int NOT NULL,
	passport_register varchar(60), -- Стоило прописать NOT NULL, стоило использовать тип text
	person_id int,
	
	CONSTRAINT PK_passports_passport_id PRIMARY KEY(passport_id),
	CONSTRAINT FK_passports_person_id FOREIGN KEY(person_id) REFERENCES persons(person_id)
) 
```

6. (**Решено верно, но можно лучше**) Добавить колонку веса в таблицу book (создавали ранее) с ограничением, проверяющим вес (больше 0 но меньше 100)
```sql
ALTER TABLE book
ADD COLUMN book_weight real CHECK (book_weight > 0 AND book_weight < 100) -- Стоило использовать decimal
```

7. (**Решено верно**) Убедиться в том, что ограничение на вес работает (попробуйте вставить невалидное значение)
```sql
INSERT INTO book (title, isbn, publisher_id, book_weight)
VALUES('Harry Potter', '9780747532743', 1, 125)
```

8. (**Решено верно**) Создать таблицу student с полями:
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

9. (**Решено верно**) Вставить запись в таблицу студентов и убедиться, что ограничение на вставку значения по умолчанию работает
```sql
INSERT INTO student(student_name)
VALUES
('Maxim Kirshin'),
('Kirill Ivanov')
```

10. (**Решено верно**) Удалить ограничение "по умолчанию" из таблицы студентов
```sql
ALTER TABLE student
ALTER COLUMN course_id DROP DEFAULT
```

11. (**Решено верно**) Подключиться к БД northwind и добавить ограничение на поле unit_price таблицы products (цена должна быть больше 0)
```sql
ALTER TABLE products
ADD CONSTRAINT product_unit_price_check CHECK(unit_price > 0)
```

12. (**Решено верно, можно иначе**) "Навесить" автоинкрементируемый счётчик на поле product_id таблицы products (БД northwind). 
    Счётчик должен начинаться с числа следующего за максимальным значением по этому столбцу.
```sql
-- Мое решение
ALTER TABLE products
ALTER COLUMN product_id ADD GENERATED ALWAYS AS IDENTITY 
(START WITH 78); -- я хз почему subquery не работает

-- Решение учителя
CREATE SEQUENCE IF NOT EXISTS products_product_id_seq
START WITH 78 OWNED BY products.product_id;

ALTER TABLE products
ALTER COLUMN product_id SET DEFAULT nextval('products_product_id_seq');
```

13. (**Решено верно**) Произвести вставку в products (не вставляя идентификатор явно) и убедиться, что 
    автоинкремент работает.Вставку сделать так, чтобы в результате команды вернулось 
    значение, сгенерированное в качестве идентификатора.
```sql
INSERT INTO products (product_name, discontinued)
VALUES ('Fanta', 0)
RETURNING product_id
```
