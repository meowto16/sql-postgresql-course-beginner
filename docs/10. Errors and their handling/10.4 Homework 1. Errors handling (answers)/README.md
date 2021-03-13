# Домашняя работа 1. Обработка ошибок (ответы)

### Код
```sql
CREATE OR replace FUNCTION should_increase_salary(
	cur_salary numeric,
	max_salary numeric DEFAULT 80, 
	min_salary numeric DEFAULT 30,
	increase_rate numeric DEFAULT 0.2
	) RETURNS bool AS $$
DECLARE
	new_salary numeric;
BEGIN
    IF min_salary > max_salary THEN
        RAISE EXCEPTION 'min_salary=% greater than max_salary=%', min_salary, max_salary USING HINT='Check out the min_salary input' ERRCODE='13000'
    ELSEIF min_salary < 0
        RAISE EXCEPTION 'min_salary=% less than 0', min_salary USING HINT='min_salary argument should be greater than 0'
    END IF;

	IF cur_salary >= max_salary OR cur_salary >= min_salary THEN 		
		return false;
	END IF;
	
	IF cur_salary < min_salary THEN
		new_salary = cur_salary + (cur_salary * increase_rate);
	END IF;
	
	IF new_salary > max_salary THEN
		RETURN false;
	ELSE
		RETURN true;
	END IF;	
END;
$$ LANGUAGE plpgsql;
```

### Задание (**Решено верно**)
Модифицировать функцию `should_increase_salary` разработанную в секции по функциям таким образом,
чтобы запретить (выбрасывая исключения) передачу аргументов так, что:

- минимальный уровень з/п превышает максимальный
- ни минимальный, ни максимальный уровень з/п не могут быть меньше нуля
- коэффициент повышения зарплаты не может быть ниже 5%

Протестировать реализацию, передавая следующие значения аргументов
(с - уровень "проверяемой" зарплаты, r - коэффициент повышения зарплаты):

- `c` = 79, `max` = 10, `min` = 80, `r` = 0.2
- `c` = 79, `max` = 10, `min` = -1, `r` = 0.2
- `c` = 79, `max` = 10, `min` = 10, `r` = 0.04

### Решение
```sql
CREATE OR replace FUNCTION should_increase_salary(
	cur_salary numeric,
	max_salary numeric DEFAULT 80, 
	min_salary numeric DEFAULT 30,
	increase_rate numeric DEFAULT 0.2
	) RETURNS bool AS $$
DECLARE
	new_salary numeric;
BEGIN
    IF min_salary > max_salary THEN
        RAISE EXCEPTION 'min_salary=%, greater than max_salary=%', min_salary, max_salary USING HINT='Check out the min_salary input', ERRCODE='13000';
    -- Я посчитал, что будет более правильно выбросить EXCEPTION для каждого аргумента по отдельности
    ELSEIF min_salary < 0 THEN
        RAISE EXCEPTION 'min_salary=% less than 0', min_salary USING HINT='min_salary argument should be greater than 0', ERRCODE='13001';
	ELSEIF max_salary < 0 THEN
        RAISE EXCEPTION 'max_salary=% less than 0', max_salary USING HINT='max_salary argument should be greater than 0', ERRCODE='13002';
	ELSEIF increase_rate < 0.05 THEN
		RAISE EXCEPTION 'increate_rate=% less than 0.05', increase_rate USING HINT='increase_rate argument should be greater or equal 0.05', ERRCODE='13003';
    END IF;

	IF cur_salary >= max_salary OR cur_salary >= min_salary THEN 		
		return false;
	END IF;
	
	IF cur_salary < min_salary THEN
		new_salary = cur_salary + (cur_salary * increase_rate);
	END IF;
	
	IF new_salary > max_salary THEN
		RETURN false;
	ELSE
		RETURN true;
	END IF;	
END;
$$ LANGUAGE plpgsql;
```

### Проверка

```sql
SELECT should_increase_salary(79, 10, 80, 0.2);
SELECT should_increase_salary(79, 10, -1, 0.2);
SELECT should_increase_salary(79, 10, 10, 0.04);
```
