# Отношение многие ко многим

Пример: у одной книги может быть множество со-авторов. И один автор, может быть автором множества книг.

## Заметки

- Отношение "многие ко многим" моделируется всегда при помощи введения 3-ей таблицы.
- В данной таблице сопоставления сопоставляются внешние ключи, ссылающиеся на первичные ключи из 1 и 2 таблицы.
- Строки в таблице сопоставления всегда являются уникальными


1. Удаляем старые БД
```postgresql
DROP TABLE IF EXISTS book;
DROP TABLE IF EXISTS author;
```

2. Создаем новые БД, где 3-ья таблица `book_author` нужна для моделирования отношения "многие ко многим"
```postgresql
CREATE TABLE book
(
	book_id int PRIMARY KEY,
	title text NOT NULL,
	isbn text NOT NULL
);

CREATE TABLE author
(
	author_id int PRIMARY KEY,
	full_name text NOT NULL,
	rating real
);

CREATE TABLE book_author
(
	book_id int REFERENCES book(book_id),
	author_id int REFERENCES author(author_id),
	
	CONSTRAINT book_author_pkey PRIMARY KEY (book_id, author_id) --  composite key
);
```
3. Заполняем значениями
```postgresql
INSERT into book
VALUES
(1, 'Book for Dummies', '123456'),
(2, 'Book for Smart Guys', '7890123'),
(3, 'Book for Dummies', '4567890'),
(4, 'Book for Dummies', '1234567');

INSERT INTO author
VALUES
(1, 'Bob', 4.5),
(2, 'Alice', 4.0),
(3, 'John', 4.7);

INSERT INTO book_author
VALUES
(1, 1),
(2, 1),
(3, 1),
(3, 2),
(4, 1),
(4, 2),
(4, 3);
```
