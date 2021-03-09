# Домашняя работа 1. Логические операторы (ответы)

1. (**Решено верно**) Выполните следующий код (записи необходимы для тестирования корректности выполнения ДЗ):

```sql
insert into customers(customer_id, contact_name, city, country, company_name)
values
('AAAAA', 'Alfred Mann', NULL, 'USA', 'fake_company'),
('BBBBB', 'Alfred Mann', NULL, 'Austria','fake_company');
```

После этого выполните задание:
Вывести имя контакта заказчика, его город и страну, отсортировав по возрастанию по имени контакта и городу,
а если город равен `NULL`, то по имени контакта и стране. Проверить результат, используя заранее вставленные строки.

```sql
SELECT contact_name, city, country
FROM customers
-- Преподаватель сделал через CASE WHEN, но мне кажется COALESCE здесь больше подходит
ORDER BY contact_name, COALESCE(city, country)
```

2. (**Решено верно**) Вывести наименование продукта, цену продукта и столбец со значениями

`too expensive` если цена >= 100
`average` если цена >=50 но < 100
`low price` если цена < 50

```sql
SELECT product_name, unit_price,
	CASE WHEN unit_price >= 100 THEN 'too expensive'
		 WHEN unit_price >= 50 AND unit_price < 100 THEN 'average'
		 WHEN unit_price < 50 THEN 'low price'
		 ELSE 'unknown'
	END AS price_definition
FROM products;
```

3. (**Решено верно**) Найти заказчиков, не сделавших ни одного заказа. Вывести имя заказчика и значение `no orders` если `order_id` = `NULL`.

```sql
SELECT company_name,
       -- Думаю преподаватель решит по-другому, загуглил каст значений, так как иначе была ошибка
       -- UPD. Да, преподаватель точно также привел просто к другому типу значение
       COALESCE(order_id::varchar, 'no orders') AS order_id
FROM customers
LEFT JOIN orders USING(customer_id)
WHERE order_id IS NULL
```

4. (**Решено верно**) Вывести ФИО сотрудников и их должности. В случае если должность = `Sales Representative` вывести вместо неё `Sales Stuff`.
```sql
SELECT 
	CONCAT(last_name, ' ', first_name) AS fio,
	COALESCE(NULLIF(title, 'Sales Representative'), 'Sales Stuff') AS title
FROM employees
```
