# Создание представлений

Создадим представление
```sql
CREATE VIEW products_suppliers_categories AS
SELECT product_name, 
       quantity_per_unit, 
	   unit_price, 
	   units_in_stock, 
	   company_name, 
	   contact_name, 
	   phone, 
	   category_name,
	   description
FROM products
JOIN suppliers USING(supplier_id)
JOIN categories USING(category_id);
```

Можно обращаться как к обычной таблице
```sql
SELECT *
FROM products_suppliers_categories;
```

Ничто не мешает работать с представлением как с обычной таблицей
```sql
SELECT *
FROM products_suppliers_categories
WHERE unit_price > 20;
```

Удалим представление
```sql
DROP VIEW IF EXISTS products_suppliers_categories;
```
