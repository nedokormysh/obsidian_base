
При большом количестве наблюдений все выборочные средние будут вести себя в соответсвии с нормальным распределением. 

Если количество меньше 30, то стандартное отклонение по выборке не хороший параметров ГС, и все выборочные средние не ведут себя в соответсвии с нормальным.

Если размер выборок небольшой, то мы чаще будем получать выборочные средние, которые далеко отклоняются от среднего ГС.

Используем распределение Стьюдента. Оно отличается тем, что более высокие хвосты распределения. Чем больше степеней свободы, тем больше распределение Стьдента похоже на нормальное распределение.

Вернемся к предельной центральной теореме, мы уже узнали, что если некий признак в генеральной совокупности распределен нормально со средним μ и стандартным отклонением σ, и мы будем многократно извлекать выборки одинакового размера n, и для каждой выборки рассчитывать, как далеко выборочное среднее 𝑋ˉXˉ отклонилось от среднего в генеральной совокупности в единицах стандартной ошибки среднего:

то эта величина z будет иметь стандартное нормальное распределение со средним равным нулю и стандартным отклонением равным единице.

Обратите внимание, что для расчета стандартной ошибки мы используем именно стандартное отклонение в генеральной совокупности - 𝜎σ. Ранее мы уже обсуждали, что на практике 𝜎σ нам практически никогда не известна, и для расчета стандартной ошибки мы используем выборочное стандартное отклонение.

Так вот, строго говоря в таком случае распределение отклонения выборочного среднего и среднего в генеральной совокупности, деленного на стандартную ошибку, теперь будет описываться именно при помощи t - распределения.

𝑡=𝑋ˉ−𝜇𝑠𝑑𝑛t=n​sd​Xˉ−μ​ 

таким образом, в случае неизвестной 𝜎σ мы всегда будем иметь дело с t - распределением. На этом этапе вы должны с негодованием спросить меня, почему же мы применяли z - критерий в первом модуле курса, для проверки гипотез, используя выборочное стандартное отклонение?

Мы уже знаем, что при довольно большом объеме выборки (обычно в учебниках приводится правило, n > 30) t - распределение совсем близко подбирается к нормальному распределению:
t=n​sd​Xˉ−μ​
Поэтому, правильнее будет сказать, что мы используем t - распределение не потому что у нас маленькие выборки, а потому что мы не знаем стандартное отклонение в генеральной совокупности. Поэтому в дальнейшем мы всегда будем использовать t - распределение для проверки гипотез, если нам неизвестно стандартное отклонение в генеральной совокупности, необходимое для расчета стандартной ошибки, даже если объем выборки больше 30.