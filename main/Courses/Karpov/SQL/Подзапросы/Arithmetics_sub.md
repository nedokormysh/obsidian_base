
Результат подзапроса, возвращающего столбец с одним значением, также можно использовать в арифметических операциях:

SELECT column
FROM table
WHERE column = (SELECT MAX(column) FROM table) - 100

Более того, с помощью подзапросов вы можете брать необходимые вам значения из нескольких разных таблиц и использовать их в качестве переменных внутри основного запроса:

SELECT column
FROM table
WHERE column >= (SELECT MAX(column_1) FROM table_1) - 100
    AND column <= (SELECT MAX(column_2) FROM table_2)

То есть в данном случае результат выполнения подзапроса можно рассматривать как некоторую переменную. Со временем число записей в таблицах может меняться (допустим, появляются новые товары по новым ценам или новые заказы с более поздними датами), и наши подзапросы будут это учитывать, каждый раз заново рассчитывая необходимые нам значения — согласитесь, довольно удобный функционал.

**На заметку:**

Если же в одном запросе используется несколько разных «переменных» из подзапросов или к одному и тому же подзапросу нужно обращаться несколько раз, тогда имеет смысл вынести эти подзапросы в начало основного запроса в виде табличных выражений в блоке `WITH`.

WITH 
subquery AS (
    SELECT MAX(column_2)
    FROM table_2
)

SELECT column_1
FROM table_1
WHERE column_1 = (SELECT * FROM subquery) 

Обратите внимание на запись со «звёздочкой». Дело в том, что обратиться к этим «переменным» просто по имени табличного выражения не получится — придётся отдельным подзапросом из табличного выражения выбрать рассчитанное значение. Самый простой вариант — написать подзапрос с `SELECT *` из табличного выражения.

Если «переменных» несколько, то запрос может выглядеть так:

WITH
subquery_1 AS (
    SELECT MAX(column_1)
    FROM table_1
),
subquery_2 AS (
    SELECT MAX(column_2)
    FROM table_2
)


SELECT column
FROM table
WHERE column >= (SELECT * FROM subquery_1) - 100
    AND column <= (SELECT * FROM subquery_2)

Такая запись может показаться громоздкой, но в некоторых случаях, когда одни и те же подзапросы необходимо использовать несколько раз по ходу основного запроса, она может существенно упростить ваш код.

## **Задание:**

Выведите информацию о товарах в таблице `products`, цена на которые превышает среднюю цену всех товаров на 20 рублей и более. Результат отсортируйте по убыванию id товара.

Поля в результирующей таблице: `product_id`, `name`, `price`

SELECT *
FROM   products
WHERE  price >= (SELECT avg(price)
                 FROM   products) + 20
ORDER BY product_id desc

SELECT product_id,
       name,
       price
FROM   products
WHERE  price >= (SELECT avg(price)
                 FROM   products) + 20
ORDER BY product_id desc

