# Домашняя работа 1. Представления

1. Создать представление, которое выводит следующие колонки:

    `order_date`, `required_date`, `shipped_date`, `ship_postal_code`, `company_name`, `contact_name`, `phone`, `last_name`, `first_name`, `title` из таблиц `orders`, `customers` и `employees`.

Сделать `SELECT` к созданному представлению, выведя все записи, где `order_date` больше 1го января 1997 года.

```sql
CREATE VIEW orders_customers_employees AS
SELECT order_date, 
	   required_date, 
	   shipped_date, 
	   ship_postal_code, 
	   company_name, 
	   contact_name, 
	   phone,
	   last_name, 
	   first_name, 
	   title
FROM orders
JOIN customers USING(customer_id)
JOIN employees USING(employee_id);

SELECT *
FROM orders_customers_employees
WHERE order_date > '1997-01-01';
```

2. Создать представление, которое выводит следующие колонки:

`order_date`, `required_date`, `shipped_date`, `ship_postal_code`, `ship_country`, `company_name`, `contact_name`, `phone`, `last_name`, `first_name`, `title` из таблиц `orders`, `customers`, `employees`.

Попробовать добавить к представлению (после его создания) колонки `ship_country`, `postal_code` и `reports_to`. Убедиться, что происходит ошибка. 
Переименовать представление и создать новое уже с дополнительными колонками.

Сделать к нему запрос, выбрав все записи, отсортировав их по `ship_county`.

Удалить переименованное представление.

```sql
CREATE VIEW orders_customers_employees AS
SELECT order_date, 
	   required_date, 
	   shipped_date,
	   ship_postal_code,
	   ship_country,
	   company_name,
	   contact_name,
	   phone,
	   last_name,
	   first_name,
	   title
FROM orders
JOIN customers USING(customer_id)
JOIN employees USING(employee_id);

-- Ошибку не поймал, мб что-то не так понял, у нас ведь тут OR REPLACE 
CREATE VIEW OR REPLACE orders_customers_employees AS
SELECT order_date,
       required_date,
       shipped_date,
       ship_postal_code,
       ship_country,
       company_name,
       contact_name,
       phone,
       last_name,
       first_name,
       title,
       postal_code,
       reports_to
FROM orders
         JOIN customers USING(customer_id)
         JOIN employees USING(employee_id);
         
SELECT *
FROM orders_customers_employees
ORDER BY ship_country;

DROP VIEW IF EXISTS orders_customers_employees;
```


3.  Создать представление "активных" (`discontinued` = 0) продуктов, содержащее все колонки. Представление должно быть защищено от вставки записей, в которых `discontinued` = 1.

Попробовать сделать вставку записи с полем `discontinued` = 1 - убедиться, что не проходит.

```sql
CREATE VIEW active_products AS
SELECT *
FROM products
WHERE discontinued = 0
WITH LOCAL CHECK OPTION;

-- ERROR:  new row violates check option for view "active_products"
INSERT INTO active_products (product_name, discontinued)
VALUES('Beach Lazanya', 1)
```
