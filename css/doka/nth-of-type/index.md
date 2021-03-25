---
title: ":first-of-type / :nth-of-type / :last-of-type / :nth-last-of-type / :only-of-type"
author: realetive
contributors: skorobaeus
summary:
  - псевдокласс
---

## Кратко

Поиск селекторов в группе на одном уровне вложенности, основанный на их порядке: `:first-of-type` — первый, `:nth-of-type(<n-число>)` — каждый n-элемент, `:last-of-type` — последний и `:nth-last-of-type(<n-число>)` — каждый n-элемент с отсчётом «с конца».

## Пример

Допустим, мы имеем такую HTML-разметку:

```html
<ol class="list">
  <li>Default означает «по умолчанию», цвет этого пункта без изменений.</li>
  <li class="list__item">Амбары красят в красный цвет, потому что красная краска...</li>
  <li>Говорят, в СССР красили подъезды в зелёный цвет, потому что осталось...</li>
  <li class="list__item">В Катаре красят асфальт в синий цвет, чтобы он не перегревался.</li>
  <li class="list__item">Газовые трубы красят в жёлтый цвет по ГОСТ 14202-69.</li>
  <li class="list__item">Розы красят в синий цвет при помощи пищевых красителей.</li>
  <li class="list__item">Чёрный на чёрном не виден.</li>
</ol>
```

И, допустим, нам нужно задать цвет текста каждого из пунктов, основываясь на их порядке:

```css
/* следующий пункт не имеет класса, всё, что мы о нём «знаем» — что он
третий <li> по порядку */
li:nth-of-type(3) {
  color: #49A16C;
}

/* 2, 4, 6 пункт с классом .list__item имеет синий цвет, т. е. каждый чётный */
.list__item:nth-of-type(2n) { /* нечётные элементы записываются как 2n+1 */
  color: #1A5AD7;
}

/* первый .list__item в списке */
.list__item:first-of-type, /* но этот селектор не сработает, потому что
                              класс .list__item принадлежит к элементу <li>,
                              а у li:first-of-type не указан класс */
/* но это так же и второй <li>, если использовать только тег как селектор
независимо от того, будет у li указан класс или нет, второй <li> будет
иметь красный цвет (если нет свойства у селектора с бо́льшей специфичностью */
li:nth-of-type(2) {
  color: #ED6742 !important; /* никогда так не делайте, это лишь для примера,
                            чтобы повысить специфичность тега и переопределить
                            .list__item:nth-of-type(2n) */
}

/* 5-й <li> (или 3 .list__item) — жёлтый */
li:nth-of-type(5),
.list__item:nth-of-type(3) {
  color: #FFD829;
}

/* последний пункт должен быть белым (укажем это и для тега, и для класса) */
li:nth-last-of-type(1),
.list__item:last-of-type {
  color: #18191C;
}
```

<iframe title="Псевдоклассы -of-type" src="demos/every.html"></iframe>

Если нам нужно выбрать единственный элемент своего родителя, используется псевдокласс `:only-of-type` (что эквивалентно комбинации `:first-of-type:last-of-type`):

<iframe title="Выбор единственного элемента родителя" src="demos/only.html"></iframe>

## Как пишется

💡 Как и все псевдокласс, слева от `:`-разделителя можно указать дополнительный селектор, к которому нужно применить правило. Если его не указать, правило будет применено для каждого подходящего элемента.

Для порядковых псевдокласс (`:nth-of-type(<n-число>)` и `:nth-last-of-type(<n-число>)`) для обозначения кратности (каждый n-элемент) используется запись в формате `<целое число>` + буква `n`, например, запись:

```css
p:nth-of-type(4n) {
  /* … */
}
```

Будет выбран каждый 4-й элемент (если таковой будет в общем родителе на одном уровне вложенности).

Для обозначения чётных или нечётных элементов можно использовать числовое выражение

```css
p:nth-of-type(2n) {
  /* выберет все чётные элементы */
}

p:nth-of-type(2n + 1) {
  /* выберет все нечётные элементы */
}
```

или именованную форму:

```css
p:nth-of-type(even) {
  /* выберет все чётные элементы — то же самое, что p:nth-of-type(2n) */
}

p:nth-of-type(odd) {
  /* выберет все нечётные элементы — то же самое, что p:nth-of-type(2n+1) */
}
```

## Аргументы

- для числовых псевдоклассов (`:nth-of-type(n)` и `:nth-last-of-type(n)`) обязательно наличие n-числа в аргументах.

## Подсказки

💡 Выбор первого (`:first-of-type`) или последнего (`:last-of-type`) элемента «грамматически» эквивалентен числовому формату — `:nth-of-type(1)` для первого элемента и `:nth-last-of-type(1)` для последнего соответственно, но не требует указания числа-аргумента.

💡 Если нужно выбрать такой элемент, который внутри своего родителя только в единственном экземпляре, используйте `:only-of-type`.