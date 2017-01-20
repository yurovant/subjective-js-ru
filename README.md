# Субъективный JavaScript

Остросюжетный гайд. Ускоренное изучение JavaScript как второго (третьего...) языка.

**Under development...**

## От автора

Данная книга предназначена для самостоятельного изучения JS на базе
имеющихся знаний Computer Science и опыта программирования на других языках.
Она не предназначена для обучения программированию с нуля.

Фундаментальные концепции Булевской алгебры, Условий, Циклов и т.д.
в тексте не поясняются. Они повторяются от языка к языку и их знание
принимается как базовое допущение.

Основной акцент сделан на особенности JS, неочевидные и малоочевидные вещи,
а также продвинутые области, которым в других учебниках уделяется минимум места.
Название книги, в том числе, даёт автору право затронуть спорные (и потому
наиболее интересные) темы. Которые обычно – "политкорректно" – оставляются
на "усмотрение читателя".

Автор текста имеет опыт коммерческого программирования на трёх языках,
некоммерческого – на 15+ (не считая CSS, HTML, SQL...).

Основная идея гайда – изучение по аналогии – дополняется взглядом на
язык с разных ракурсов: парадигм, истории, конкуренции, контр-примеров и поиска истины.

## [Мотивация](./motivation.md)

## [Переводы терминов](./translations.md)

## Вступление

