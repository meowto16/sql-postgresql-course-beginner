# Оператор LIMIT

Оператор SQL `LIMIT` позволяет вывести указанное число строк из таблицы.  
Оператор SQL `LIMIT` записывается **всегда в конце запроса**.

Оператор LIMIT имеет следующий синтаксис:

```sql
LIMIT first_row [, last_row]
```

Оператор SQL `LIMIT` выводит то количество записей, которое указано в параметре `first_row`.   
Если, через запятую указано значение параметра `last_row`, то будут выведены строки в диапазоне `first_row` — `last_row` включительно.

```postgresql
SELECT * FROM Universities LIMIT 2 -- вывести первые 2 записи таблицы
```

```sql
SELECT UniversityName FROM Universities LIMIT 4, 6 -- в постгресе не поддерживается

SELECT UniversityName from Universities OFFSET 4 LIMIT 2 -- аналог в постгресе
```
