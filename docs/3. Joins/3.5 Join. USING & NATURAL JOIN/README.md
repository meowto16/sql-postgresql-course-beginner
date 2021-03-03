# Соединения. USING & NATURAL JOIN

### USING
`USING` - синтаксический сахар, который позволяет сократить код JOIN'ов. Использовать, если возможно
```sql
SELECT contact_name, company_name, phone, first_name, last_name, title
FROM orders
JOIN order_details USING(order_id) -- Вместо ON orders.order_id = order_details.order_id
JOIN products USING(product_id)
JOIN customers USING(customer_id)
JOIN employees USING(employee_id) -- ON orders.employee_id = employees.employee_id
```

### NATURAL JOIN

- Код не явен, гораздо лучше явно указывать JOIN, по каким столбцам происходит соединение
- Подвержен багам (появились новые столбцы с одинаковыми именами, сложно отлаживать)
- Никогда не используй NATURAL JOIN


```sql
SELECT order_id, customer_id, first_name, last_name, title
FROM orders
NATURAL JOIN employees -- Работает как INNER JOIN, а само соединение проходит по столбцам
-- которые проименованы одинаково
```
