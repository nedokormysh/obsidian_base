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

# **Using Conversion Functions and Conditional Expressions**

1) Таблица Employees. Получить список всех сотрудников которые пришли на работу в первый день месяца (любого)

```
SELECT *
FROM employees
WHERE DATE_PART('day', hire_date) = 01
```

```
SELECT *  FROM employees WHERE TO_CHAR (hire_date, 'DD') = '01';
```

2) Таблица Employees. Получить список всех сотрудников которые пришли на работу в 2008ом году

```
SELECT *
FROM employees
WHERE DATE_PART('year', hire_date) = 2008
```

```
SELECT *  FROM employees WHERE TO_CHAR (hire_date, 'YYYY') = '2008';
```

3) Таблица DUAL. Показать завтрашнюю дату в формате: Tomorrow is Second day of January


```
SELECT 'Tomorrow is ' ||
       TO_CHAR(CURRENT_DATE + INTERVAL '1 day', 'DD') || ' day of ' ||
       TO_CHAR(CURRENT_DATE + INTERVAL '1 day', 'Month') AS info;
```

```
SELECT TO_CHAR (SYSDATE, 'fm""Tomorrow is ""Ddspth ""day of"" Month')     info  FROM DUAL;
```

4) Таблица Employees. Получить список всех сотрудников и дату прихода на работу каждого в формате: 21st of June, 2007
```
SELECT first_name, last_name
FROM employees, TO_CHAR (DATE_PART('day', hire_date) || 'th' || DATE_PART('month', hire_date)|| DATE_PART('year',  hire_date)) as hire_date_1
```

```
SELECT
    employee_id,
    first_name,
    last_name,
    TO_CHAR(hire_date, 'DDth of FMMonth, YYYY') AS hire_date_formatted
FROM
    employees;
```

```
SELECT first_name, TO_CHAR (hire_date, 'fmddth ""of"" Month, YYYY') hire_date  FROM employees;
```

5) Таблица Employees. Получить список работников с увеличенными зарплатами на 20%. Зарплату показать со знаком доллара

```
SELECT *,
       (salary * 1.2)::text || '$' AS salary_formatted
FROM employees;
```

```
SELECT first_name, TO_CHAR (salary + salary * 0.20, 'fm$999,999.00') new_salary  FROM employees;
```

6) Таблица Employees. Получить список всех сотрудников которые пришли на работу в феврале 2007го года.
```
SELECT *
FROM employees
WHERE DATE_PART('year', hire_date) = 2007 AND
	  DATE_PART('month', hire_date) = 02
```

```
SELECT *  FROM employees WHERE hire_date BETWEEN TO_DATE ('01.02.2007', 'DD.MM.YYYY')                     AND LAST_DAY (TO_DATE ('01.02.2007', 'DD.MM.YYYY'));

SELECT *  FROM employees WHERE to_char(hire_date,'MM.YYYY') = '02.2007';
```

7) Таблица DUAL. Вывезти актуальную дату, + секунда, + минута, + час, + день, + месяц, + год
```
SELECT
    CURRENT_TIMESTAMP AS current_timestamp,
    CURRENT_TIMESTAMP + INTERVAL '1 second' AS plus_1_second,
    CURRENT_TIMESTAMP + INTERVAL '1 minute' AS plus_1_minute,
    CURRENT_TIMESTAMP + INTERVAL '1 hour' AS plus_1_hour,
    CURRENT_TIMESTAMP + INTERVAL '1 day' AS plus_1_day,
    CURRENT_TIMESTAMP + INTERVAL '1 month' AS plus_1_month,
    CURRENT_TIMESTAMP + INTERVAL '1 year' AS plus_1_year;
```

```
SELECT SYSDATE                          now,       SYSDATE + 1 / (24 * 60 * 60)     plus_second,       SYSDATE + 1 / (24 * 60)          plus_minute,       SYSDATE + 1 / 24                 plus_hour,       SYSDATE + 1                      plus_day,       ADD_MONTHS (SYSDATE, 1)          plus_month,       ADD_MONTHS (SYSDATE, 12)         plus_year  FROM DUAL;
```

8) Таблица Employees. Получить список всех сотрудников с полными зарплатами (salary + commission_pct(%)) в формате: $24,000.00

```
SELECT
    first_name,
    last_name,
    '$' || TO_CHAR(salary + (salary * commission_pct / 100), 'FM999,999,999,999.00') AS whole_salary
FROM
    employees;
```

```
SELECT first_name, salary, TO_CHAR (salary + salary * NVL (commission_pct, 0), 'fm$99,999.00') full_salary  FROM employees;
```

9) Таблица Employees. Получить список всех сотрудников и информацию о наличии бонусов к зарплате (Yes/No)

```
SELECT
    employee_id,
    first_name,
    last_name,
    salary,
    commission_pct,
    CASE
        WHEN commission_pct IS NOT NULL THEN 'Yes'
        ELSE 'No'
    END AS has_bonus
FROM
    employees;
```

```
SELECT first_name, commission_pct, NVL2 (commission_pct, 'Yes', 'No') has_bonus  FROM employees;
```

