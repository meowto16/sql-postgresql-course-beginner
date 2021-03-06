# DEFAULT. Ограничения

Ограничение `DEFAULT` - устанавливает значение по-умолчанию, если оно не было
задано.

Создадим таблицу с ограничением `DEFAULT`
```sql
CREATE TABLE customer
(
    customer_id serial,
    full_name text,
    status char DEFAULT 'r',

    CONSTRAINT PK_customer_customer_id PRIMARY KEY(customer_id)
    CONSTRAINT CHK_customer_status CHECK (status = 'r' OR status = 'p')
)
```

Уберем ограничение `DEFAULT` у столбца `status` таблицы `customer`
```sql
ALTER TABLE customer
ALTER COLUMN status DROP DEFAULT
```

Поставим ограничение на существующий столбец
```sql
ALTER TABLE customer
ALTER COLUMN status SET DEFAULT 'r'
```


