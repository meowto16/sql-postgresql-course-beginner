# Foreign Key. Внешний ключ

Если ограничение `FOREIGN KEY` задано, то сервер при `INSERT` значений проверяет,
действительно ли существует такой первичный ключ `PRIMARY KEY` в таблице, на которую ссылаются.

```sql
CREATE TABLE publisher
(
	publisher_id int,
	publisher_name varchar(128) NOT NULL,
	address text,
	
	CONSTRAINT PK_publisher_publisher_id PRIMARY KEY(publisher_id),
	CONSTRAINT FK_book_publisher FOREIGN KEY (publisher_id) REFERENCES publisher(publisher_id)
);

CREATE TABLE book
(
	book_id int,
	title text NOT NULL,
	isbn varchar(32) NOT NULL,
	publisher_id int,
	
	CONSTRAINT PK_book_book_id PRIMARY KEY(book_id)
);
```

Добавляем внешний ключ в уже существующую таблицу
```sql
ALTER TABLE book
CONSTRAINT FK_book_publisher FOREIGN KEY (publisher_id) REFERENCES publisher(publisher_id)
```

Удаляем внешний ключ из уже существующей таблицы
```sql
ALTER TABLE book
DROP CONSTRAINT FK_book_publisher
```