10) Таблица Employees. Получить уровень зарплаты каждого сотрудника: Меньше 5000 считается Low level, Больше или равно 5000 и меньше 10000 считается Normal level, Больше иои равно 10000 считается High level

```
SELECT first_name, last_name,
	   CASE
		   WHEN salary < 5000 THEN 'Low level'
		   WHEN salary >= 5000 AND salary < 10000 THEN 'Normal level'
		   ELSE 'High level'
	   END AS salary_level
FROM employees
```

```
SELECT first_name,       salary,       CASE           WHEN salary < 5000 THEN 'Low'           WHEN salary >= 5000 AND salary < 10000 THEN 'Normal'           WHEN salary >= 10000 THEN 'High'           ELSE 'Unknown'       END           salary_level  FROM employees;
```

11) Таблица Countries. Для каждой страны показать регион в котором он находится: 1-Europe, 2-America, 3-Asia, 4-Africa (без Join)
```
SELECT country_name AS country,
	CASE region_id
		WHEN 1 THEN 'Europe'
		WHEN 2 THEN 'America' 
		WHEN 3 THEN 'Asia' 
		WHEN 4 THEN 'Africa' 
		ELSE 'Unknown' 
	END AS region FROM countries;
```

```
SELECT country_name country,       DECODE (region_id,               1, 'Europe',               2, 'America',               3, 'Asia',               4, 'Africa',               'Unknown')           region  FROM countries;SELECT country_name           country,       CASE region_id           WHEN 1 THEN 'Europe'           WHEN 2 THEN 'America'           WHEN 3 THEN 'Asia'           WHEN 4 THEN 'Africa'           ELSE 'Unknown'       END           region  FROM countries;
```

# **Reporting Aggregated Data Using the Group Functions**

1) Таблица Employees. Получить репорт по department_id с минимальной и максимальной зарплатой, с ранней и поздней датой прихода на работу и с количествов сотрудников. Сорировать по количеству сотрудников (по убыванию)

```
SELECT department_id, 
	MIN(salary),
	MAX(salary), 
	MIN(hire_date), 
	max(hire_date), 
	COUNT(employee_id) as cnt
FROM employees
GROUP BY department_id
ORDER COUNT(employee_id) as cnt BY DESC
```

```
SELECT department_id,         MIN (salary) min_salary,         MAX (salary) max_salary,         MIN (hire_date) min_hire_date,         MAX (hire_date) max_hire_Date,         COUNT (*) count    FROM employeesGROUP BY department_idorder by count(*) desc;
```

2) Таблица Employees. Сколько сотрудников имена которых начинается с одной и той же буквы? Сортировать по количеству. Показывать только те где количество больше 1

```
SELECT
    LEFT(first_name, 1) AS initial,
    COUNT(*) AS cnt
FROM
    employees
GROUP BY
    LEFT(first_name, 1)
HAVING
    COUNT(*) > 1
ORDER BY
    cnt DESC;
```

```
SELECT SUBSTR (first_name, 1, 1) first_char, COUNT (*)    FROM employeesGROUP BY SUBSTR (first_name, 1, 1)  HAVING COUNT (*) > 1ORDER BY 2 DESC;
```

3) Таблица Employees. Сколько сотрудников которые работают в одном и тоже отделе и получают одинаковую зарплату?
```
SELECT department_id, salary, COUNT(*)
FROM employees
GROUP BY department_id, salary
HAVING COUNT(*) > 1
```

```
SELECT department_id, salary, COUNT (*)    FROM employeesGROUP BY department_id, salary  HAVING COUNT (*) > 1;
```

4) Таблица Employees. Получить репорт сколько сотрудников приняли на работу в каждый день недели. Сортировать по количеству

```
SELECT
    COUNT(employee_id) AS cnt,
    DATE_PART('dow', hire_date) AS day_of_week
FROM
    employees
GROUP BY
    DATE_PART('dow', hire_date)
ORDER BY
    cnt DESC;
```

```
SELECT COUNT(employee_id) AS cnt, TO_CHAR(hire_date, 'Dy') AS day_of_week FROM employees GROUP BY TO_CHAR(hire_date, 'Dy') ORDER BY cnt DESC;
```

```
SELECT TO_CHAR (hire_Date, 'Day') day, COUNT (*)    FROM employeesGROUP BY TO_CHAR (hire_Date, 'Day')ORDER BY 2 DESC;
```

5) Таблица Employees. Получить репорт сколько сотрудников приняли на работу по годам. Сортировать по количеству
```
SELECT COUNT(*) AS cnt, DATE_PART('year', hire_date)
FROM employees
GROUP BY DATE_PART('year', hire_date)
ORDER BY cnt DESC
```

```
SELECT TO_CHAR (hire_date, 'YYYY') year, COUNT (*)    FROM employeesGROUP BY TO_CHAR (hire_date, 'YYYY');
```

6) Таблица Employees. Получить количество департаментов в котором есть сотрудники

```
SELECT COUNT(*) as cnt
FROM employees
GROUP BY department_id
HAVING COUNT(*) IS NOT NULL AND COUNT(*) >= 1
```

```
SELECT COUNT (COUNT (*))     department_count    FROM employees   WHERE department_id IS NOT NULLGROUP BY department_id;
```