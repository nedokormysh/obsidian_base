MSE (Mean squared error, средняя квадратичная ошибка) – применима, если распределение целевой переменной нормальное, либо близко к нормальному.

$𝑄(𝑎, 𝑋) = \frac{1}{l}\sum\limits_{i=1}^l(y_i - a(x_i))^2$

Также данную ошибку можем использовать в качестве *функции потерь* для поиска весов:

$𝑄(𝑎, 𝑋) = \frac{1}{l}\sum\limits_{i=1}^lL(y_i - a(x_i)) = \frac{1}{l}\sum\limits_{i=1}^l(y_i - a(x_i))^2$

• Функция выпукла, ее легко минимизировать (при использовании как функции потерь)
• Сильно штрафует за большие ошибки за счет квадрата в выражении (плохо работает с выбросами)
• Даёт плохое представление о том насколько хорошо решена задача. Решение с 𝑀𝑆𝐸 = 10 может быть плохим, если 𝑦 ∈ (0,…, 10), или хорошим, если 𝑦 ∈ 1000,…, 10000 , вспоминаем про разброс (дисперсию)

[[RMSE]]

[[Линейная регрессия]]