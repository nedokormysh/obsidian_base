Площадь под кривой используется в качестве метрики качества алгоритма. В зависимости от выбранного порога, можно построить график Precision-Recall

Проходя каждый раз по разным thresholds, мы пересчитываем значения precision и recall.
Если порог 0.4, то считаем: все что равно или больше этого значения будет являться 1 классом, далее мы сравниваем это значение с истинным, после чего заносим в формулу для точности и полноты и считаем показатели.
• Чем больше будет данных, тем больше график будет похож не на ломанную, а на гладкую кривую.
• Чем ближе график будет стремиться к точке (1,1), тем точнее алгоритм, соответственно это хорошая оценка принадлежности к 1 классу.
• Кривая показывает компромисс между точностью и полной для различного порога.
Площадь под кривой AUC-PR – оценка работы алгоритма.

[[Метрики качества в задачах классификации]]