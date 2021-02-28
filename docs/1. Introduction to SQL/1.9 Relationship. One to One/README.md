# Отношение один к одному

## Заметки

- Самый часто встречающийся вид отношений.
- "Отношение один к одному", синтаксически ничем не отличается от отношения "Один ко многим".
- В pgAdmin4 можно в ***Query Tool*** выделить определенный кусок SQL запроса и исполнить только его через `F5 (Execute)`


Создаем две БД `person` и `passport`. У каждого паспорта будет один владелец `fk_passport_person`.
```postgresql
CREATE TABLE person
(
	person_id int PRIMARY KEY,
	first_name varchar(64) NOT NULL,
	last_name varchar(64) NOT NULL
);

CREATE TABLE passport
(
	passport_id int PRIMARY KEY,
	serial_number int NOT NULL,
	fk_passport_person int REFERENCES person(person_id)
);

/* Добавляем забытую колонку регистрации */
ALTER TABLE passport
ADD COLUMN registration text NOT NULL;
```

Заполняем БД
```postgresql
INSERT INTO person VALUES (1, 'John', 'Snow');
INSERT INTO person VALUES (2, 'Ned', 'Start');
INSERT INTO person VALUES (3, 'Rob', 'Baratheon');

INSERT INTO passport VALUES (1, 123456, 1, 'Winterfell');
INSERT INTO passport VALUES (2, 789012, 2, 'Winterfell');
INSERT INTO passport VALUES (3, 345678, 3, 'King''s Landing');
```
