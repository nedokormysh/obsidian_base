
# GROUP BY

Для группировки данных по какому-либо полю и дальнейшей агрегации значений (например, группировка по имени и агрегация поля цена), применяют функцию GROUP BY

SELECT CustomerID, FirstName, sum(Price) as sum_price
FROM Orders
GROUP BY CustomerID, FirstName