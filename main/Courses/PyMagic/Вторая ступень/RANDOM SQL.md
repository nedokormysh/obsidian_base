
1) Таблица Employees. Получить список с информацией обо всех сотрудниках

```
SELECT * FROM employees e
```
2) Таблица Employees. Получить список всех сотрудников с именем 'David'

```
SELECT * FROM employees e
WHERE first_name='DAvid'
```
3) Таблица Employees. Получить список всех сотрудников с job_id равным 'IT_PROG'

```
SELECT first_name, last_name, *
FROM employees
WHERE job_id = 'IT_PROG'
```

4) Таблица Employees. Получить список всех сотрудников из 50го отдела (department_id) с зарплатой(salary), большей 4000

```
SELECT *
FROM employees
WHERE department_id = 50 AND salary > 4000
```

5) Таблица Employees. Получить список всех сотрудников из 20го и из 30го отдела (department_id)

```
SELECT *
FROM employees
WHERE department_id IN (20, 30)
```

6) Таблица Employees. Получить список всех сотрудников у которых последняя буква в имени равна 'a' 

```
SELECT *
FROM employees
WHERE first_name LIKE '%a'
```

7) Таблица Employees. Получить список всех сотрудников из 50го и из 80го отдела (department_id) у которых есть бонус (значение в колонке commission_pct не пустое)

```
SELECT *
FROM employees
WHERE department_id IN (50, 80) AND
	  commission_pct IS NOT NULL
```

8) Таблица Employees. Получить список всех сотрудников у которых в имени содержатся минимум 2 буквы 'n'
```
SELECT *
FROM employees
WHERE first_name LIKE '%n%n%'
```

9) Таблица Employees. Получить список всех сотрудников у которых длина имени больше 4 букв

```
SELECT *
FROM employees
WHERE array_length(first_name, 1) > 4

--- LIKE '%____%'
```

10) Таблица Employees. Получить список всех сотрудников у которых зарплата находится в промежутке от 8000 до 9000 (включительно)

```
SELECT *
FROM employees
WHERE salary in BETWEEN 8000 AND 9000
```

11) Таблица Employees. Получить список всех сотрудников у которых в имени содержится символ '%'
```
SELECT *
FROM employees
WHERE first_name LIKE '%\%%' ESCAPE '\'
```

12) Таблица Employees. Получить список всех ID менеджеров

```
SELECT DISTINCT manager_id
FROM employees
WHERE manager_id IS NOR NULL
```

13) Таблица Employees. Получить список работников с их позициями в формате: Donald(sh_clerk)
```
SELECT first_name || '('|| LOWER (job_id)|| ')' employee 
FROM employees
```