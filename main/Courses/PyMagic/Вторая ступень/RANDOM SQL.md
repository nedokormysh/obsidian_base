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

7)   Таблица Employees. Получить список department_id в котором работают больше 30 сотрудников
```
SELECT department_id, COUNT(*)
FROM employees
GROUP BY department_id
HAVING COUNT(*) > 30
```

```
  SELECT department_id    FROM employeesGROUP BY department_id  HAVING COUNT (*) > 30;
```

8) Таблица Employees. Получить список department_id и округленную среднюю зарплату работников в каждом департаменте.

```
SELECT department_id, ROUND(AVG(salary), 2)
FROM employees
GROUP BY department_id
```

```
  SELECT department_id, ROUND (AVG (salary)) avg_salary    FROM employeesGROUP BY department_id;
```

9) Таблица Countries. Получить список region_id сумма всех букв всех country_name в котором больше 60ти

```
SELECT region_id
FROM countries
GROUP BY region_id
HAVING SUM(array_length(country_name), 1))> 60
```

```
  SELECT region_id    FROM countriesGROUP BY region_id  HAVING SUM (LENGTH (country_name)) > 60;
```

10) Таблица Employees. Получить список department_id в котором работают работники нескольких (>1) job_id

```
SELECT department_id
FROM employees
GROUP BY department_id
HAVING COUNT (DISTINCT job_id) > 1;
```

```
  SELECT department_id    FROM employeesGROUP BY department_id  HAVING COUNT (DISTINCT job_id) > 1;
```

11) Таблица Employees. Получить список manager_id у которых количество подчиненных больше 5 и сумма всех зарплат его подчиненных больше 50000

```
SELECT manager_id
FROM employees
GROUP BY manager_id
HAVING COUNT (*) > 5 AND SUM (salary) > 50000;
```

12) Таблица Employees. Получить список manager_id у которых средняя зарплата всех его подчиненных находится в промежутке от 6000 до 9000 которые не получают бонусы (commission_pct пустой)

```
SELECT manager_id, AVG (salary) avg_salary
FROM employees
WHERE commission_pct IS NULL
GROUP BY manager_id
HAVING AVG (salary) BETWEEN 6000 AND 9000;
```

13) Таблица Employees. Получить максимальную зарплату из всех сотрудников job_id которых заканчивается на слово 'CLERK'

```
SELECT MAX(salary) max_salary
FROM employees
WHERE job_id LIKE "%CLERK"
```

14) Таблица Employees. Получить максимальную зарплату среди всех средних зарплат по департаменту
```
SELECT MAX(AVG(salary))
FROM employees
GROUP BY department_id
```

15) Таблица Employees. Получить количество сотрудников с одинаковым количеством букв в имени. При этом показать только тех у кого длина имени больше 5 и количество сотрудников с таким именем больше 20. Сортировать по длинне имени

```
SELECT LENGTH (first_name), COUNT (*)
FROM employees
GROUP BY LENGTH (first_name)
HAVING LENGTH (first_name) > 5 AND COUNT (*) > 20
ORDER BY LENGTH (first_name);

SELECT LENGTH (first_name), COUNT (*)
FROM employees
WHERE LENGTH (first_name) > 5
GROUP BY LENGTH (first_name)
HAVING COUNT (*) > 20
ORDER BY LENGTH (first_name);
```

# **Displaying Data from Multiple Tables Using Joins**

1) Таблица Employees, Departaments, Locations, Countries, Regions. Получить список регионов и количество сотрудников в каждом регионе

```
SELECT r.region_id, r.region_name, COUNT(*) as cnt
FROM employees e
JOIN departments d ON e.departmet_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
JOIN countries c ON l.country_id = c.country_id
JOIN regions r ON c.region_id = r.region_id
GROUP BY region_id, region_name
```

```
 SELECT region_name, COUNT (*)    FROM employees e         JOIN departments d ON (e.department_id = d.department_id)         JOIN locations l ON (d.location_id = l.location_id)         JOIN countries c ON (l.country_id = c.country_id)         JOIN regions r ON (c.region_id = r.region_id)GROUP BY region_name;
```

2) Таблица Employees, Departaments, Locations, Countries, Regions. Получить детальную информацию о каждом сотруднике:  First_name, Last_name, Departament, Job, Street, Country, Region
```
SELECT  
	e.first_name,
	e.last_name,
	d.department_name,
	e.job_id,
	l.street_address,
	c.country_name,
	r.region_name		
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
JOIN countries c ON l.country_id = c.country_id
JOIN regions r ON c.region_id = r.region_id
```

```
SELECT First_name,       Last_name,       Department_name,       Job_id,       street_address,       Country_name,       Region_name  FROM employees  e       JOIN departments d ON (e.department_id = d.department_id)       JOIN locations l ON (d.location_id = l.location_id)       JOIN countries c ON (l.country_id = c.country_id)       JOIN regions r ON (c.region_id = r.region_id);
```

