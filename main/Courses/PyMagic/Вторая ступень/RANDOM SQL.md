# **Restricting and Sorting Data**


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

# **Using Single-Row Functions to Customize Output**

1) Таблица Employees. Получить список всех сотрудников у которых длина имени больше 10 букв
```
SELECT * 
FROM emloyees
WHERE array_length(first_name, 1) > 10
```

2) Таблица Employees. Получить список всех сотрудников у которых в имени есть буква 'b' (без учета регистра)
```
SELECT * 
FROM emloyees
WHERE first_name LIKE '%b%' OR first_name LIKE '%B%'
```

```
SELECT *
FROM employees
WHERE INSTR (LOWER (first_name), 'b') > 0;
```

3) Таблица Employees. Получить список всех сотрудников зарплата которых кратна 1000

```
SELECT * 
FROM employees
WHERE MOD (slary, 1000) = 0
```

4) Таблица Employees. Получить первое 3х значное число телефонного номера сотрудника если его номер в формате ХХХ.ХХХ.ХХХХ

```
SELECT phone_number, SUBSTR (phone_number, 1, 3) new_phone_number  FROM employees WHERE phone_number LIKE '___.___.____';
```

5) Таблица Departments. Получить первое слово из имени департамента для тех у кого в названии больше одного слова

```
SELECT department_name,
SUBSTR (department_name, 1, INSTR (department_name, ' ')-1) first_word
FROM departments
WHERE INSTR (department_name, ' ') > 0;
```

```
SELECT department_name,
       SUBSTRING(department_name FROM 1 FOR POSITION(' ' IN department_name) - 1) AS first_word
FROM departments
WHERE POSITION(' ' IN department_name) > 0;
```

6) Таблица Employees. Получить имена сотрудников без первой и последней буквы в имени

```
SELECT first_name, SUBSTR (first_name, 2, LENGTH (first_name) - 2) new_name  FROM employees;
```

```
SELECT first_name,
       SUBSTRING(first_name FROM 2 FOR LENGTH(first_name) - 2) AS new_name
FROM employees;
```

7) Таблица Employees. Получить список всех сотрудников у которых последняя буква в имени равна 'm' и длинной имени большей 5ти

```
SELECT first_name,
       SUBSTRING(first_name FROM LENGTH(first_name) FOR 1) AS new_name ='m'
FROM employees
AND LENGTH(first_name) > 5;
```

```
SELECT *  FROM employees WHERE SUBSTR (first_name, -1) = 'm' AND LENGTH(first_name)>5;
```

8) Таблица Dual. Получить дату следующей пятницы

```
SELECT NEXT_DAY (SYSDATE, 'FRIDAY') next_friday FROM DUAL;
```

```
SELECT CURRENT_DATE + (5 - EXTRACT(DOW FROM CURRENT_DATE))::int + 7 * (CASE WHEN EXTRACT(DOW FROM CURRENT_DATE) <= 5 THEN 0 ELSE 1 END) AS next_friday;
```

9) Таблица Employees. Получить список всех сотрудников которые работают в компании больше 17 лет
```
SELECT *  FROM employees WHERE MONTHS_BETWEEN (SYSDATE, hire_date) / 12 > 17;
```

```
SELECT *
FROM employees
WHERE EXTRACT(YEAR FROM AGE(CURRENT_DATE, hire_date)) > 17;
```

```
SELECT *,
       DATE_PART('year', AGE(CURRENT_DATE, hire_date)) AS years_since_hire
FROM employees
WHERE DATE_PART('year', AGE(CURRENT_DATE, hire_date)) > 17;
```

10) Таблица Employees. Получить список всех сотрудников у которых последня цифра телефонного номера нечетная и состоит из 3ех чисел разделенных точкой

```
SELECT *
FROM employees
WHERE     MOD (SUBSTR (phone_number, -1), 2) != 0       AND
		  INSTR (phone_number,'.',1,3) = 0       AND
		  INSTR (phone_number,'.',1,2) > 0;
```

```
SELECT *
FROM employees
WHERE MOD(SUBSTRING(phone_number FROM LENGTH(phone_number) FOR 1)::int, 2) != 0
  AND POSITION('.' IN phone_number) > 0
  AND POSITION('.' IN SUBSTRING(phone_number FROM POSITION('.' IN phone_number) + 1)) = 0;
```

12) Таблица Employees. Получить список всех сотрудников у которых в значении job_id после знака '_' как минимум 3 символа но при этом это значение после '_' не равно 'CLERK'
```
SELECT *  FROM employees WHERE     LENGTH (SUBSTR (job_id, INSTR (job_id, '_') + 1)) > 3       AND SUBSTR (job_id, INSTR (job_id, '_') + 1) != 'CLERK';
```

```
SELECT *
FROM employees
WHERE LENGTH(SUBSTRING(job_id FROM POSITION('_' IN job_id) + 1)) > 3
  AND SUBSTRING(job_id FROM POSITION('_' IN job_id) + 1) != 'CLERK';
```

13) Таблица Employees. Получить список всех сотрудников заменив в значении PHONE_NUMBER все '.' на '-'
```
SELECT phone_number, REPLACE (phone_number, '.', '-') new_phone_number  FROM employees;
```

```
SELECT phone_number,
       REPLACE(phone_number, '.', '-') AS new_phone_number
FROM employees;
```