# Обновляемые представления

Создадим представление "тяжелых заказов" (по весу)
```sql
CREATE VIEW heavy_orders AS
SELECT *
FROM orders
WHERE freight > 50;
```

Сделаем обычный `SELECT` к ней
```sql
SELECT *
FROM heavy_orders
ORDER BY freight;
```

Заменим уже созданное представление
```sql
CREATE OR REPLACE VIEW heavy_orders AS
SELECT *
FROM orders
WHERE freight > 100;
```

Переименуем представление с `heavy_orders` на `very_heavy_orders`
```sql
ALTER VIEW heavy_orders RENAME TO very_heavy_orders
```

Если в представлении есть только 1 `FROM`, в такие представление можно `INSERT`ить значения
```sql
INSERT INTO very_heavy_orders
VALUES(11078, 'VINET', 5, '2019-12-10', '2019-12-15', '2019-12-14', 1, 120,
       'Hanari Carnes', 'Rua do Paco', 'Bern', null, 3012, 'Switzerland')
```

Можно удалять значения с помощью представления, но только те, которые присутствуют в представлении
```sql
DELETE FROM very_heavy_orders
WHERE freight < 100.25

-- DELETE FROM very_heavy_orders
-- WHERE freight < 30 
-- Не сработает, так как в представлении у нас все строки начинаются минимум с freight > 100
```
