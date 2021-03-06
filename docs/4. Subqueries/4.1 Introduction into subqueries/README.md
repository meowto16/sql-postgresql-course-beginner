# Введение в подзапросы

- Запросы бывают логически сложными
- Если не получается реализовать запрос сразу и в лоб, попробуйте написать подзапрос, решающий часть задачи
- Бывают запросы, которые без подзапросов реализовать либо невозможно, либо крайне затруднительно
- Зачастую, запрос с подзапросом можно переписать так, чтобы избавиться от подзапроса (обычно с помощью соединения)
- Если можно избавиться от подзапроса - надо ли это делать?
    - не обязательно, зависит от обстоятельств
    - если запрос с подзапросом так же производителен как и запрос с соединением, то надо сравнивать читабельность
    - зачастую, планировщик раскрывает запросы с подзапросами в запросы с соединениями
    - чтобы убедиться, что запрос достаточно производителен - надо опросить планировщика

Все страны, в которых находятся заказчики
```sql
SELECT DISTINCT country
FROM customers
```

Находим всех поставщиков, которые находятся в той же стране что и заказчики
```sql
SELECT company_name
FROM suppliers
WHERE country IN(SELECT DISTINCT country FROM customers)
```

Тоже самое, но без подзапроса
```sql
SELECT DISTINCT suppliers.company_name
FROM suppliers
JOIN customers USING(country)
```

Надуманный пример использования подзапроса. Подзапросы всегда используются в скобках ()
```sql
SELECT category_name, SUM(units_in_stock)
FROM products
INNER JOIN categories USING(category_id)
GROUP BY category_name
ORDER BY SUM(units_in_stock) DESC
LIMIT (SELECT MIN(product_id) + 4 FROM products)
```

Вывести все продукты, которых в наличии больше, чем количество в продаже в среднем
```sql
SELECT product_name, units_in_stock
FROM products
WHERE units_in_stock > (SELECT AVG(units_in_stock) FROM products)
ORDER BY units_in_stock DESC
```
