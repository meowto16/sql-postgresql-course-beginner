# Введение в PL_pgSQL

```sql
CREATE FUNCTION func_name([arg1, arg2...]) RETURNS data_type AS $$
BEGIN
-- logic
END;
$$ LANGUAGE plpgsql;
```

### Почему PL_pgSQL
- `BEGIN / END` - тело метода (дело не в транзакциях)
- Создание переменных
- Прогон циклов и развитая логика
- Возврат значения через `RETURN` (вместо `SELECT`)
или `RETURN QUERY` (в дополнение к `SELECT`)
