# GROUPING SET, ROLLUP, CUBE

- `ROLLUP` генерирует агрегатные результаты для выбранных столбцов иерархическим образом.
- `CUBE`, напротив, генерирует агрегатные результаты, которые содержат все возможные комбинации для выбранных столбцов.

Итак, общее правило такое. Если вы имеете иерархические данные (например, страна->штат->город или отдел->менеджер->продавец и т.п.), 
то вам обычно требуются иерархические результаты, и вы используете `ROLLUP` для 
группировки данных.

Если вы имеете неиерархические данные (например, город-пол-национальность), 
то вам не нужны иерархические результаты, поэтому вы используете `CUBE`, 
который обеспечивает все возможные комбинации.

```sql
SELECT *
FROM products;

SELECT supplier_id, SUM(units_in_stock)
FROM products
GROUP BY supplier_id
ORDER BY supplier_id;

SELECT supplier_id, category_id, SUM(units_in_stock)
FROM products
GROUP BY supplier_id, category_id
ORDER BY supplier_id;
```

```sql
SELECT supplier_id, category_id, SUM(units_in_stock)
FROM products
GROUP BY GROUPING SETS ((supplier_id), (supplier_id, category_id))
ORDER BY supplier_id, category_id NULLS FIRST;
```

```sql
SELECT supplier_id, SUM(units_in_stock)
FROM products
GROUP BY ROLLUP(supplier_id)
```

```sql
SELECT supplier_id, category_id, SUM(units_in_stock)
FROM products
GROUP BY ROLLUP (supplier_id, category_id)
ORDER BY supplier_id, category_id NULLS FIRST;
```

```sql
SELECT supplier_id, category_id, reorder_level, SUM(units_in_stock)
FROM products
GROUP BY ROLLUP (supplier_id, category_id, reorder_level)
ORDER BY supplier_id, category_id NULLS FIRST;
```

```sql
SELECT supplier_id, category_id, SUM(units_in_stock)
FROM products
GROUP BY CUBE (supplier_id, category_id)
ORDER BY supplier_id, category_id NULLS FIRST;
```

```sql
SELECT supplier_id, category_id, reorder_level, SUM(units_in_stock)
FROM products
GROUP BY CUBE (supplier_id, category_id, reorder_level)
ORDER BY supplier_id, category_id NULLS FIRST;
```
