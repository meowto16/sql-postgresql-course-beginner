# Реляционная модель и SQL

## Реляционные СУБД
- Теоретической основой служит реляционная алгебра
- Реляционная алгебра определяет систему операций над отношениями (таблицами): 
  объединение, пересечение, вычитание, соединение и т.д
- Все эти операции выражаются через SQL

## Реляционная модель
- Сущность — например, клиенты, заказы, поставщики
- Таблица — отношение
- Столбец — атрибут
- Строка / запись — кортеж
- Результирующий набор - результат запроса SQL:
```sql
SELECT contact_name, address, city
FROM customers
LIMIT 13
```

## SQL - Structured Query Language
- SQL — непроцедурный язык и не язык общего назначения
- Если необходимо реализовывать процедурную логику — нужен другой язык, вроде Python, C#, Java и т.д
- Результатом SQL запроса является результирующий набор (как правило - таблица)
- DDL (Data Definition Language) - `CREATE`, `ALTER`, `DROP`
- DML (Data Manipulation Language) - `SELECT`, `INSERT`, `UPDATE`, `DELETE`
- TCL (Transaction Control Language) - `COMMIT`, `ROLLBACK`, `SAVEPOINT`
- DCL (Data Control Language) - `GRANT`, `REVOKE`, `DENY`
- ANSI SQL-92 (стандарт)
- Различия в процедурных расширениях:
  **PL/pgSQL** в PostgreSQL, **PL/SQL** в Oracle, **T-SQL** в MS SQL
