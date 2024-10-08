Ошибка на новых тестовых данных раскладывается:
$𝑸(𝒂) = 𝑩𝒊𝒂𝒔 (\hat{𝒚})^ 𝟐 + 𝑽𝒂𝒓(\hat{𝒚}) + 𝝈^𝟐$

**Смещение Bias**
Сгенерируем несколько обучающих выборок и обучим модели. Смещением называется отклонение среднего ответа на основании обученных моделей $𝔼 [\hat{𝑦}]$ от ответа идеального алгоритма 𝑓.
Смещение показывает, насколько хорошо с помощью данного алгоритма можно приблизить истинную зависимость.

$𝑩𝒊𝒂𝒔 (\hat{𝒚})^ 𝟐 = (𝑓 − 𝔼 [\hat{𝑦}])^2$
где $𝔼 [𝑎(𝑥)] = 𝔼[\hat{𝑦}]$ – матожидание

**Разброс Var (дисперсия)**
Сгенерируем несколько обучающих выборок и обучим модели. Разброс ответов у данных моделей и будет являться дисперсией. Разброс характеризует чувствительность алгоритма к изменениям в обучающей выборке.

$𝑉𝑎𝑟(𝑋) = 𝔼[𝑋^2] − (𝔼[𝑋])^2$
где $𝑉𝑎𝑟(𝑋) = \frac{\sum_{i=1}^N(X_i - X)^2}{N}=𝔼[X-𝔼X]^2=𝔼[𝑋^2] − (𝔼[𝑋])^2$

**Шум**
Неустранимый шум в данных
* При высоком смещении наши предсказания хорошо концентрируются в одном месте, но так и не попадают в цель.
 * При высоком разбросе, какая-то часть ответов близка к верным, но вариация ответов довольно большая.

• Смещение (bias) – показывает, насколько хорошо с помощью данного метода обучения и семейства алгоритмов можно приблизить оптимальный алгоритм.
• Дисперсия (variance) – показывает, насколько сильно может изменяться ответ обученного алгоритма в зависимости от выборки — иными словами, она характеризует чувствительность метода обучения к изменениям в выборке.
• Маленькое смещение у сложных семейств (например, у деревьев) и большое у простых семейств (например, линейных классификаторов)
• Простые семейства имеют маленькую дисперсию, а сложные семейства — большую дисперсию.
![[Pasted image 20240826165206.png]]

• Если мы строим простые модели (например линейные), у них может быть довольно
большое смещение, но низкий разброс
• Если берем что-то более сложные модели, например деревья, то можем получить
обратную историю с низким смещением, но большим разбросом.
Поэтому подбор модели это всегда также компромисс, между двумя составляющими.

[[Регуляризация. Кросс-валидация. Поиск параметров]]