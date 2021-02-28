# Отношение один ко многим

## Добавление данных

Оператор `INSERT` используется для вставки (добавления) строк в таблицу базы данных.

Пример использования `INSERT`
```postgresql
INSERT INTO book VALUES (1, 'The Diary of a Young Girl', '0199535566');
INSERT INTO book VALUES (2, 'Pride and Prejudice', '9780307594006');
```

```postgresql
INSERT INTO book
VALUES
(1, 'The Diary of a Young Girl', '0199535566'),
(2, 'Pride and Prejudice', '9780307594006'),
(3, 'To Kill a Mockingbird', '0446310786'),
(4, 'The Book of Gutsy Women: Favorite Stories of Courage and Resilience', '1501178415'),
(5, 'War and Peace', '1788886526');

INSERT INTO publisher
VALUES
(1, 'Everyman''s Library', 'NY'),
(2, 'Oxford University Press', 'NY'),
(3, 'Grand Central Publishing', 'Washington'),
(4, 'Simon & Schuster', 'Chicago');
```

## Один ко многим

Самое частое отношение между таблицами. Моделируется с помощью создания дополнительной колонки, которая ссылается на первичный ключ,
и этой колонке присваивается статус внешнего ключа (**FOREIGN KEY**).

Внешний ключ (**FOREIGN KEY**) - это ключ, используемый для объединения двух таблиц. 
Иногда его также называют ссылочным ключом. 
Внешний ключ — это столбец или комбинация столбцов, значения которых соответствуют 
**Первичному ключу** в другой таблице.  

Пример добавления новой колонки, и назначения её внешним ключом к таблице publisher
```postgresql
ALTER TABLE book /* Изменить таблицу book */
ADD COLUMN fk_publisher_id integer; /* Добавить колонку fk_publisher_id с типом integer */

ALTER TABLE book /* Изменить таблицу book */
ADD CONSTRAINT fk_book_publisher /* Добавить constraint fk_book_publisher */
FOREIGN KEY(fk_publisher_id) REFERENCES publisher(publisher_id); /* А именно, внешний ключ, ссылающийся на таблицу publisher */
```
