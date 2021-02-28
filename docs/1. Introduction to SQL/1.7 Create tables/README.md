# Создание таблиц

## Заметки

Первичный ключ (**PRIMARY KEY**) — как правило, целочисленное значение. Является уникальным идентификатором каждой из строк. 

#### Пример создания таблиц через SQL
```postgresql
CREATE TABLE publisher
(
	publisher_id integer PRIMARY KEY,
	org_name varchar(128) NOT NULL,
	address text NOT NULL
);

CREATE TABLE book
(
	book_id integer PRIMARY KEY,
	title text NOT NULL,
	isbn varchar(32) NOT NULL
);
```

#### Пример удаления таблиц через SQL
```postgresql
DROP TABLE publisher;
DROP TABLE book;
```
