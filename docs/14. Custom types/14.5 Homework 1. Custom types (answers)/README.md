# Домашняя работа 1. Пользовательские типы (ответы)

1. Переписать функцию, которую мы разработали ранее в одном из ДЗ таким образом, чтобы функция возвращала экземпляр композитного типа. Вот та самая функция:

```sql
CREATE OR REPLACE FUNCTION get_salary_boundaries_by_city(
	emp_city varchar, OUT min_salary numeric, OUT max_salary numeric) 
AS 
$$
	SELECT MIN(salary) AS min_salary,
	   	   MAX(salary) AS max_salary
  	FROM employees
	WHERE city = emp_city
$$ LANGUAGE SQL;
```

### Решение (решено почти верно)

```sql
CREATE TYPE numeric_boundary AS (
  min_numeric numeric,
  max_numeric numeric
);

CREATE OR REPLACE FUNCTION get_salary_boundaries_by_city(emp_city varchar) 
	RETURNS numeric_boundary -- Ошибка: Нужно было RETURNS SETOF numeric_boudary
AS 
$$
	SELECT MIN(salary) AS min_salary,
	   	   MAX(salary) AS max_salary
  	FROM employees
	WHERE city = emp_city
$$ LANGUAGE SQL;
```

2. Задание состоит из пунктов:

Создать перечисление армейских званий США, включающее следующие значения: Private, Corporal, Sergeant

Вывести все значения из перечисления.

Добавить значение Major после Sergeant в перечисление

Создать таблицу личного состава с колонками: person_id, first_name, last_name, person_rank (типа перечисления)

Добавить несколько записей, вывести все записи из таблицы


### Решение (решено верно)
```sql
CREATE TYPE army_rank as ENUM
('Private', 'Corporal', 'Sergeant');

SELECT enum_range(null::army_rank);

ALTER TYPE army_rank
ADD VALUE 'Major' AFTER 'Sergeant';

CREATE TABLE army_staff(
    person_id serial PRIMARY KEY,
    first_name varchar(100),
    last_name varchar(100),
    person_rank army_rank
);

INSERT INTO army_staff(first_name, last_name, person_rank)
VALUES
('Maxim', 'Kirshin', 'Major'),
('Sergey', 'Bondarenko', 'Sergeant'),
('Kirill', 'Vdovin', 'Corporal'),
('Oleg', 'Matyuchenko', 'Private');
```
