
## 1. Знакомство с print

**Функция** - это объект, который принимает **входные данные** (аргументы) и возвращает **результат** после выполнения определенных действий. Функция выполняет определенные действия согласно ее логике.

**Аргументы** - это значения, передаваемые функции для обработки.
Аргументы записываются внутри круглых скобок. Для строк мы всегда используем только данные символы, позже в уроке про типы данных, разберем этот момент более подробно.

## 2. Разделители в print

Как мы уже сказали, на вход функция принимает **аргументы**, либо же их также еще называют **параметрами**. Предположим, мы хотим подать несколько аргументов, для этого необходимо их перечислить через запятую:

```bash
print("Hello", "world", "!")
```

Результат:

```no-highlight
Hello world !
```

По умолчанию `print()` отделяет аргументы пробелом при выводе, далее ставит перенос строки (как могли увидеть на втором примере). Но если мы хотим использовать другие **разделители**, а также другой тип **окончания** строки, то для этого предусмотрены **именованные параметры**: `sep` - используется для разделения строки, `end` - для окончания строки.

**Именованные параметры** - это параметры, которым при вызове функции присваивается значение с указанием имени параметра. Это позволяет явно задавать значения отдельных параметров функции, не меняя остальные.

Функция `print()` также имеет аргументы со **значениями по умолчанию**. **Значение по умолчанию** - это _предопределенное_ значение аргумента, которое будет использоваться, если при вызове функции этот аргумент не указывался. 

**По умолчанию** параметр `sep` - содержит значение одного пробела `' '`, а `end` - строку, которая добавляется после последнего значения (значение по умолчанию `'\n'`)

- Давайте попробуем вместо пробела указать запятую:

```bash
print("Hello", "world", "!", sep=",")
```

Результат:

```no-highlight
Hello,world,!
```

- А теперь попробуем посмотреть на примере, что делает `end`, добавим дополнительно ниже строку с кодом, но уже с другим текстом:

```lua
print("Hello", "world", "!", sep=",", end=" - ")
print("It's", "my", "program")
```

 Результат:

```no-highlight
Hello,world,! - It's my program
```

Видим, что так как указали новое значение параметра `end` наша вторая строка не была переведена на новую строку, а лишь вывелась после значения `" - "`.

**Популярные символы**, которые вы также можете использовать:

- `\n` - знак перевода на новую строку
- `\t` - знак табуляции

Помимо использования данных символов в аргументах в методе `print()` вы их также можете задействовать в самой строке. Пример:

```swift
print("Hello\nworld!")
```

Результат:

```no-highlight
Hello
world!
```