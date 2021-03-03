# Соединение SELF JOIN

`SELF JOIN` используется для объединения таблицы с ней самой таким образом, 
будто это две разные таблицы, временно переименовывая одну из них.

```sql
-- Join таблицы на саму себя
SELECT e.first_name || ' ' || e.last_name as employee,
       m.first_name || ' ' || e.last_name as manager
FROM employee
LEFT JOIN employee m ON m.employee_id = e.manager_id
ORDER BY manager
```
