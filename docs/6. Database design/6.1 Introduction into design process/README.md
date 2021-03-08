# Введение в процесс проектирования

### Проблемы с БД:

- **Проблема проектирования БД** - перед тем, как создавать таблицы, необходимо задуматься как будет выглядеть БД.
- **Проблема представления предметной области**
- **Логическое проектирование**

### Проблемы плохого проектирования:

- Возможность записи невалидных данных
- Возможность потери информации (нет нужных связей)
- Отсутствие необходимой информации (забыли то, что было нужно)

### Стадии проектирования БД

- Анализ требования предметной области
- Логическое моделирование данных предметной области
- Физическое проектирование и нормализация

### Анализ требований

- Составление USE CASES (сценарии использования). 
  Понять, кто действующие лица в предметной области, понять чего они хотят.
  Понять, что они делают.
- Аналитический процесс с участием stakeholders (владельцев, экспертов домена).
- Концептуальная схема БД (выделяем сущности, атрибуты, взаимосвязи, характеристики)

### Логическое проектирование

- Детализирует концептуальную модель БД
- Разные источники включают разные компоненты в логическую модель
- Полностью описывает все ключи
- Полностью определяет типы данных (безотносительно конкретной СУБД)
- Полностью описывает все логические ограничения ***(спорно)***
- Нормализация отношений обычно максимум до формы 3НФ

### Физическая модель данных

- Выбирается конкретная СУБД (могла быть выбрана раньше)
- Определяются типы данных
- Определяются индексы
- Могут определяться представления (views)
- Определяются ограничения на доступ (security)

### ER Diagrams

Очень много платных инструментов для моделирования:

- MySQL Workbench
- Oracle SQL Developer Data Modeler
- pgModeler
- SQL Power Architect