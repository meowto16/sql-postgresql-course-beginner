# Перечисление (ENUM)

- Перечисление служит заменой элементарной справочной таблицы
```sql
CREATE TYPE type_name AS ENUM ('value1', 'value2', ...);
```
- Между собой `enum` сравнивать нельзя
- Накладывает ограничение (в этом смысл перечисления) на добавление в колонку типа перечисления
значения, отсутствующего в перечислении
- Значения регистрозависимы

Решение через отдельную таблицу
```sql
CREATE TABLE chess_title (
	title_id serial PRIMARY KEY,
	title text
);

CREATE TABLE chess_player (
	player_id serial PRIMARY KEY,
	first_name text,
	last_name text,
	title_id int REFERENCES chess_title(title_id)
);

INSERT INTO chess_title(title)
VALUES
('Candidate Master'),
('FIDE Master'),
('International Master'),
('Grand Master');

SELECT * FROM chess_title;

INSERT INTO chess_player(first_name, last_name, title_id)
VALUES
('Wesley', 'So', 4),
('Vladimir', 'Kramnik', 4),
('Vasiliy', 'Pupkin', 1);

SELECT *
FROM chess_player
JOIN chess_title USING (title_id);

DROP TABLE chess_title CASCADE; -- Удалит все зависимые таблицы
DROP TABLE chess_player;
```

Решение через Enum
```sql
CREATE TYPE chess_title AS ENUM
('Candidate Master', 'FIDE Master', 'International Master');

SELECT enum_range(null::chess_title); -- лайфхак, чтобы посмотреть enum

ALTER TYPE chess_title
ADD VALUE 'Grand Master' AFTER 'International Master';

CREATE TABLE chess_player (
	player_id serial PRIMARY KEY,
	first_name text,
	last_name text,
	title chess_title
);

-- Успешно вставится, такой ENUM есть
INSERT INTO chess_player(first_name, last_name, title)
VALUES
('Magnus', 'Carlsen', 'Grand Master');

-- ERROR:  invalid input value for enum chess_title: "wrong value"
INSERT INTO chess_player(first_name, last_name, title)
VALUES
('Magnus', 'Carlsen', 'wrong value');

SELECT *
FROM chess_player;
```

От себя

- Похожи на строковые литералы из Typescript
