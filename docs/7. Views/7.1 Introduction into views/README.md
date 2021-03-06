# Введение в представления

`View` - по своей сути — это сохраненный запрос.
`ORM` - Object Relational Mapper

- `View` - сохраненный запрос в виде объекта БД (виртуальная таблица)
- К `View` можно делать обычный `SELECT`
- `Views` можно соединять и т.д.
- Производительность такая же, как и у обычной таблицы (если сравниваем сопоставимые вещи)

### Что позволяет

- Делать кеширование с помощью материализации
- Сокращать сложные запросы
- Подменить реальную таблицу
- Создавать виртуальные таблицы соединяющие несколько таблиц
- Скрыть логику агрегации данных при работе через ORM
- Скрыть информацию (столбцы) от групп пользователей
- Скрыть информацию на уровне строк от групп пользователей (строки отсекаются самим запросом)

### Виды представлений

- Временные
- Рекурсивные
- Обновляемые
- Материализуемые

### Создание представлений

```sql
CREATE VIEW view_name AS
SELECT select_statement
```

### Изменение представлений

- Можно только добавить новые столбцы
  - нельзя удалить существующие
  - нельзя поменять имена столбцом
  - нельзя поменять даже порядок следования столбцом
    
```sql
CREATE OR REPLACE VIEW view_name AS 
SELECT select_statement
```

### Переименование представлений

```sql
ALTER VIEW old_view_name RENAME TO new_view_name
```

### Модификация данных через представления

- Только одна таблица в FROM
- Нет `DISTINCT`, `GROUP BY`, `HAVING`, `UNION`, `INTERSECT`, `EXCEPT`, `LIMIT`
- Нет оконных функций `MIN`, `MAX`, `SUM`, `COUNT`, `AVG`
- `WHERE` не под запретом

### Удаление представлений

```sql
DROP VIEW [IF EXISTS] view_name
```

### Устройство БД
