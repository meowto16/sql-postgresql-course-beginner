# Домашняя работа. Соединения

1. Найти заказчиков и обслуживающих их заказы сотрудников таких, что и заказчики и сотрудники из города London, а доставка идёт компанией Speedy Express. Вывести компанию заказчика и ФИО сотрудника.
```sql
SELECT customers.company_name, 
       employees.last_name || ' ' ||  employees.first_name AS employee_fio
FROM orders
INNER JOIN employees USING(employee_id)
INNER JOIN customers USING(customer_id)
INNER JOIN shippers ON shippers.shipper_id = orders.ship_via
WHERE customers.city = 'London' 
AND employees.city = 'London'
AND shippers.company_name = 'Speedy Express'
```
2. Найти активные (см. поле discontinued) продукты из категории Beverages и Seafood, которых в продаже менее 20 единиц. Вывести наименование продуктов, кол-во единиц в продаже, имя контакта поставщика и его телефонный номер.
```sql
SELECT products.product_name, 
       products.units_in_stock, 
	   suppliers.contact_name, 
	   suppliers.phone
FROM products
LEFT JOIN categories USING(category_id)
LEFT JOIN suppliers USING(supplier_id)
WHERE products.discontinued = 0
AND categories.category_name IN('Beverages', 'Seafood')
AND products.units_in_stock < 20
```
3. Найти заказчиков, не сделавших ни одного заказа. Вывести имя заказчика и order_id.
```sql
SELECT customers.company_name, COUNT(orders.order_id) as orders_count
FROM customers
LEFT JOIN orders USING(customer_id)
GROUP BY customers.company_name
HAVING COUNT(orders.order_id) = 0
```
4. Переписать предыдущий запрос, использовав симметричный вид джойна (подсказка: речь о LEFT и RIGHT).
```sql
SELECT customers.company_name, COUNT(orders.order_id) as orders_count
FROM orders
RIGHT JOIN customers USING(customer_id)
GROUP BY customers.company_name
HAVING COUNT(orders.order_id) = 0
```
