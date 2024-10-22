## PyPlot

Главной единицей (объектом самого высокого уровня) при работе с matplotlib является рисунок (Figure). Любой рисунок в matplotlib имеет вложенную структуру. Пользовательская работа подразумевает операции с разными уровнями структуры:

**Figure(Рисунок)** -> **Axes(Область рисования)** -> **Axis(Координатная ось)**

Интерфейс pyplot позволяет пользователю сосредоточиться на выборе готовых решений и настройке базовых параметров рисунка. Это его главное достоинство, поэтому изучение matplotlib лучше всего начинать именно с интерфейса pyplot.
```
import matplotlib.pyplot as plt
```
Создать рисунок figure позволяет метод plt.figure(). После вызова любой графической команды, то есть функции, которая создаёт какой-либо графический объект, например, plt.scatter() или plt.plot(), всегда существует хотя бы одна область для рисования (по умолчанию прямоугольной формы).

Чтобы сохранить получившийся рисунок нужно воспользоваться методом plt.savefig()

```
fig = plt.figure()  # Создание объекта Figure

print(fig.axes)  # Список текущих областей рисования пуст
print(type(fig))  # тип объекта Figure
  

plt.scatter(x=1.0, y=0.0)  # scatter - метод для нанесения маркера в точке (1.0, 1.0) shift + Tab

# plt.savefig('report/example.png')  # сохранение рисунка (для презентаций)

# После нанесения графического элемента в виде маркера

# список текущих областей состоит из одной области

print('После нанесения: ', fig.axes)



plt.show()
```


## Элементы рисунка

1) Область рисования Axes
- Заголовок области рисования -> plt.title();
2) Ось абсцисс Xaxis
- Подпись оси абсцисс OX -> plt.xlabel();
3) Ось ординат Yaxis
- Подпись оси ординат OY -> plt.ylabel();
4) Легенда -> plt.legend() 
5) Цветовая шкала -> plt.colorbar()
- Подпись горизонтальной оси абсцисс OY -> cbar.ax.set_xlabel();
- Подпись вертикальной оси ординат OY -> cbar.ax.set_ylabel();
6) Деления на оси абсцисс OX -> plt.xticks()
7) Деления на оси ординат OY -> plt.yticks()

## Цвета

- Красный - 'red', 'r', (1.0, 0.0, 0.0)
- Оранженвый - 'orange'
- Жёлтый - 'yellow', 'y', (0.75, 0.75, 0)
- Зелёный - 'green', 'g', (0.0, 0.5, 0.0)
- Голубой - 'cyan', 'c', (0.0, 0.75, 0.75)
- Синий - 'blue', 'b', (0.0, 0.0, 1.0)
- Фиолетовый - 'magenta', 'm', (0.75, 0, 0.75)
- Чёрный - 'black', 'k', (0.0, 0.0, 0.0)
- Белый -'white', 'w', (1.0, 1.0, 1.0)

Также дополнительно можно пользоваться готовой цветовой палитрой cmap=plt.get_cmap('название') или можно поменять через plt.rcParams['image.cmap']='имя_палитры' или через plt.set_cmap('имя_палитры').

## Свойства графических элементов

Свойства объекта задаюся при помощи задания значения параметров, которые принимают на вход функции.

Наиболее часто встречаемые: 

- alpha - степень прозрачности (от 0 до 1)
- fontsize - размер шрифта
- marker - тип маркера
- color/colors/c - цвет
- linewidth/linewidths - толщина линии
- linestyle - тип линии
- s - размер маркера в методе plt.scatter(только цифры);
- rotation - поворот строки на X градусов.

```
N = 100
x = np.arange(N)
y_1 = np.sin(x / 10)
y_2 = np.cos(x / 5)
y_3 = np.sin(x / 2)  

# plot - рисует линию
# alpha - прозрачность
# linewidth - ширина линии
# linestyle - стиль линий
# marker - маркер, по точкам

plt.plot(x, y_1, color='blue', alpha=0.6, linewidth=3.0, linestyle='dashed')
plt.plot(x, y_2, color='green', marker='v')
plt.plot(x, y_3, color='red', alpha=0.2, marker='o')  

plt.title('Title')
plt.xlabel('X', fontsize=25)
plt.ylabel('Y')

plt.show()
```

## Основные графические команды

В Matplotlib уже заложены основные графические команды, если вы кратко обозначили pyplot через plt, то доступ будет "plt.название_команды()".
Наиболее распространённые команды для создания научной графики:

**1) Самые простые графические команды:**

- plt.scatter() - маркер или точечное рисование
- plt.plot() - линия
- plt.text() - нанесение текста  

**2) Диаграммы:**

