База данных чаще всего содержит одну или несколько таблиц. Каждая таблица обозначается именем (например, «Клиенты» или «Заказы»). Таблицы содержат записи (строки) с данными.
# Основы синтаксиса

1) Чтобы вывести таблицу на экран, либо ее отдельные элементы (сроки, столбцы) мы должны прописать код, используя такие методы как SELECT и FROM (не чувствительны к регистру).

```
SELECT *
FROM Orders
```

2) Чтобы вывести определенную колонку таблицы необходимо прописать ее название, либо несколько названий столбцов

```
Select ProdID, CustomerID
From Orders
```


3) Бывают ситуации, когда необходимо работать с несколькими таблицами, тогда после каждого названия таблицы необходимо указать сокращенное название и уже обращаться к нему при наборе названия колонок. Также сокращенное название можно записать через алиас as

```
SELECT o.ProdID, o.CustomerID
FROM Orders o
```

4) Для вывода первых N строк в разных БД используется различные операторы и конструкции. Операторы SQL не чувствительны к регистру, стоит обращать внимание на регистр при использовании таблиц и названия колонок.

```
SELECT *
FROM orders o
LIMIT 10
```

5) Полезен, когда хотим найти неповторяющиеся значения. Но применять нужно аккуратно, только к небольшим таблицам и к малому кол-ву столбцов
```
select distinct title, gender
from employees
```
6) Если мы хотим вывести/выбрать данные по определенному условию, то применяют оператор **WHERE**. К примеру, мы хотим вывести клиентов с конкретным именем, либо клиентов чей возраст более 30 лет и так далее.
В примере ниже мы вывели всех клиентов, которые имеют имя Sam

```
SELECT *
FROM Orders
WHERE FirstName = 'Sam'
```
Выведем зарплаты сотрудников (только 5 строк), которые меньше 100000 при помощи WHERE
```
SELECT *
from salaries
limit 5;
```

Выведем зарплаты сотрудников (только 5 строк), которые больше 10000 и меньше 50000
```
SELECT *
from salaries
where salary BETWEEN 10000 and 50000
limit 5;
```

```
SELECT *
from employees
where (gender = 'M' and title='Staff') or (last_name = 'Facello')
limit 5;
```
6) **LIMIT
Выводит n-строк. Также можно задать **номер строки** и **кол-во последующих выводов**

- LIMIT offset, count (offset начинается с 0 позиции!!!)

Выведем **вторую по счету максимальную зарплату**. Перед этим произвели сортировку по убыванию при помощи ORDER BY и DESC

```
select salary
from salaries
order by salary desc
limit 1, 1
```

Если мы хотим вывести **третью по счету максимальную зарплату** сотрудника

```
select salary
from salaries
order by salary desc
limit 2, 1;
```

8) **ORDER BY** сортирует данные в порядке убывания или возрастания, также может использоваться совместно и с другими операторами, например GROUP BY, OVER().

По умолчанию сортировка происходит по возрастанию, чтобы сделать по убыванию, необходимо добавить DESC после необходимых колонок.
В примере ниже мы отсортировали колонку ProductId по убыванию.

```
SELECT *
FROM Orders
ORDER BY ProductID DESC
```

Выведем зарплаты порядке убывания (первые три строки)
```
select distinct salary
from salaries
order by salary desc
limit 5;
```
9) **LIKE**
LIKE используется, если хотим найти **строки** в столбце, которые содержат искомую **подстроку**.
Если перед/после подстрокой/ки могут быть любые другие символы, то используем %.

> 'abc%' - Любые строки, которые **начинаются** с букв «abc»
> 
> 'abc_' - Строки длиной **строго 4 символа** (смотрим по общему кол-ву символов в подстроке), первыми символами строки должны быть «abc»
> '%abc' - Любая последовательность символов, которая обязательно **заканчивается** «abc»
> '%abc%' - Любая последовательность символов, содержащая слово «abc» в **любой позиции** строки
> '% % %' - Текст, содержащий не менее 2-х пробелов (считаем по кол-ву пробелов между %), например, "Hello world !"
```
select distinct last_name
from employees
where last_name like 'be%'
limit 3
```
10) **IN**
```
select *
from employees
where birth_date in ('1957-05-23', '1959-10-01')
limit 10
```
11) Из даты **типа TEXT** вы можете получить месяц/день год и так далее при помощи метода **strftime**
```
select *, strftime('%m', birth_date) as month
from employees
limit 3
```

