
Отметим,  что в английском языке есть ещё один термин для многоклассовой классификации: это _multilabel_ классификация. Multiclass и multilabel классификации - это задачи разных типов.

- **Multiclass-классификация**: задача, в которой целевая переменная - это один из нескольких классов (где классов может быть больше,  чем два)
- **Multilabel-классификация**: задача, в которой для каждого объекта предсказывается несколько меток (например, для фильма одновременно можно предсказать его жанр, год выпуска, язык фильма и так далее). Или же на картинке может быть изображен не один объект, а несколько - и наша задача определить классы всех объектов.

##  One-versus-rest (OVR) подход

Этот подход сводит задачу к серии бинарных классификаторов, где 𝑖-й классификатор определяет, относится объект к классу 𝑖 или не относится (два варианта, то есть бинарный классификатор).

В этом подходе обучается столько бинарных классификаторов, сколько классов в исходной multiclass-задаче.

## One-versus-one (OVO) подход

В этом подходе каждый бинарный классификатор пытается отличить, принадлежит объект классу 𝑖 или классу 𝑗. Каждый такой классификатор обучается только на объектах 𝑖-го и 𝑗-го классов.

Нельзя в общем случае сказать, какой из подходов лучше. Однако у каждого из подходов есть особенности, связанные со скоростью обучения:

- Если мы выбираем подход one-versus-all, то при большом количестве объектов в данных обучение будет долгим, ведь нужно обучить несколько бинарных классификаторов на большом датасете. В случае one-versus-one каждый бинарный классификатор (с номером 𝑖𝑗) обучается только на части датасета, состоящей из классов 𝑖 и 𝑗.
- Если же классов в задаче много, то подход one-versus-one будет работать долго, так как он сводится к обучению бинарных классификаторов на каждой паре классов, то есть, если в задаче 𝑁 классов, то необходимо обучить 𝑁(𝑁−1)/2 бинарных классификаторов.

Отметим, что для некоторых алгоритмов (например, для логистической регрессии) существует их обобщение на многоклассовый случай, поэтому для них не требуется сводить задачу к серии бинарных задач.

**Метрики качества многоклассовой классификации (начало)**

- Метрика _accuracy_ - это доля правильных ответов модели, она без изменений в формуле может применяться для любого количества классов.

Давайте обсудим, как модифицируются метрики precision, recall и f1-score в многоклассовом случае.

**Метрики качества многоклассовой классификации: macro-average**

В случае, если классов больше, чем два, также можно построить матрицу ошибок.

Для вычисления точности и полноты в этом случае существует несколько подходов:

- Микроусреднение (micro-average)
- Макроусреднение (macro-average)
- Взвешенное усреднение (weighted-average)

Макроусреднение (macro-average)

_В этом подходе мы вычисляем значение выбранной метрики для каждой бинарной ситуации , а затем усредняем полученные числа._

Взвешенное усреднение (weighted-average)

В этом подходе мы усредняем посчитанные для каждого класса метрики с весами, пропорциональными количеству объектов класса.

Микроусреднение (micro-average)

В этом подходе мы вычисляем значения TP, TN, FP, FN по всей матрице ошибок сразу, исходя из их определения. Затем по полученным числам вычисляем выбранные метрики.


