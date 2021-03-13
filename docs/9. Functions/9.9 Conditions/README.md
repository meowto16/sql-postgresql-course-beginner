# Условия PL/pgSQL

Синтаксис
```sql
IF expression THEN
    -- logic
ELSIF expression THEN -- Можно также писать ELSIF
    -- logic
ELSEIF expression THEN -- Можно также писать ELSEIF
    -- logic
ELSE
    -- logic
END IF;
```

Напишем функцию перевода температуры из фаренгейта в цельсии и обратно
```sql
CREATE FUNCTION convert_temp_to(temperature real, to_celsius bool DEFAULT true) RETURNS real AS $$
DECLARE
	result_temp real;
BEGIN
	IF to_celsius THEN
		result_temp = (5.0/9.0)*(temperature-32);
	ELSE
		result_temp = (9*temperature+(32*5))/5.0;
	END IF;
	RETURN result_temp;
END;
$$ LANGUAGE plpgsql;

SELECT convert_temp_to(80);
SELECT convert_temp_to(26.666667, false);
```

На вход принимает номер месяца, а возвращает время года
```sql
CREATE FUNCTION get_season(month_number int) RETURNS text AS $$
DECLARE
    season text;
BEGIN
    IF month_number BETWEEN 3 AND 5 THEN
        season = 'Spring';
    ELSEIF month_number BETWEEN 6 AND 8 THEN
        season = 'Summer';
    ELSEIF month_number BETWEEN 9 AND 11 THEN
        season = 'Autumng';
    ELSE
        season = 'Winter';
    END IF;
    RETURN season;
END;
$$ LANGUAGE plpgsql;

SELECT get_season(12);
```