```
select *, strftime('%d', birth_date) as day
from employees
limit 10
```
12) Агрегатные функции – это функции, которые выполняют на наборе данных
арифметические вычисления и возвращают итоговое значение.
Если хотим например посмотреть среднюю зп по департаменту, то тут уже нужно
применять с GROUP BY, если просто среднюю зп по всем департаментам, то в SELECT
используем только одно поле и применяем к нему функцию select AVG(salary) …
• SUM – возвращает сумму значений в столбце;
• COUNT —вычисляет количество значений в столбце (значения NULL не
учитываются);
• AVG —среднее значение в столбце;
• MAX —максимальное значение в столбце;
• MIN —минимальное значение в столбце

```
SELECT COUNT(ProductID)
FROM Orders
```

13) В SQL подзапросы (внутренние/вложенные запросы) — это запрос внутри другого запроса SQL, который вложен в условие WHERE.

• Подзапрос используется для возврата данных, которые будут использоваться в основном запросе в качестве условия для дальнейшей фильтрации данных, подлежащих извлечению.
• Подзапросы могут использоваться с инструкциями SELECT, INSERT, UPDATE и DELETE вместе с операторами типа =, <,>,> =, <=, IN, BETWEEN и т. д. Подзапросы должны быть заключены в круглые скобки.
Пример: необходимо вывести все строки с заказами, где цена продукта, больше чем средняя цена по всем заказам.

```
SELECT *
FROM Orders
WHERE Price >(
	SELECT AVG(Price)
	FROM Orders
)
```

Вывести вторую по счету максимальную зарплату сотрудников. Задача аналогичная как и с limit, но решается другим способом

```
select max(salary) as max_salary
from salaries
where salary not in (
    -- число
    select max(salary)
    from salaries
)
```

11) Соединение таблиц производится при помощи оператора JOIN. ON – позволяет прописать поля, по которым мы хотим соединить таблицы. В конкретном примере, мы соединили только те строки, которые присутствуют в обеих таблицах при помощи INNER JOIN (INNER ó JOIN). Если нам необходимо добавить условия по полям, то используют OR/AND.

```
SELECT c.CustomerID, o.ProductID, o.Counts, o.FirstName, o.Price
FROM Orders o
INNER JOIN Customers c ON c.FirstName = o.FirstName
```

Добавим в таблицу employees с сотрудниками их значение зарплаты из таблицы salaries (используем as - алиас для сокращения).

```
SELECT e.*, s.salary
from employees as e
left join salaries as s on e.emp_no = s.emp_no
limit 5;
```

Если хотим задать дополнительное условие, например вывести только инженеров, то также прописваем WHERE

```
SELECT e.*, s.salary
from employees as e
left join salaries as s on e.emp_no = s.emp_no
where e.title = 'Engineer'
limit 5;
```
- 1) способ. Находим по названию title
```
SELECT e.*, s.salary
from employees as e
left join salaries as s on e.emp_no = s.emp_no
where e.title = 'Manager'
limit 5;
```
Попробуем найти зарплату только у менеджеров и соединить необходимые таблицы

- 2) способ. Соединим с таблицей dept_manager, где содержатся только менеджеры. Такой способ предпочтительнее, если в title могут быть отличные от Manager названия специализаций (а также с ошибками)
```
SELECT e.*, m.dept_name, s.salary
from employees as e
inner join dept_manager m on e.emp_no = m.emp_no
left join salaries as s on m.emp_no = s.emp_no
limit 5;
```
## Логический порядок операций
Логический порядок операций для SQL-запроса:
1. FROM, включая JOINs
2. WHERE
3. GROUP BY
4. HAVING
5. Функции WINDOW (оконные функции)
6. SELECT
7. DISTINCT
8. UNION
9. ORDER BY
10. LIMIT и OFFSET

https://habr.com/ru/articles/461567/


[[Вторая ступень. Основы Python и SQL._Content]]