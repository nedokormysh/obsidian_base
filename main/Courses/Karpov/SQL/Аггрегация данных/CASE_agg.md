Аргументом агрегирующей функции может быть и более сложная расчётная колонка — например, полученная в результате работы конструкции `CASE`.

В таком случае сама конструкция `CASE` помещается внутрь скобок агрегирующей функции:

AVG(
    CASE  
    WHEN logical_expression_1 THEN expression_1
    WHEN logical_expression_2 THEN expression_2
    ELSE expression_else
    END
)

---

Так, если бы в нашей таблице все товары были разбиты по категориям и мы захотели бы посчитать среднюю цену товаров с учётом повышающих или понижающих коэффициентов для каждой категории, то мы могли бы сделать это, например, следующим образом:

SELECT AVG(
    CASE 
    WHEN category='мясо' THEN price*0.95
    WHEN category='рыба' THEN price*0.9
    WHEN category='напитки' THEN price*1.05
    ELSE price
    END
    ) AS avg_price
FROM products


**Задание:**

Посчитайте стоимость заказа, в котором будут три пачки сухариков, две пачки чипсов и один энергетический напиток. Колонку с рассчитанной стоимостью заказа назовите `order_price`.

Для расчётов используйте таблицу `products`.

Поле в результирующей таблице: `order_price`


SELECT sum(case when name = 'сухарики' then price * 3
                when name = 'чипсы' then price * 2
                when name = 'энергетический напиток' then price
                else 0 end) as order_price
FROM   products


[[Аггрегация_данных_content]]