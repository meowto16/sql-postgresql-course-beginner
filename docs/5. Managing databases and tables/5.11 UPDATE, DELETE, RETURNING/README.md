# UPDATE, DELETE, RETURNING

Обновление строк
```sql
UPDATE author
SET full_name = 'Elias', rating = 5
WHERE author_id = 1
```

Удаление строк
```sql
DELETE FROM author
WHERE rating < 4.5
```

Удаление всех строк
```sql
DELETE FROM author
-- Отличается от TRUNCATE тем, что остаются логи
```

Вернуть данные добавленное строки
```sql
INSERT INTO book (title, isbn, publisher_id)
VALUES('title', 'isbn', 3)
RETURNING book_id -- Вернет ID, который был сгенерирован авто инкрементом, как пример.
-- RETURNING * -- Если надо вернуть все данные
```

`RETURNING` можно использовать и при `UPDATE`
```sql
UPDATE author
SET full_name = 'Walter', rating = 5
WHERE author_id = 1
RETURNING author_id - Вернет ID, автора, который был обновлен
```

`RETURNING` можно использовать и при `DELETE`
```sql
DELETE FROM author
WHERE rating = 5
RETURNING *
```
