# Полная выборка

Выбрать **все** колонки из таблицы `products`:
```postgresql
SELECT * 
FROM products
```

`*` - Wildcard, выбрать все колонки. Лучше всегда указывать конкретные колонки, которые нужны.

Выбрать **конкретные** колонки из таблицы `products`
```postgresql
SELECT product_id, product_name, unit_price
FROM products
```
