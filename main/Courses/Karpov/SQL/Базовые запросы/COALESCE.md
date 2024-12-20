Давайте сделаем так, чтобы вместо пустых значений функция `DATE_PART` возвращала какое-нибудь другое значение. В этом нам поможет функция `COALESCE`, которая возвращает первое `не NULL` значение из списка поданных ей на вход аргументов.

Работу `COALESCE` можно описать следующим образом: она буквально читает список значений слева направо и, как только видит значение, которое не является `NULL`, сразу же возвращает его и прекращает чтение списка. Посмотрите внимательно на следующие примеры:

SELECT COALESCE(NULL, 'I am not NULL' , 'karpov.courses')

Результат:
I am not NULL


SELECT COALESCE(NULL, 25, 100, 150)

Результат:
25


SELECT COALESCE('NULL', 'I am not NULL', 'karpov.courses')

Результат:
NULL

## **Задание:**

Как и в предыдущем задании, снова выведите id всех курьеров и их годы рождения, только теперь к извлеченному году примените функцию `COALESCE`. Укажите параметры функции так, чтобы вместо `NULL` значений в результат попадало текстовое значение unknown. Названия полей оставьте прежними.

Отсортируйте итоговую таблицу сначала по убыванию года рождения курьера, затем по возрастанию id курьера.

Поля в результирующей таблице: `courier_id`, `birth_year`

SELECT courier_id,
       coalesce(cast(date_part('year', birth_date) as varchar), 'unknown') as birth_year
FROM   couriers
ORDER BY birth_year desc, courier_id


[[Courses/Karpov/SQL/Базовые запросы/TimeStamp]]
[[Базовые_запросы_content]]