- plt.bar(), plt.barh(), plt.barbs(), broken_barh() - столбчатая диаграмма
- plt.hist(), plt.hist2d(), plt.hlines - гистограмма
- plt.pie() - круговая диаграмма
- plt.boxplot() - "ящик с усами" (boxwhisker)
- plt.errorbar() - оценка погрешности, "усы" (раздел статистики)  

**3) Изображения в изолиниях:**

- plt.contour() - изолинии
- plt.contourf() - изолинии с послойной окраской  

**4) Отображения:**

- plt.pcolor(), plt.pcolormesh() - псевдоцветное изображение матрицы (2D массива)
- plt.imshow() - вставка графики (пиксели + сглаживание)
- plt.matshow() - отображение данных в виде квадратов

  

**5) Заливка:**

- plt.fill() - заливка многоугольника
- plt.fill_between(), plt.fill_betweenx() - заливка между двумя линиями  

**6) Векторные диаграммы:**

- plt.streamplot() - линии тока
- plt.quiver() - векторное поле


### Scatter

Отлично подходит при отображении embedings для кластеризации, для взаимосвязи количественных данных

```
N = 50

  

# массив со случайными числами (float), где их кол-во = N

x = np.random.rand(N)
y = np.random.rand(N)

colors = np.random.rand(N)

# массив со значением радиусов

area = (30 * np.random.rand(N))**2  

plt.scatter(x, y, s=area, c=colors, alpha=0.8)
# s - площадь каждого круга
# c - цвета (список)
# alpha - прозрачность

plt.show()
```

### Bar

```
# Диаграммы

s = ['one', 'two', 'three ', 'four', 'five']

# x - подписи по OX

x = [9, 2, 3, 4, 5]

# y_1 - высота баров

y_1 = np.random.rand(5) * 10
y_2 = np.random.uniform(-1,20,100)
plt.bar(x=x, height=y_1)
plt.title('Bar chart');
```


### Hist
```
plt.hist(x=ex, bins=2)

# bins - детализация
plt.title('Histogram');
```
```
mu, sigma = 0, 0.8 # mean and standard deviation
s1 = np.random.normal(mu, sigma, 10000)

mu, sigma = -2, 1 # mean and standard deviation
s2 = np.random.normal(mu, sigma, 10000)

# мы можем все отобразить на одном графике

plt.hist(s1, bins=50, alpha=0.5)
plt.hist(s2, bins=50, alpha=0.5, density=False);
```

### Pie

```
fig = plt.figure(figsize=(7, 7))

  

# explode - уровень выдвижения секторов
explode = (0, 0.2, 0, 0, 0.1)
# labels - подписи для каждого из x (список)
plt.pie(x=x, labels=s, startangle=45, explode=explode, autopct='%1.1f%%')
plt.title('Pie chart', fontsize=16);
```

### Boxplot
```
x_box = np.random.normal(100, 10, 200)
y_box = np.random.normal(300, 50, 100) 

plt.boxplot([x_box, y_box], vert=1)

#plt.boxplot(y_box, vert=0)

# vert - горизонтальное положение при 0

plt.title('Boxplot');

sns.boxplot(x="day", y="total_bill", data=tips);
```


### Errorbar

### Imshow

### Contour

## Мультиблочность

Если необходимо отобразить на одном рисунке несколько графиков, например для визуального сравнения, то могут применяться такие часто популярные функции как:

  

- fig.add_subplot() - добавление одного subplot на рисунок. Удобно для отображения 2-3 диаграмм
- plt.subplot() - аналогичный предыдущему по результату метод для pyplot
- plt.subplots() - удобный метод для автоматизированного создания нескольких subplots

```
fig = plt.figure()

  

x = [1, 2, 3, 4, 5]

y_1 = np.random.random(5) * 10
y_2 = np.random.random(20) * 10

  

ax1 = fig.add_subplot(221)  # (nrows, ncols, index)
ax1.bar(x, y_1)
ax1.set_title('Область 1')  

ax2 = fig.add_subplot(212)  # (nrows, ncols, index)
ax2.hist(y_2, bins=20)
ax2.set_title('Область 2')


# Координаты областей, которые занимают subplots

print(f'Область 1 = {ax1}')
print(f'Область 2 = {ax2}')  

# Grid в каждой из областей

for ax in fig.axes:
    ax.grid(True)  

plt.show()
```

```
nrows = 2
ncols = 2
fig, axes = plt.subplots(nrows=2, ncols=2)
colors = ['black', 'red', 'blue', 'orange']
  
x = np.arange(10)

c = 0

  

for i in range(nrows):
    for j in range(ncols):
        axes[i, j].plot(x, color=colors[c])
        c += 1
```

[[Третья ступень. Теоретическая база._Content]]