3) Таблица Employees. Показать всех менеджеров которые имеют в подчинении больше 6ти сотрудников

```
SELECT manager_id, COUNT (*)
FROM employees e
	JOIN employees m ON (e.manager_id = m.employee_id)
GROUP BY m.manager_id
HAVING COUNT (*) > 6
```

```
SELECT man.first_name, COUNT (*)
FROM employees emp
	JOIN employees man ON (emp.manager_id = man.employee_id)
GROUP BY man.first_name
HAVING COUNT (*) > 6;
```

4) Таблица Employees. Показать всех сотрудников которые ни кому не подчиняются

```
SELECT emp.first_name
FROM employees  emp
LEFT JOIN employees man ON (emp.manager_id = man.employee_id)
WHERE man.FIRST_NAME IS NULL;
```

```
SELECT first_name
FROM employees
WHERE manager_id IS NULL;
```

5) Таблица Employees, Job_history. В таблице Employee хранятся все сотрудники. В таблице Job_history хранятся сотрудники которые покинули компанию. Получить репорт о всех сотрудниках и о его статусе в компании (Работает или покинул компанию с датой ухода)  
Пример:  
first_name | status  
Jennifer | Left the company at 31 of December, 2006  
Clara | Currently Working

```
SELECT  e.first_name || 
		CASE WHEN end_date IS NULL THEN 'Left the company at ' ||
									    TO_CHAR(jh.end_date, 'DD of Month, YYYY')
			 ELSE 'Currently working'
		END AS status
		
FROM employees e
JOIN job_history jh ON e.employee_id = jh.employee_id
```

```
SELECT first_name,       NVL2 (           end_date,           TO_CHAR (end_date, 'fm""Left the company at"" DD ""of"" Month, YYYY'),           'Currently Working')           status  FROM employees e LEFT JOIN job_history j ON (e.employee_id = j.employee_id);
```

6) Таблица Employees, Departaments, Locations, Countries, Regions. Получить список сотрудников которые живут в Europe (region_name)

```
SELECT *
FROM employees e
JOIN departments d ON e.department_id = d.department_id
JOIN locations l ON d.location_id = l.location_id
JOIN countries c ON l.country_id = c.country_id
JOIN regions r ON c.region_id = r.region_id
WHERE r.region_name = 'Europe'
```

```
SELECT first_name  FROM employees       JOIN departments USING (department_id)       JOIN locations USING (location_id)       JOIN countries USING (country_id)       JOIN regions USING (region_id) WHERE region_name = 'Europe';

SELECT first_name  FROM employees  e       JOIN departments d ON (e.department_id = d.department_id)       JOIN locations l ON (d.location_id = l.location_id)       JOIN countries c ON (l.country_id = c.country_id)       JOIN regions r ON (c.region_id = r.region_id) WHERE region_name = 'Europe';
```

7) Таблица Employees, Departaments. Показать все департаменты в которых работают больше 30ти сотрудников

```
SELECT department_id, COUNT(employee_id)
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY department_id
HAVING COUNT(employee_id) > 30
```

```
SELECT department_name, COUNT (*)    FROM employees e JOIN departments d ON (e.department_id = d.department_id)GROUP BY department_name  HAVING COUNT (*) > 30;
```

8) Таблица Employees, Departaments. Показать всех сотрудников которые не состоят ни в одном департаменте

```
SELECT employee_id
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id
WHERE department_name IS NULL
```

```
SELECT first_name
FROM employees  e
LEFT JOIN departments d ON (e.department_id = d.department_id)
WHERE d.department_name IS NULL;

SELECT first_name
FROM employees
WHERE department_id IS NULL;
```

9) Таблица Employees, Departaments. Показать все департаменты в которых нет ни одного сотрудника

```
SELECT depatment_id
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id
GROUP BY depatment_id
HAVING COUNT(e.employee_id) = 0
```

```
SELECT
    d.department_id,
    d.department_name
FROM
    departments d
LEFT JOIN
    employees e ON d.department_id = e.department_id
WHERE
    e.employee_id IS NULL;

```

```
SELECT department_name
FROM employees  e
RIGHT JOIN departments d ON (e.department_id = d.department_id)
WHERE first_name IS NULL;
```

10) Таблица Employees. Показать всех сотрудников у которых нет ни кого в подчинении

```
SELECT
    man.first_name,
    man.last_name
FROM
    employees man
LEFT JOIN
    employees emp ON man.employee_id = emp.manager_id
WHERE
    emp.employee_id IS NULL;

```

```
SELECT man.first_name
FROM employees  emp
RIGHT JOIN employees man ON (emp.manager_id = man.employee_id)
WHERE emp.FIRST_NAME IS NULL;
```

11) Таблица Employees, Jobs, Departaments. Показать сотрудников в формате: First_name, Job_title, Department_name.  
Пример:  
First_name | Job_title | Department_name  
Donald | Shipping | Clerk Shipping

