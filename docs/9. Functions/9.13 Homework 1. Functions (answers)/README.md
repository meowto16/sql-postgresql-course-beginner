# Домашняя работа 1. Функции (ответы)

1. (**Решено верно**) Создайте функцию, которая делает бэкап таблицы `customers` (копирует все данные в другую таблицу), 
   предварительно стирая таблицу для бэкапа, если такая уже существует (чтобы в случае многократного запуска таблица 
   для бэкапа перезатиралась).
```sql
CREATE OR REPLACE FUNCTION create_backup_customers() RETURNS void AS $$
DROP TABLE IF EXISTS backup_customers;

-- Мой вариант оказался лучшим, преподаватель рассказал о том, что такой бывает
-- Я данный вариант нагуглил
CREATE TABLE backup_customers AS TABLE customers;
$$ LANGUAGE SQL;
```

2. (**Решено верно, но можно лучше**) Создать функцию, которая возвращает средний фрахт (`freight`) по всем заказам
```sql
-- Стоило использовать RETURNS float8, так как AVG() - возвращает float8
CREATE OR REPLACE FUNCTION get_avg_orders_freight() RETURNS real AS $$
SELECT AVG(freight)
FROM orders;
$$ LANGUAGE SQL;
```

3. (**Решено верно**) Написать функцию, которая принимает два целочисленных параметра, используемых как нижняя и 
   верхняя границы для генерации случайного числа в пределах этой границы (включая сами граничные значения).  
   Функция `random` генерирует вещественное число от 0 до 1.  
   Необходимо вычислить разницу между границами и прибавить единицу.  
   На полученное число умножить результат функции `random()` и прибавить к результату значение нижней границы.  
   Применить функцию `floor()` к конечному результату, чтобы не "уехать" за границу и получить целое число.
```sql
CREATE OR REPLACE FUNCTION random_between(min_boundary int, max_boundary int) RETURNS int AS $$
BEGIN
RETURN floor((max_boundary - min_boundary + 1) * random() + min_boundary);
END;
$$ LANGUAGE plpgsql;

```

4. (**Решено верно**) Создать функцию, которая возвращает самые низкую и высокую зарплаты среди сотрудников заданного города
```sql
CREATE OR REPLACE FUNCTION get_employee_extension_range_by_city(city_input varchar, OUT min_extension text, OUT max_extension text) RETURNS SETOF RECORD AS $$
	SELECT MIN(extension), MAX(extension)
	FROM employees
	WHERE city = city_input;
$$ LANGUAGE SQL;
```

5. (**Решено верно, но можно лучше**) Создать функцию, которая корректирует зарплату на заданный процент, но не корректирует зарплату, если её уровень превышает заданный уровень при этом верхний уровень зарплаты по умолчанию равен 70, а процент коррекции равен 15%.
```sql 
-- Можно было решить через обычный LANGUAGE SQL
CREATE OR REPLACE FUNCTION adjust_salary() RETURNS void AS $$
DECLARE
    max_salary int = 70;
    adjustment_percent real = 0.15;
BEGIN
UPDATE employees
SET extension = floor(extension::int * (1 + adjustment_percent))
WHERE extension::int < max_salary;
END;
$$ LANGUAGE plpgsql;
```

6. (**Решено верно, но можно лучше**) Модифицировать функцию, корректирующую зарплату таким образом, чтобы в результате коррекции, 
   она так же выводила бы изменённые записи.
```sql
-- Можно было решить через обычный LANGUAGE SQL
CREATE OR REPLACE FUNCTION adjust_salary() RETURNS SETOF employees AS $$
DECLARE
    max_salary int = 70;
    adjustment_percent real = 0.15;
BEGIN
    RETURN QUERY
        UPDATE employees
        SET extension = floor(extension::int * (1 + adjustment_percent))
        WHERE extension::int < max_salary
		RETURNING *;
END;
$$ LANGUAGE plpgsql;
```

7. (**Решено верно, но можно лучше**) Модифицировать предыдущую функцию так, чтобы она возвращала только колонки `last_name`, `first_name`, `title`, `salary`
```sql
-- Можно было решить через обычный LANGUAGE SQL
CREATE OR REPLACE FUNCTION adjust_salary() 
	RETURNS TABLE(last_name varchar, first_name varchar, title varchar, extension varchar) AS $$
DECLARE
    max_salary int = 70;
    adjustment_percent real = 0.15;
BEGIN
   RETURN QUERY
      UPDATE employees as e
      SET extension = floor(e.extension::int * (1 + adjustment_percent))
      WHERE e.extension::int < max_salary
      RETURNING e.last_name, e.first_name, e.title, e.extension;
END;
$$ LANGUAGE plpgsql;
```

8. (**Решено верно, но можно лучше**) Написать функцию, которая принимает метод доставки и возвращает записи из таблицы `orders` в которых `freight` меньше значения, 
   определяемого по следующему алгоритму:

- ищем максимум фрахта (`freight`) среди заказов по заданному методу доставки
- корректируем найденный максимум на 30% в сторону понижения
- вычисляем среднее значение фрахта среди заказов по заданному методому доставки
- вычисляем среднее значение между средним найденным на предыдущем шаге и скорректированным максимумом
- возвращаем все заказы в которых значение фрахта меньше найденного на предыдущем шаге среднего
```sql
CREATE OR REPLACE FUNCTION get_orders_by_ship_via_with_freight(shipped_via smallint DEFAULT 1) RETURNS SETOF orders AS $$
DECLARE
freight_max real;
	corrected_freight_max real;
	freight_avg real;
	result_freight real;
	freight_correction real = 0.3;
BEGIN
	freight_max := (SELECT MAX(freight) FROM orders WHERE ship_via = shipped_via);
	corrected_freight_max := freight_max * (1 - freight_correction);
	freight_avg := (SELECT AVG(freight) FROM orders WHERE ship_via = shipped_via);
	result_freight := (freight_avg + corrected_freight_max) / 2;
RETURN QUERY
SELECT * FROM orders WHERE freight < result_freight;
END;
$$ LANGUAGE plpgsql;
```

9. (**Решено верно**) Написать функцию, которая принимает:

уровень зарплаты, максимальную зарплату (по умолчанию 80) минимальную зарплату (по умолчанию 30), коээфициет роста зарплаты (по умолчанию 20%)

- Если зарплата выше минимальной, то возвращает `false`
- Если зарплата ниже минимальной, то увеличивает зарплату на коэффициент роста и проверяет не станет ли зарплата после повышения превышать максимальную.
- Если превысит - возвращает `false`, в противном случае `true`.

Проверить реализацию, передавая следующие параметры

(где `c` - уровень з/п, `max` - макс. уровень з/п, `min` - минимальный уровень з/п, `r` - коэффициент):

`c = 40`, `max = 80`, `min = 30`, `r = 0.2` - должна вернуть `false`

`c = 79`, `max = 81`, `min = 80`, `r = 0.2` - должна вернуть `false`

`c = 79`, `max = 95`, `min = 80`, `r = 0.2` - должна вернуть `true`
```sql
-- У меня получилось решить лаконичнее
CREATE OR REPLACE FUNCTION check_salary_level(salary int, max_salary int DEFAULT 80, min_salary int DEFAULT 30, coefficient real DEFAULT 0.2) RETURNS bool AS $$
BEGIN
	IF salary > min_salary THEN RETURN false;
	ELSE
		RETURN (salary * (1 + coefficient)) <= max_salary;
	END IF;
END;
$$ LANGUAGE plpgsql;
```
