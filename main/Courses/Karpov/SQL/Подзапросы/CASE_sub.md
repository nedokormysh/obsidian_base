Подзапросы также могут использоваться в операторе `CASE` при формировании сложных условных конструкций. Это бывает полезно, когда необходимо сравнить значения в некотором столбце с одним значением, рассчитанным по той же или другой таблице. Или, например, в случаях, когда для каждой записи в таблице нужно проверить вхождение значения в колонке в определённое множество значений из другой таблицы.

Допустим, у нас есть две таблицы, в которых хранятся почты старых и новых клиентов, и нам необходимо «подтянуть» эту информацию в таблицу c заказами. Сделать это можно так:

SELECT client_id, email, order_id,
    CASE 
    WHEN email IN (SELECT email FROM clients_new)
    THEN 'new'
    WHEN email IN (SELECT email FROM clients_old)
    THEN 'old'
    ELSE 'unknown'
    END AS client_type
FROM orders

## **Задание:**

Назначьте скидку 15% на товары, цена которых превышает среднюю цену на все товары на 50 и более рублей, а также скидку 10% на товары, цена которых ниже средней на 50 и более рублей. Цену остальных товаров внутри диапазона (среднее - 50; среднее + 50) оставьте без изменений. При расчёте средней цены, округлите её до двух знаков после запятой.

Выведите информацию о всех товарах с указанием старой и новой цены. Колонку с новой ценой назовите `new_price`.

Результат отсортируйте сначала по убыванию прежней цены в колонке `price`, затем по возрастанию id товара.

Поля в результирующей таблице: `product_id`, `name`, `price`, `new_price`

SELECT product_id, name, price,
    CASE
        WHEN price >= (SELECT AVG(price) FROM products) + 50 THEN price - price * 0.15
        WHEN price <= (SELECT AVG(price) FROM products) - 50 THEN price - price * 0.1
        ELSE price
    END AS new_price
FROM products
ORDER BY price DESC, product_id


WITH avg_price as (SELECT round(avg(price), 2) as price
                   FROM   products)
SELECT product_id, name, price,
    CASE
        WHEN price >= (SELECT * FROM avg_price) + 50 THEN price - price * 0.15
        WHEN price <= (SELECT * FROM avg_price) - 50 THEN price - price * 0.1
        ELSE price
    END AS new_price
FROM products
ORDER BY price DESC, product_id