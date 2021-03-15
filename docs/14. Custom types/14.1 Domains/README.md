# Домены

- Домен — пользовательские типы данных с ограничениями
- Составной тип — тип, объединяющий логически взаимосвязанные данные без создания полноценной таблицы
- Перечисление — позволяет эмулировать простейшие справочные таблицы

```sql
CREATE DOMAIN domain_name AS data_type CONSTRAINTS;
```

```sql
CREATE DOMAIN text_no_space_null AS TEXT NOT NULL CHECK (value !~'\s');
```

```sql
ALTER DOMAIN domain_name ADD CONSTRAINT new_constraint [NOT VALID]
ALTER DOMAIN domain_name VALIDATE CONSTRAINT new_constraint
```

Создадим домен
```sql
CREATE DOMAIN text_no_space_null AS TEXT NOT NULL CHECK (value ~ '^(?!\s*$).+');
```

Создадим табличку
```sql
CREATE TABLE agent(
    first_name text_no_space_null,
    last_name text_no_space_null
);
```

Проверим
```sql
-- Отработает
INSERT INTO agent
VALUES('Bob', 'Taylor');

-- Не отработает, value for domain text_no_space_null violates check constraint "text_no_space_null_check"
INSERT INTO agent
VALUES('', 'Taylor');

-- Не отработает, domain text_no_space_null does not allow null values
INSERT INTO agent
VALUES(NULL, 'Taylor');

-- Не отработает, value for domain text_no_space_null violates check constraint "text_no_space_null_check"
INSERT INTO agent
VALUES('     ', 'Taylor');

-- Отработает, регулярка не запрещает пробелы
INSERT INTO agent
VALUES('bob junior', 'Taylor');
```

Удалим таблицу и домен.
```sql
DROP TABLE IF EXISTS agent;
DROP DOMAIN IF EXISTS text_no_space_null;
```

```sql
ALTER DOMAIN text_no_space_null ADD CONSTRAINT text_no_space_null_length32 CHECK (length(value)<=32) NOT VALID;

ALTER DOMAIN text_no_space_null VALIDATE CONSTRAINT text_no_space_null_length32;
```
