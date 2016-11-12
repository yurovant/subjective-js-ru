# Основы JS за 30 минут

Туториал рассчитан на программистов / сисадминов, не знающих или плохо знающих JS.
Основной акцент сделан на особенности JS, отличающие его от остальных языков,
и неочевидные вещи. Также даются практические советы по обхождению и минимизации недостатков языка.

## Синтаксис JS похож на C, Java, PHP

```js
// Однострочный комментарий 

/*
  Многострочный
  комментарий
*/

1.99 // Число

"hello" // Строка

true // Булевское значение

[1, 2, 3] // Массив

{foo: "bar"} // Словарь (Запись)

new Date("1999-01-01") // Дата

/^Jack$/ // Регулярное выражение

let x = "He-he" // Присвоение переменной

(x + 2) * 3 // Арифметические операции, скобки

(x && y || z) // Логические операции

function add(x, y) { // Декларация функции
  return x + y
}

add(x, 2) // Вызов функции

class User { // Декларация класса
  constructor(name, age) { // Декларация метода
    super(name, age)
    this.name = name
    this.age = age
  }
}

new User("Jack", 31) // Создание экземпляра объекта
```

## Слабая динамическая типизация

С точки зрения статической типизации, в JS все выражения (expressions) имеют тип `any`.
Все функции в JS являются [вариадическими](https://en.wikipedia.org/wiki/Variadic_function).
На практике, это означает, что все вызовы функций и операции являются допустимыми (см. ниже).

В рантайме – выражениям и переменным присваивается определённый тип, меняющийся по необходимости.

JS использует автоприведение типов (коэрцию), в случае обнаружения операндов
различных типов:

```js
true * 7 // 7 
// 1 * 7
```

Операция умножения является арифметической, а не логической, поэтому `boolean` приводится к типу `number`
(`true -> 1`, `false -> 0`) и операция выполняется.

В интернете есть множество примеров и таблиц, описывающих правила коэрции ("js wtf stackoverflow").
Почти все они основаны на произвольной логике которая, в принципе, могла бы быть иной. 

Например:

```js
1 + "1" // "11"
1 - "1" // 0
```

Правила коэрции для сложения выбраны в пользу строк.
Правила коэрции для вычитания – в пользу чисел. 

С нашей точки зрения, запоминать эти правила не имеет никакого смысла.
Их умышленное использование свидетельствует о низком уровне программиста.

Если вас спрашивают подобные вещи на собеседовании – это повод задуматься *куда вы устраиваетесь*.
Зазубривание данных таблиц *на память* не сделает вас лучшим программистом. Скорее – это забивание памяти ненужным хламом. 
Незнание "правил" коэрции никоим образом не скажется на вашей продуктивности и качестве кода. Со временем, вы, так или иначе, 
запомните некоторые из них (что также не будет иметь никакого значения).

Сами создатели JS и все профессионалы считают автоприведение типов серьёзным просчётом (в дизайне языка).
В том же Python, `1 + "1"` порождает `TypeError: unsupported operand type(s)...`, что лучше чем коэрция (но хуже чем ошибка компиляции).
К сожалению, это решение уже невозможно пересмотреть. JS содержит оператор `===`, который может послужить паллиативным
решением в некоторых случаях (см. ниже). Однако, с нашей точки зрения, гораздо продуктивнее оказывается думать о типах как о статических.

В целом, механизм исключений в JS используется довольно редко (намного реже, чем в том же Python).
Иногда это оказывается удобнее, иногда – нет.

В хорошем коде, переменные не меняют свой тип, а выражения, в которых возможно использование разных типов, 
обрабатываются вручную: `1 + parseInt(x)`, `String(x) + "1"`.

Далее по тексту мы будем использовать фразу "операция определена" как для языков со статической типизацией.
Разница будет состоять в том, что во втором случае неопределённая операция будет означать ошибку этапа компиляции.
А в первом случае – получение "неведомой фигни" в рантайме. 

Хотя `true / true` синтаксически корректно, его семантика нас не интересует, т.к. она выбрана произвольно
и не должна использоваться. Подобные операции мы будем считать "неопределёнными".

Перебор или недобор аргументов функции не является синтаксической ошибкой:

```js
function add(x, y) {
  return x + y
}

add()         // синтаксически корректно
add(1)        // +
add(1, 2)     // + 
add(1, 2, 3)  // +
```

Вероятно, ни один другой распространённый язык не обладает настолько слабой проверкой
семантики кода на этапе парсинга. По этой причине, использование JS без линтера или без доп. механизма статической типизации типа [Flow](https://flowtype.org/)) требует большой внимательности и временных затрат на поиск багов. 

## Примитивные и объектные типы

Все (рантайм) типы JS делятся на две категории – Примитивные (П) и Объектные (О). 

Тип из категории П может быть проверен с помощью ключевого слова `typeof`:


```js
typeof 2          // number
typeof "hi"       // string
typeof undefined  // undefined
typeof []         // object  |
typeof {}         // object  | не видно разницы между "типами"
typeof new Date() // object  |
typeof /^jack/    // object  |    
```

Тип из категории О может быть проверен с помощью ключевого слова `instanceof`:

```js
new Date() instanceof Date // true
/^foo/ instanceof RegExp   // true

class Pepe { ... } 
new Pepe instanceof Pepe   // true
```

Класс с конструктором без аргументов можно инстанциировать без скобочек (`new Pepe` эквивалентно `new Pepe()`).

Для примитивных типов можно полагаться на автоконверсию в Boolean:

```js
Boolean("")  // false
Boolean("0") // true

Boolean(0) // false
Boolean(1) // true

if ("") { ... } // эквивалентно if (false) { ... }
if (0) { ... } // эквивалентно if (false) { ... }
```

Для объектных типов нельзя полагаться на автоконверсию в Boolean

```js
Boolean([]) // true
Boolean({}) // true

if ([]) { ... } // эквивалентно if (true) { ... }
if ({}) { ... } // эквивалентно if (true) { ... }
```

Из последнего следует, что привычные для пользователей PHP / Python проверки
должны выполняться иначе (см. ниже).

Создание пользовательских П типов в JS невозможно.

## Обёрточные объекты

Для всех примитивных типов в JS определены обёрточные классы (упрощение):

```js
"foo"             // литерал 
String("foo")     // функция (приведения к типу)
new String("foo") // инстанциирование

42                // литерал 
String(42)        // функция (приведения к типу)
new Number(42)    // инстанциирование
...
```

Обёрточные классы используются автоматически в случаях, подобных следующему:

```js
"".length
```

При этом, литерал сначала приводится к типу `String`, а затем обратно.

Использование конструкторов обёрточных классов для П типов настоятельно не рекомендуется:

```js
typeof "foo"              // 'string' 
typeof String("foo")      // 'string' 
typeof new String("foo")  // 'object' (!)

typeof false              // 'boolean'
typeof Boolean(false)     // 'boolean'
typeof new Boolean(false) // 'object' (!)
```

Использование конструкторов для О типов является каноническим:

```js
new Date("1999-01-01")
new RegExp(/^jack$/)
```

Следует обратить особое внимание на то, что вызов конструктора как функции `Date(...)` и "корректным" образом `new Date(...)`
означает разные вещи. Например:

```
> Date("1999-01-01")
'Sat Nov 12 2016 14:50:47 GMT+0200 (EET)'
> new Date("1999-01-01")
1999-01-01T00:00:00.000Z
```

Первый вариант возвращает дату как строку (в каком-то левом формате) и, скорее всего, вам не нужен.

Повторим совет данный выше: 

* для О типов (включая пользовательские) всегда используйте конструкторы с `new` (`new Date("1999-01-01")`)
* для П типов всегда используйте литералы и одноимённые функции (`"foo"` и `String("foo")`)

Поскольку число П типов фиксировано, – выполнение данного правила не является большой проблемой.

## Числа

В JS есть только один тип числа (64-bit IEEE 754 double), называемый `number`.
Его достаточно для хранения целых чисел с точностью до 9×10¹⁵.

```js
-1
0
42
99.9
```

Для чисел определены [арифметические операции](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators):

```js
1 + 1  // 2   сложение
1 - 1  // 0   вычитание
10 * 2 // 20  умножение
5 / 2  // 2.5 деление
35 % 5 // 0   деление по модулю
```

Double ведут себя (не)предсказуемо (гугл в помощь): 

```js
0.1 + 0.2 // 0.30000000000000004
```

Порядком вычисления можно управлять с помощью скобок:

```js
(1 + 3) * 2 // 8
```

Есть три специальных значения, возникающие в результате арифметич. операций, но не являющиеся настоящими числами:

```js
Infinity  // бесконечность       (1 / 0)
-Infinity // минус бесконечность (-1 / 0)
NaN       // не число            (0 / 0)
```

Для чисел определены [операторы сравнения](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators):

```js
// Проверка на равенство
2 == 1 // false   
2 == 2 // true   

// Проверка на "больше"
1 > 2 // false
2 > 1 // true 

// Проверка на "больше или равно"
1 >= 2 // false
2 >= 1 // true

// Проверка на "меньше"
2 < 1 // false
1 < 2 // true

//  Проверка на "меньше или равно"
2 <= 1 // false  
1 <= 2 // true
```

Стоит помнить, что проверка на равенство не рекомендуется для Double в общем случае (неуправляемая точность).

Для чисел определены [битовые операции](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators):

```js
// Битовая инверсия
~0 // -1

// Сдвиги влево и вправо (деление и умножение на 2...)
1 << 2 // 4
2 << 2 // 8
8 >> 1 // 4
8 >> 2 // 2

// Битовый AND
5 & 1 // 1
5 & 3 // 1
5 & 0 // 0

// Битовый OR
5 | 1 // 5
5 | 3 // 7
5 | 0 // 5

// Битовый XOR
7 ^ 7 // 0
0 ^ 7 // 7
```

Битовые операции требуют понимания как числа представляются в двоичной системе (`5 -> ...000101`) 
и в классическом веб-программировании используются очень редко.

## Булеаны

Данный тип ничем не примечателен.

```js
true
false
```

Для булевских значений определены булевские операции и операция сравнения:

```js
// Логический AND (конъюнкция)
true && true  // true
true && false // false

// Логический OR (дизъюнкция)
true || true  // true
true || false // true

// Логический NOT (отрицание)
!true  // false
!false // true

// Сравнение
true == true  // true
true == false // false
```

`&&` приоритетнее `||`:

```js
x1 && x2 || y1 && y2 // (x1 && x2) || (y1 && y2)
```

## Строки

TODO

## Массивы

TODO

Проверка на пустой массив:

```js
// let xs = []
if (xs.length) { ... }
```

## Словари

TODO

Проверка на пустой словарь:

```js
// let xs = {}
if (Object.keys(xs).length) { ... }
```

## Даты

TODO

## Регулярные выражения

TODO


## Юниты

`null` и `undefined`

## Строгое и нестрогое равенство

TODO 

