# CHECK. Логические ограничения

Создадим таблицу
```sql
CREATE TABLE book
(
    book_id int,
    title text NOT NULL,
    isbn varchar(32) NOT NULL,
    publisher_id int,
    
    CONSTRAINT PK_book_book_id PRIMARY KEY(book_id)
)
```

```sql
ALTER TABLE book
ADD COLUMN price decimal CONSTRAINT CHK_book_price CHECK (price > 0)
```

