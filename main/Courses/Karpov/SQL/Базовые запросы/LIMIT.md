# Задача 3 LIMIT
 
Сортировать результат SQL-запроса можно сразу по нескольким колонкам, указывая их после `ORDER BY` через запятую вместе с направлением сортировки (`ASC` или `DESC`):

SELECT column_1, column_2
FROM table
ORDER BY column_1 DESC, column_2    -- сначала сортировка по первой колонке (по убыванию), -- затем по второй (по возрастанию)

Для ограничения числа извлекаемых из таблицы записей применяется оператор `LIMIT`.

Записывается он так:

SELECT column
FROM table
LIMIT n 

**На заметку:**

Важно помнить, что при работе с большими таблицами нужно по возможности ограничивать число извлекаемых записей, чтобы не создавать лишнюю нагрузку на базу данных.

Разумеется, операторы `ORDER BY` и `LIMIT` можно совмещать в одном запросе, при этом оператор `LIMIT` записывается и выполняется после оператора `ORDER BY`, ограничивая число записей в уже отсортированном результате:

SELECT column_1, column_2
FROM table
ORDER BY column_1 DESC, column_2
LIMIT 100

## **Задание:**

Отсортируйте таблицу `courier_actions` сначала по колонке `courier_id` по возрастанию id курьера, потом по колонке `action` (снова по возрастанию), а затем по колонке `time`, но уже по убыванию — от самого последнего действия к самому первому. Не забудьте включить в результат колонку `order_id`.

Добавьте в запрос оператор `LIMIT` и выведите только первые 1000 строк результирующей таблицы.

Поля в результирующей таблице: `courier_id`, `order_id`, `action`, `time`

---

SELECT *
FROM   courier_actions
ORDER BY courier_id, action, time desc limit 1000


[[ORDER BY]]
[[Базовые_запросы_content]]

