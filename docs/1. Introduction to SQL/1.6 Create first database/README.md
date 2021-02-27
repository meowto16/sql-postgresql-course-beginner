# Создание БД

Для того, чтобы сконнектить pgAdmin4 с PostgresSQL нужно поставить пароль для postgres user'a.
https://docs.boundlessgeo.com/suite/1.1.1/dataadmin/pgGettingStarted/firstconnect.html

## Заметки
- UTF-8, в подавляющем большинстве случаев этой кодировки достаточно

#### Пример создания БД через SQL

```postgresql
CREATE DATABASE testdb
    WITH 
    OWNER = postgres
    ENCODING = 'UTF8'
    CONNECTION LIMIT = -1;
```

#### Пример удаления БД через SQL

```postgresql
DROP DATABASE testdb
```
