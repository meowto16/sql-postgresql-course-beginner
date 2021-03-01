# Домашнее задание 1. Простые выборки

## Задание

1. Выбрать все данные из таблицы customers
```postgresql
SELECT *
FROM customers
```
2. Выбрать все записи из таблицы customers, но только колонки "имя контакта" и "город"
```postgresql
SELECT contact_name, city
FROM customers
```
3. Выбрать все записи из таблицы orders, но взять две колонки: 
   идентификатор заказа и колонку, значение в которой мы рассчитываем 
   как разницу между датой отгрузки и датой формирования заказа.
```postgresql
SELECT order_id, shipped_date - order_date AS days_between_shipped_and_order
FROM orders
```
4. Выбрать все уникальные города в которых "зарегистрированы" заказчики
```postgresql
SELECT DISTINCT city
FROM customers
```
5. Выбрать все уникальные сочетания городов и стран в которых "зарегистрированы" заказчики
```postgresql
SELECT DISTINCT country, city
FROM customers
```
6. Посчитать кол-во заказчиков
```postgresql
SELECT COUNT(*)
FROM customers
```
7. Посчитать кол-во уникальных стран в которых "зарегистрированы" заказчики
```postgresql
SELECT COUNT(DISTINCT country)
FROM customers
```
