---
title: "Селектор по идентификатору"
author:
  - solarrust
summary:
  - initial
  - inherit
  - unset
  - revert
---

## Кратко

Селектор по идентификатору находит на странице элемент, которому задан атрибут `id` с конкретным значением.

## Пример

```html
<p id="first" class="paragraph">Какой-то текст</p>
<div id="second">Красивый блок</div>
<form id="last" action="" method="get"></form>
```

```css
#first {
  color: red;
}

#last {
  border: 2px solid green;
}
```

В этом примере в HTML есть три элемента с разными идентификаторами. В CSS для элемента с идентификатором `first` прописываем, что цвет текста должен быть красным, а для элемента с идентификатором `last` задаём рамку зелёного цвета шириной в 2 пикселя.

## Как пишется

```css
#id {
  color: black;
}
```

## Как это понять

Если нужно применить стили только к одному конкретному элементу, то ему задают идентификатор при помощи атрибута [`id`](/html/doka/global-attrs) и используют его в качестве селектора в CSS.

## Подсказки

💡 Идентификатор чувствителен к регистру. Для браузера `id="myTag"` и `id="MyTag"` будут двумя разными идентификаторами.

💡 В имени идентификатора нельзя использовать пробел.

💡 Селектор по идентификатору имеет очень высокую специфичность. У одного блока зададим и класс и идентификатор:

```html
<section class="class" id="id">
  <p>Some text</p>
</section>
```

В стилях напишем два блока правил. В первом случае используем в качестве селектора идентификатор, а во втором случае используем в качестве селектора класс.

```css
#id {
  color: red;
}

.class {
  color: green;
}
```

В итоге цвет текста будет красным, потому что селектор по идентификатору более специфичный, чем селектор по классу.

Подробнее про специфичность читайте в статье «[Специфичность](/css/articles/specificity)» (в работе).