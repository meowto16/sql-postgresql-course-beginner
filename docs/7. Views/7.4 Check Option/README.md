# Опция CHECK

Попробуем вставить невалидное значение для `very_heavy_orders`

```sql
INSERT INTO very_heavy_orders
VALUES(11900, 'FOLIG', 1, '2000-01-01', '2000-01-05', '2000-01-04', 1, 
       80, -- freight > 100,
	   'Folies gourmandes', '184, chaussee de Tournai', 'Lille', NULL, 59000, 'France')
```

### Что произойдет

- Строка вставится
- При `SELECT`e из `very_heavy_orders` - мы не найдем данную строку
- Но если сделать `SELECT` из `orders` - мы найдем вставленную строку

### Что делать

В PostgreSQL мы можем сделать так, чтобы фильтр также срабатывал и на `INSERT` данных в представление.
Для этого необходимо прописать `WITH LOCAL CHECK OPTION` при создании представления

Пример:

```sql
CREATE OR REPLACE VIEW very_heavy_orders AS
SELECT *
FROM orders
WHERE freight > 100
WITH LOCAL CHECK OPTION;
```

Также есть еще вариант `WITH CASCADE CHECK OPTION`. Смысл в том, что можно создавать представления от других представлений.
Соответственно проверка будет наследоваться для подлежащих представлений, с этой опцией
