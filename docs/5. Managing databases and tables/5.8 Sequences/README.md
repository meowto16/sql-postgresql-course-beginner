# Sequences. Последовательности

`SEQUENCE` – это структура для генерации уникальных целочисленных значений. 

Только одна сессия может запросит следующее значение и таким образом увеличить счётчик. Поэтому все сгенерированные значения будут уникальными.

Механизм `SEQUENCE` не зависит от таблиц, механизма блокировок и транзакций. 

Это значит что `SEQUENCE` может генерировать тысячи уникальных значений в минуту – гораздо быстрее чем методы выборки данных, обновления и подтверждения изменений.

Синтаксис:
```sql
CREATE [ TEMPORARY | TEMP ] SEQUENCE [ IF NOT EXISTS ] name [ INCREMENT [ BY ] increment ]
    [ MINVALUE minvalue | NO MINVALUE ] [ MAXVALUE maxvalue | NO MAXVALUE ]
    [ START [ WITH ] start ] [ CACHE cache ] [ [ NO ] CYCLE ]
    [ OWNED BY { table_name.column_name | NONE } ]
```

Пример: 
```sql
CREATE SEQUENCE seq1;

SELECT nextval('seq1');
SELECT currval('seq1');
SELECT lastval();

SELECT setval('seq1', 16, true);
SELECT currval('seq1');
SELECT nextval('seq1');

CREATE SEQUENCE IF NOT EXISTS seq2  INCREMENT 16;
SELECT nextval('seq2');
```

```sql
CREATE SEQUENCE IF NOT EXISTS seq2 INCREMENT 16;
SELECT nextval('seq2');

CREATE SEQUENCE IF NOT EXISTS seq3
INCREMENT 16
MINVALUE 0
MAXVALUE 128
START WITH 0;

SELECT nextval('seq3');
ALTER SEQUENCE seq3 RENAME TO seq4;
ALTER SEQUENCE seq4 RESTART WITH 16;

DROP SEQUENCE seq4;
```
