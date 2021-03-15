# Домашняя работа 2. Массивы

Написать функцию, которая фильтрует телефонные номера по коду оператора.

Принимает 3-х значный код мобильного оператора и список телефонных номеров в формате +1(234)5678901 (variadic)

Функция возвращает только те номера, код оператора которых соответствует значению соответствующего аргумента.

Проверить функцию передав следующие аргументы:

`903`, `+7(903)1901235`, `+7(926)8567589`, `+7(903)1532476`

Попробовать передать аргументы с созданием массива и без.

Подсказка: чтобы передать массив в VARIADIC-аргумент, надо перед массивом прописать, собственно, ключевое слово variadic.

## Решение (решено верно и креативно, но можно лучше)
```sql
-- Можно было проверить входные аргументы на формат
CREATE OR REPLACE FUNCTION get_phone_operator_from_list(phone_code char(3), VARIADIC phones char(14)[]) RETURNS SETOF char(14) AS $$
SELECT phone
FROM UNNEST(phones) AS phone -- UNNEST не проходили, но вариант интересный
WHERE phone LIKE CONCAT('%(', phone_code, ')%'); -- Стоило написать паттерн лучше, как у преподавателя
$$ LANGUAGE sql;

SELECT get_phone_operator_from_list('903', VARIADIC ARRAY['+7(903)1901235', '+7(926)8567589', '+7(903)1532476']);
```

## Решение преподавателя
```sql
CREATE OR REPLACE FUNCTION filter_by_operator(oper int, VARIADIC numbers text[]) RETURNS SETOF text AS
$$
DECLARE
    cur_val text;
BEGIN
    FOREACH cur_val IN ARRAY numbers
    LOOP
        CONTINUE WHEN cur_val NOT LIKE CONCAT('__(', oper, ')%'); -- Не забывать, что тут тоже можно использовать LIKE
        RETURN NEXT cur_val
    END LOOP;
END;
$$ LANGUAGE plpgsql;
```
