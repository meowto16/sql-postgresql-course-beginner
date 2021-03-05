# Домашняя работа. Соединения (ответы)

1. (**Решено верно**) Найти заказчиков и обслуживающих их заказы сотрудников таких, что и заказчики и сотрудники из города London, а доставка идёт компанией Speedy Express. Вывести компанию заказчика и ФИО сотрудника.
```sql
SELECT customers.company_name, 
       employees.last_name || ' ' ||  employees.first_name AS employee_fio -- Можно было использовать CONCAT
FROM orders -- Можно было присвоить алиасы, чтобы было короче, AS o
INNER JOIN employees USING(employee_id) -- Можно было писать просто JOIN
INNER JOIN customers USING(customer_id)
INNER JOIN shippers ON shippers.shipper_id = orders.ship_via
WHERE customers.city = 'London' 
AND employees.city = 'London'
AND shippers.company_name = 'Speedy Express'
```
2. (**Решено верно, но можно лучше**) Найти активные (см. поле discontinued) продукты из категории Beverages и Seafood, которых в продаже менее 20 единиц. Вывести наименование продуктов, кол-во единиц в продаже, имя контакта поставщика и его телефонный номер.
```sql
SELECT products.product_name, 
       products.units_in_stock, 
	   suppliers.contact_name, 
	   suppliers.phone
FROM products
LEFT JOIN categories USING(category_id) -- Нужно было использовать INNER JOIN, можно было использовать алиасы
LEFT JOIN suppliers USING(supplier_id) -- Нужно было использовать INNER JOIN, можно было использовать алиасы
WHERE products.discontinued = 0
AND categories.category_name IN('Beverages', 'Seafood')
AND products.units_in_stock < 20
-- Можно было добавить ORDER BY units_in_stock
```
3. (**Решено верно, но можно лучше**) Найти заказчиков, не сделавших ни одного заказа. Вывести имя заказчика и order_id.
```sql
-- Можно было искать просто по WHERE order_id IS NULL
SELECT customers.company_name, COUNT(orders.order_id) as orders_count
FROM customers
LEFT JOIN orders USING(customer_id)
GROUP BY customers.company_name
HAVING COUNT(orders.order_id) = 0
```
4. (**Решено верно**) Переписать предыдущий запрос, использовав симметричный вид джойна (подсказка: речь о LEFT и RIGHT).
```sql
SELECT customers.company_name, COUNT(orders.order_id) as orders_count
FROM orders
RIGHT JOIN customers USING(customer_id)
GROUP BY customers.company_name
HAVING COUNT(orders.order_id) = 0
```
