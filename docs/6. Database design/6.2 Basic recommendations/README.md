# Базовые рекомендации

### Базовые советы по проектированию

- Таблица: объект, событие, абстракция
- Поле (колонка): свойство объекта
- Запись (строка): совокупность полей
- Значения в каждом поле по отдельности не должны содержать не валидных данных
- Значения в совокупности полей должны быть непротиворечивы

### Плохие практики

- Игнорирование нормализации — избыточность данных
- Отсутствие стандартов именования на проекте
- Одна таблица для разных по смыслу данных
  - Каждая таблица должна выражать некий объект
  - Каждая строка должна выражать некое свойство этого объекта
- Наплевательское отношение к актуальности репрезентации данных
  (домен меняется — это живой механизм)
  
### Еще плохие практики

- Поле, содержащее более 1 логической части (`full_name`). Лучше создать 3 отдельных колонки как пример.
- Поле, содержащее более 1 значения (массив, когда не надо).
- Вычислимое поле (полная зарплата за всё время работы).
- Неправильно выбранные первичные ключи (ИНН — плохой PK). Или серия и номер паспорта
- Избегайте композитных PK (может приводить к деградации производительности)
- В идеале, в таблице кроме суррогатного ключа, должен быть и натуральный
  - ID'шники - это суррогатные ключи. Не имеют смысла в реальном мире
  - Не может заселектить строку без знания суррогатного ключа, лучше пересмотреть подход к проектированию.
- Правила иногда можно нарушать!
  - Вычислимое поле дает улучшение производительности? Делаем вычислимое поле...
