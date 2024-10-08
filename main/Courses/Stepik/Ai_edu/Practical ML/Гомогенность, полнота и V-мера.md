

Пусть $𝐻=−∑_{𝑖=1}^{∣𝑈∣}𝑃(𝑖)𝑙𝑜𝑔𝑃(𝑖) - энтропия, 𝐾 - результат кластеризации, 𝐶 - результат разбиения на классы.

Тогда

- $ℎ=1−\frac{𝐻(𝐶∣𝐾)}{𝐻(𝐶)}$ - _гомогенность_

Гомогенность показывает, насколько каждый кластер однороден (то есть состоит из объектов одного класса)

- $𝑐=1−\frac{𝐻(𝐾∣𝐶)}{𝐻(𝐾)}$- _полнота_

Полнота показывает, насколько объекты одного класса относятся к одному кластеру

Метрики напоминают точность и полноту в задаче классификации. Аналогично задаче классификации, мы можем посчитать некоторую усредненную метрику на базе гомогенности и полноты - это 𝑉-мера:

𝑉=2ℎ𝑐/(ℎ+𝑐)

V-мера - способ учесть оба показателя. Она показывает, насколько разбиения 𝐾 и 𝐶 похожи.

Метрики гомогенность, полнота и 𝑉-мера не нормализованы, то есть зависят от числа объектов и числа кластеров. Но в большинстве задач эта зависимость не так ярко выражена, и ее можно не учитывать.

Правило такое:

- При большом числе кластеров и малом числе классов лучше отдавать предпочтение нормализованным метрикам 𝐴𝑅𝐼 и 𝐴𝑀𝐼
- При более 1000 объектов и не более 10 кластеров (ориентировочно) проблема не так ярко выражена, и можно использовать любые из описанных метрик

[[AMI]]
[[Силуэт (silhouette score)]]
[[Метрики качества кластеризации]]