JS часто воспринимается как простой язык и учится как [набор трюков](https://github.com/loverajoel/jstips),
заменяющих реальное знание. – Я знаю JS! – утверждает персонаж, использующий `alert` для отладки...
Несложно встретить и противоположный взгляд, по которому JS – это полностью
сломанный язык, непроходимая PITA. Мнение подкрепляется специально отобранными "ужасающими" примерами
(не встречающимися на практике).

Оба мнения расходятся с реальностью. JavaScript (в редакции ES 2016-2017) – это сравнительно
сложный язык, обладающий огромной грамматикой. Выразительные возможности JavaScript находятся на одном уровне с Python и Ruby.

JavaScript сложно выучить на однообразных задачах, возникающих в
коммерческих проектах. В то же время, значительный процент "знаний"
составляют deprecated синтаксические единицы и паттерны. В повседневной
практике, следует ограничивать себя отобранным подмножеством языка.

Официальное название спецификации JavaScript: EcmaScript. Разница
связана с лицензиями и не представляет особого интереса.
Мы будем использовать термин JS или JavaScript в качестве названия языка
и ES5, ES6, ES2015, ES2016 для ссылок на конкретные версии.

Все версии языка, на данный момент, обладают 100% обратной совместимости.
Ни авторы ES, ни сообщество пока не смогли выработать удовлетворительного
механизма удаления фич. Небольшие исключения типа `"use strict"` можно
вынести за скобки.

Ситуацию спасает отсутствие стандартных библиотек в языке. Да – это огромный плюс!
Python сообщество показало всем убедительный контр-пример. "Batteries included",
сначала, кажется хорошей идеей. Затем, из-за увязки обновления библиотек
с выпуском новых версий языка, – превращает базовый репозиторий в
археологический музей. Стандартные пакеты Python устарели и разлагаются.
Но, вопреки всеобщей ненависти, продолжают использоваться, как "стандартные"...

JS, безусловно, страдает от противоположной крайности, однако появление
NodeJS и NPM в корне изменило ситуацию. Проблем с поиском библиотек в
самой большой экосистеме в мире возникнуть не должно. Вероятно, даже
число научных библиотек в JS уже превысило таковое в Python.

Проблема с "избытком альтернатив" является меньшим злом. Конкуренция –
двигатель прогресса, а культура написания документации в JS сообществе
находится на сравнительно высоком уровне Достаточно сказать, что на
титульных страницах документаций нередко можно встретить перечень
отличий от "конкурентов". О таком уровне большинство программистских
сообществ могут лишь мечтать. Возьмите PHP с его бесчисленными
"фреймворками", отличающимися друг от друга только названиями.
Вспомните выдохшийся Java, где одно упоминание альтернатив считалось
личным оскоблением. Или тот же Python с его культом "лидеров"...

## Как читать эту книгу

Данная книга посвящёна JavaScript и не затрагивает вопросы API
(DOM, HTTP, и т.д.). Однако, примеры кода будут содержать `console.log`,
который не является частью языка. Впрочем, и NodeJS и Браузеры
предоставляют данную функцию и, вероятно, её стандартизация – вопрос времени.

Также, при необходимости показать функцию из сторонней библиотеки,
мы будем использовать синтаксис CommonJS. Имплементация ES модулей в NodeJS
ожидается весной 2017, после чего материал будет обновлён.

Желающим запускать примеры предлагается:

* установить [NodeJS](https://nodejs.org/en/) (и редактор – если вы форматировали диск)
* копировать тексты примеров в отдельные файлы
* запускать файлы из консоли командой вида `$ node exampleX.js`

Для простейших примеров можно ограничиться Node REPL (`$ node`).

Автор книги стремится сделать ВСЕ примеры максимально простыми и
понятными с первого взгляда. Если вы испытываете сложность с пониманием
какого-либо абзаца или примера при последовательном чтении –
открывайте соответствующий [тред](https://github.com/ivan-kleshnin/subjective-js/issues).

Чтение гайда рекомендуется выполнить дважды. Первый раз – пропуская
ссылки на доп. ресурсы в пользу скорейшего прохождения.
Второй раз – переходя по ним (... по интересующим лично вас).

## Содержание

### 1. Введение

#### [1.1 Пятиминутный обзор](./content/1.1.five-min-overview.md)

#### [1.2 Сущности первого класса](./content/1.2.first-class-things.md)

#### [1.3 Синтаксические единицы](./content/1.3.syntax-units.md)

### 2. Примитивы

#### [2.1 Зарезервированные слова](./content/2.1.keywords.md)

#### [2.2 Значения](./content/2.2.values.md)

#### [2.3 Переменные](./content/2.3.variables.md)

#### [2.4 Ссылки](./content/2.5.references.md)

#### [2.5 Инструкции](./content/2.5.statements.md)

### 3. Система типов

#### [3.1 Слабая динамическая типизация](./content/3.1.weak-typing.md)

#### [3.2 Типы и Прототипы](./content/3.2.types-and-prototypes.md)

#### [3.3 Проверка типов](./content/3.3.type-checking.md)

#### [3.4 Обёрточные типы](./content/3.4.wrapper-classes.md)

#### [3.5 Коэрция типов](./content/3.5.type-coercion.md)

#### [3.6 Сравнение значений](./content/3.6.equality.md)

#### [3.7 Практические правила](./content/3.7.practical-rules.md)

### 4. Builtins

#### [4.1 Нулы](./content/4.1.nils.md)

#### [4.2 Булеаны](./content/4.2.booleans.md)

#### [4.3 Числа](./content/4.3.numbers.md)

#### [4.4 Строки](./content/4.4.strings.md)

#### [4.5 Массивы](./content/4.5.arrays.md)

#### [4.6 Записи](./content/4.6.records.md)

#### [4.7 Словари](./content/4.7.dicts.md)

#### [4.8 Множества](./content/4.8.sets.md)

#### [4.9 Дата и время](./content/4.9.date-time.md)

#### [4.10 Регулярки](./content/4.10.regexes.md)

#### [4.11 Функции](./content/4.11.functions.md)

### 5. Видимость переменных

Скоупинг, Хойстинг, Замыкания

### 5. Прототипная модель

Наследование, делегация, классы, ООП, динамический this.

### 6. Встроенные типы и классы

Встроенные П- и О-типы во втором приближении. Продвинутые темы.

## Ресурсы

Рекомендованные ресурсы:

* [Speaking JavaScript](http://speakingjs.com/es5/index.html)
* [Exploring ES6](http://exploringjs.com/es6/index.html)
* [Exploring ES2016-ES2017](https://leanpub.com/exploring-es2016-es2017/read)
* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
* [Eloquent JavaScript](http://eloquentjavascript.net/)
* [2ality blog](http://www.2ality.com)

## Поддержать проект

Данный проект является некоммерческим. Автор вложил в него более сотни часов свободного времени.
Поддержите данный проект, поставив ему &starf; и поделившись ссылкой
со знакомыми. Для подписки на обновления используйте режим **Watch**.
