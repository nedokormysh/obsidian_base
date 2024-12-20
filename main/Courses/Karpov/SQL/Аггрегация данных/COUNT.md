Также при подсчёте количества записей иногда вместо наименования колонки в качестве атрибута функции `COUNT` используют звёздочку «*»:

SELECT COUNT(*) FROM table

---

Однако важно учитывать один нюанс: запрос со звёздочкой возвращает количество вообще всех записей в таблице, а запрос с указанием столбца — количество тех записей, где в заданном столбце значения не являются `NULL`.

Таким образом, если в некоторой колонке column есть пропуски, выражения `COUNT(*)` и `COUNT(column)` вернут разные значения.

Давайте это проверим!

---

## **Задание:**

Как вы помните, в таблице `users` у некоторых пользователей не были указаны их даты рождения.

Посчитайте в одном запросе количество всех записей в таблице и количество только тех записей, для которых в колонке `birth_date` указана дата рождения.

Колонку с общим числом записей назовите `dates`, а колонку с записями без пропусков — `dates_not_null`.

Поля в результирующей таблице: `dates`, `dates_not_null`

SELECT count(*) as dates,
       count(birth_date) as dates_not_null
FROM   users

И ещё один важный момент: агрегирующие функции можно применять в сочетании с ключевым словом `DISTINCT`. В таком случае расчёты будут производиться только по уникальным значениям.

Если в случае с `MIN` и `MAX` это не имеет особого смысла, то при расчёте `AVG`, `SUM` и `COUNT` иногда это бывает полезно:

SELECT SUM(DISTINCT column) AS sum_distinct FROM table

---

При этом довольно часто `DISTINCT` используется именно в сочетании с `COUNT` — для подсчёта числа уникальных пользователей, уникальных заказов и т.д.

SELECT COUNT(DISTINCT column) AS count_distinct FROM table

## **Задача:**

Посчитайте количество всех значений в колонке `user_id` в таблице `user_actions`, а также количество уникальных значений в этой колонке (т.е. количество уникальных пользователей сервиса).

Колонку с первым полученным значением назовите `users`, а колонку со вторым — `unique_users`.

Поля в результирующей таблице: `users`, `unique_users`

SELECT count(user_id) as users,
       count(distinct user_id) as unique_users
FROM   user_actions

[[Аггрегация_данных_content]]