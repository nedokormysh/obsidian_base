
В этом задании ещё немного поработаем с текстовыми данными и рассмотрим функцию `CONCAT`, с помощью которой можно соединять в одну строку значения из нескольких столбцов. 

Функция `CONCAT` принимает на вход несколько аргументов и возвращает результат их последовательного сложения друг с другом. Хорошая аналогия — составление предложений из разных карточек со словами:

SELECT CONCAT('SQL', ' ', 'Simulator ', 2022) Результат: SQL Simulator 2022

---

При этом аргументы не обязательно должны быть выражены текстовыми значениями — главное, они должны быть конвертируемыми в текст. В примере выше число 2022 можно конвертировать в текст '2022', поэтому запрос работает без ошибок.

## **Задание:**

Для первых 200 записей из таблицы `orders` выведите информацию в следующем виде (обратите внимание на пробелы):

_Заказ № [id заказа] создан [дата]_

Полученную колонку назовите `order_info`.


SELECT concat('Заказ № ', order_id, ' создан ', date(creation_time)) as order_info
FROM   orders
LIMIT 200

[[Cast]]
[[Базовые_запросы_content]]