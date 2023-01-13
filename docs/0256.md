# CSS 选择器–类、名称、子选择器列表的备忘单

> 原文：<https://www.freecodecamp.org/news/css-selectors-cheat-sheet-for-beginners/>

CSS 选择器定位并选择你想要设置样式的 HTML 元素。

具体来说，CSS 选择器允许您一次选择多个元素。

当您想要将相同的样式应用于多个 HTML 元素时，它们会很有帮助，因为您不会重复为不同的元素编写相同的代码行。

当你想做一个改变时，CSS 选择器也很有帮助——你只需要在一个地方做改变，这样可以节省你很多时间。

当你第一次开始编写 CSS 代码时，CSS 选择器是你需要学习的第一件事。

有许多选择器可供选择，还有几种不同的使用方法——比您可能意识到的更多。

也就是说，没有必要担心——你不必记住所有的东西。

这个备忘单涵盖了开始时你需要知道的最常用的选择器。将它加入书签，这样当你在做下一个网页设计项目时，当你需要快速提醒的时候，你就可以回到它上面。

以下是我们将要介绍的内容:

1.  [简单的 CSS 选择器](#basic)
    1.  [通用选择器](#universal)
    2.  [类型选择器](#type)
    3.  [类别选择器](#class)
    4.  [ID 选择器](#id)
2.  [属性选择器](#attribute)
    1.  [`[attribute]`选择器](#attr-1)
    2.  [`[attribute="value"]`选择器](#attr-2)
    3.  [`[attribute^="value"]`选择器](#attr-3)
    4.  [`[attribute$="value"]`选择器](#attr-4)
    5.  [`[attribute*="value"]`选择器](#attr-5)
    6.  [`[attribute~="value"]`选择器](#attr-6)
3.  [分组 CSS 选择器](#grouping)
4.  [CSS 组合子](#combinators)
    1.  [后代组合子](#descendant)
    2.  [直接子组合子](#child)
    3.  [一般兄弟组合子](#general-sibling)
    4.  [相邻兄弟组合子](#adjacent-sibling)
5.  [伪类选择器](#pseudo-class)
    1.  [链接的伪类选择器](#links)
    2.  [输入的伪类选择器](#inputs)
    3.  [位置](#position)的伪类选择器
6.  [伪元素选择器](#elements)
    1.  [`::before`伪元素](#element-1)
    2.  [`::after`伪元素](#element-2)
    3.  [`::first-letter`伪元素](#element-3)
    4.  [`::first-line`伪元素](#element-4)

## 简单的 CSS 选择器

选择器允许您定位和选择文档的特定部分，以便进行样式设置。

简单的选择器直接选择一个或多个元素:

*   使用通用选择器`*`。
*   基于元素的名称/类型。
*   基于元素的类值。
*   基于元素的 ID 值。

通过学习最简单的选择器是如何工作的，您可以理解如何使用更复杂的选择器。

如果你有编写 CSS 代码的经验，那么简单的选择器通常是你最常用的，也是你最熟悉的。

### CSS 通用选择器

通用选择器，也称为通配符，选择文档中的所有元素。

要使用通用选择器，请使用星号字符`*`。

```
* {
    property: value;
} 
```

在添加任何其他样式之前，您可以使用通用选择器在文件顶部将浏览器的默认填充和边距重置为零:

```
* {
    padding: 0;
    margin:  0;
} 
```

### CSS 类型选择器

CSS 类型选择器选择指定类型的所有 HTML 元素。

要使用它，请提及 HTML 元素的名称。

例如，如果您想对 HTML 文档中的每个段落应用一种样式，您可以指定`p`元素:

```
p {
    property: value;
} 
```

上面的代码匹配并选择文档中的所有 `p`元素，并设置它们的样式。

### CSS 类选择器

类选择器根据给定类的值匹配和选择 HTML 元素。具体来说，它选择文档中具有特定类名的每个元素。

使用类选择器，您可以一次选择多个元素，并以相同的方式设置它们的样式，而不必分别为每个元素复制和粘贴相同的样式。

类是可重用的，这使它们成为实践干式开发的好选择。DRY 是一个编程原则，是‘不要重复自己’的简称。顾名思义，目的是尽可能避免编写重复的代码。

要使用类选择器选择元素，请使用点字符`.`，后跟类的名称。

```
.my_class {
    property: value;
} 
```

在上面的代码中，选择了类为`my_class`的元素，并相应地设置了样式。

### CSS ID 选择器

ID 选择器根据 ID 属性的值选择 HTML 元素。

请记住，一个元素的 ID 在一个文档中应该是惟一的，这意味着只有一个 HTML 元素具有给定的 ID 值。除了那个元素之外，不能在另一个元素上使用相同的 ID 值。

要选择具有特定 ID 的元素，请使用散列字符`#`，后跟 ID 值的名称:

```
#my_id {
    property: value;
} 
```

上面的代码将只匹配 ID 值为`my_id`的唯一元素。

值得一提的是，最好尝试限制这个选择器的使用，并选择使用类选择器。使用 ID 选择器应用样式并不理想，因为样式不可重用。

## CSS 属性选择器

许多 HTML 元素都有属性。

HTML 属性:

*   提供关于 HTML 元素的附加信息。
*   总是在开始(或开始)标记中指定。
*   通常以名称/值对的形式出现，比如`name="value"`。
*   名称/值对中的`value`应该用引号括起来。

您可能遇到过的最流行的 HTML 属性之一是`href`属性，它被添加到开始的`<a>`标签中，并指定`<a>`标签链接到的 URL:

```
<a href="https://www.freecodecamp.org/">The best place to learn to code for free!</a> 
```

`href`、`https://www.freecodecamp.org/`的值是用户点击链接文本`The best place to learn to code for free!`时将被带到的 URL。

属性选择器根据属性或特定属性值的存在来匹配和选择 HTML 元素。

有不同类型的属性选择器。

下面是一些最常见的属性选择器。

### `[attribute]`选择器

要使用属性选择器，请使用一对方括号`[]`来选择您想要的属性。

属性选择器的一般语法如下:

```
element[attribute] 
```

如果给定的属性存在，这个选择器选择一个元素。

在下面的例子中，选择了具有属性`attr`的元素，而不考虑`attr`的具体值:

```
a[attr] {
    property: value;
} 
```

在上面的例子中，选择了属性名为`attr`的`a`元素，而不考虑`attr`的值。

也就是说，你可以更具体地设计自己的风格。

### `[attribute="value"]`选择器

您可以使用以下语法指定属性的值:

```
element[attribute="value"] 
```

因此，如果您想要用一个具有`1`的**精确**值的`attr`属性来样式化`a`元素，您应该执行以下操作:

```
a[attr="1"] {
    property: value;
} 
```

上面的代码匹配`a`元素，其中`attr`属性名的确切值为`1`。

### `[attribute^="value"]`选择器

您还可以使用以下语法指定属性**的值以特定字符开始**:

```
element[attribute^="value"] 
```

例如，如果您想要选择任何具有以`www`开头的`attr`属性的`a`元素并设置其样式，您可以执行以下操作:

```
a[attr^="www"] {
    property: value;
} 
```

上面的代码选择任何`a`元素，其中`attr`属性名称的值以`www`开头。

### `[attribute$="value"]`选择器

您还可以使用以下语法指定属性**的值以特定字符结束**:

```
element[attribute$="value"] 
```

例如，如果您想要选择具有以`.com`结尾的`attr`属性名称的`a`元素，您可以执行以下操作:

```
a[attr$=".com"] {
    property: value;
} 
```

### `[attribute*="value"]`选择器

您还可以指定属性值包含特定的子串-此选择器称为“属性包含子串选择器”,其语法如下:

```
element[attribute*="value"] 
```

在这种情况下，字符串`value`需要出现在属性值中，后跟任意数量的其他字符- `value`不需要是一个完整的单词。

例如，如果您想要选择具有包含字符串`free`的值的`attr`属性的`a`元素，您应该执行以下操作:

```
a[attr*="free"] {
    property:value;
} 
```

当字符串`free`出现在属性值中时，上面的代码选择具有`attr`属性名称的`a`元素——即使是子字符串(子字符串是另一个单词中的一个单词)。

只要属性的值包含`free`，那么 HTML 元素就会被选中——例如，这可以匹配一个值为`free`、`freeCodeCamp`或`freediving`的`attr`属性。

### `[attribute~="value"]`选择器

还可以使用以下语法指定选择器匹配包含整个单词的属性值:

```
element[attribute~= "value"] 
```

在这种情况下，字符串`value`需要是一个完整的单词。

例如，如果您想选择属性名为`attr`的`a`元素，其值包含单词`free`，您可以执行以下操作:

```
a[attr~= "free"] {
    property: value;
} 
```

上面的代码将匹配一个值为`free`的`attr`属性，该属性包含不同种类的空格。

代码不会像您在前面的例子中看到的那样选择具有`freeCodeCamp`或`freediving`的`attr`值的元素，因为`free`本身需要是一个完整的单词——而不是一个子串。

## 分组 CSS 选择器

使用分组选择器，您可以一次为多个元素设定目标和样式。

要使用分组选择器，请使用逗号`,`来分组和分隔您想要选择的不同元素。

例如，下面是如何一次定位多个元素，如`div` s、`p` s 和`span` s，并对每个元素应用相同的样式:

```
div, p, span {
    property: value;
} 
```

上面的代码匹配页面上的所有`div`、`p`和`span`元素，这三个元素将共享相同的样式。

## CSS 组合子

组合器允许您根据元素之间的关系以及它们在文档中的位置来组合两个元素。

本质上，您可以将两个简单的选择器组合在一起，解释这些 CSS 选择器之间的关系。组合子是一种选择器，它指定并描述了两个选择器之间的关系。

有四种类型的组合子:

*   后代组合子。
*   直接子组合子。
*   一般兄弟组合子。
*   相邻兄弟组合子

### 后代组合子

顾名思义，后代组合子只选择指定元素的后代。

本质上，你先提父元素，留个空格，再提第一个元素的后代，也就是父元素的子元素。子元素是父元素内部的元素。

下面我们来举个例子:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="main.css">
  <title>Document</title>
</head>
<body>
  <div>
    <h2>I am level 2 heading</h2>
    <p>I am a paragraph inside a div</p>
    <span>I am a span</span>
    <p>I am a paragraph inside a div</p>
  </div>
  <p>I am a paragraph outside a div</p>
</body>
</html> 
```

在上面的例子中，`div`是父元素，而`h2`、`span`和两个`p`是子元素，因为它们在`div`中。`div`外还有一段。

如果您只想设计`div`中的段落，您可以这样做:

```
div p {
    property: style;
} 
```

在上面的例子中，只有包含文本`I am a paragraph inside a div`的`div`中的两个段落得到样式化。另外两个子元素和带有文本`I am a paragraph outside a div`的`div`之外的段落没有样式化。

### 直接子组合子

直接子组合子，也称为直接后代，只选择父代的直接子代。

要使用直接子组合符，请指定父元素，然后添加`>`字符，后跟您想要选择的父元素的直接子元素。

下面我们来举个例子:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="main.css">
  <title>Document</title>
</head>
<body>
  <div>
    <a href="#">I am a link</a>
    <a href="#">I am a link</a>
    <p><a href="#">I am a link inside a paragraph</a></p>
  </div>
</body>
</html> 
```

有一个`div`是父元素。

在父元素中，有两个`a`元素是`div`的直接子元素。

在一个`p`元素中还有另一个`a`元素。`p`元素是`div`的子元素，但是段落中的`a`元素是*而不是*的直接子元素`div`。

因此，为了只访问作为`div`的直接子元素的`a`元素，您应该执行以下操作:

```
div > a  {
    property: value;
} 
```

上面的代码匹配直接嵌套在`div`元素中的`a`元素，并且是直接子元素。

### 通用兄弟组合符

一般兄弟组合子选择兄弟。

您可以指定第一个元素和第一个元素之后的第二个元素。第二个元素不需要紧跟在第一个元素之后。

要使用一般的兄弟组合符，请指定第一个元素，然后使用`~`字符，后跟需要跟在第一个元素后面的第二个元素。

下面我们来举个例子:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="main.css">
  <title>Document</title>
</head>
<body>
  <div>
    <p>I am paragraph inside a div</p>
  </div>
  <p>I am a paragraph outside a div</p>
  <h3>I am a level three heading</h3>
  <p>I am a paragraph outside a div</p>
</body>
</html> 
```

在`div`中嵌套了一个`p`元素。那个特定的`p`元素是`div`的子元素。

还有两个包含文本`I am a paragraph outside a div`和一个`h3`元素的段落。所有这三个元素都是`div`元素的兄弟。

因此，要选择与`div`元素同级的`p`元素，您需要执行以下操作:

```
div ~ p {
    property: value;
} 
```

上面的代码设计了跟在`div`后面的两个`p`元素的样式。

它甚至样式化了不直接跟在`div`元素后面的`p`元素，比如跟在`h3`元素后面的`p`。它这样做是因为它仍然在`div`之后。

### 相邻兄弟组合符

相邻兄弟组合子比一般兄弟组合子更具体。

这个选择器只匹配直接的*兄弟。直接兄弟是紧跟在第一个元素之后的兄弟。*

要使用相邻的兄弟组合符，请指定第一个元素，然后添加`+`字符，后跟您想要选择的元素，该元素紧跟在第一个元素之后。

让我们重温一下上一节的例子:

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="main.css">
  <title>Document</title>
</head>
<body>
  <div>
    <p>I am paragraph inside a div</p>
  </div>
  <p>I am a paragraph outside a div</p>
  <h3>I am a level three heading</h3>
  <p>I am a paragraph outside a div</p>
</body>
</html> 
```

虽然跟在`h3`元素后面的`p`元素是`div`元素的同级，但它不是像在`h3`之前的`p`元素那样的*直接*同级。

因此，为了只定位紧接在`div`之后的`p`元素，您应该执行以下操作:

```
div + p {
    property: value;
} 
```

## 伪类选择器

伪类选择器选择处于特定状态的元素。

元素可能处于的状态的一些示例如下:

*   鼠标指针悬停在该元素上。
*   该元素是其类型中的第一个。
*   该链接以前曾被该特定浏览器访问过。
*   该链接之前没有从该特定浏览器被访问过。
*   复选框/单选按钮已被选中。

伪类选择器以冒号`:`开头，后面跟一个反映指定元素状态的关键字。

一般语法如下所示:

```
element:pseudo-class-name {
    property: value;
} 
```

### 链接的伪类选择器

有基于链路状态信息的选择器。

当元素以前没有被访问过时,`:link`选择器应用样式:

```
a:link {
    property: value;
} 
```

当元素在当前浏览器中已被访问过时，应用`:visited`选择器:

```
a:visited {
    property: value;
} 
```

当鼠标指针悬停在一个元素上时，应用`:hover`选择器:

```
a:hover {
    property: value;
} 
```

当用户在一个元素上定位时，应用`:focus`选择器:

```
a:focus {
    property:value;
} 
```

点击并按住鼠标按钮后选择元素时，应用`:active`选择器:

```
a:active {
    property: value;
} 
```

### 输入的伪类选择器

您之前看到的用于链接的`:focus`选择器也用于输入:

```
input:focus {
    property: value;
} 
```

`:required`选择器选择所需的输入。必需的输入具有`required`属性。

```
input:required {
    property: value;
} 
```

`:checked`选择器选择已被选中的复选框或单选按钮:

```
input:checked {
    property:value;
} 
```

`:disabled`选择器选择禁用的输入。禁用的输入具有`disabled`属性。默认情况下，许多浏览器将禁用的输入设置为淡出的灰色:

```
input:disabled {
    property:value;
} 
```

### 位置的伪类选择器

第一个子选择器`:first-child`选择第一个元素，这将是父容器中的第一个子元素。

例如，当`a`元素是父容器中的第一个子元素时，您将如何选择它:

```
a:first-child {
    property: value;
} 
```

最后一个子选择器`:last-child`选择最后一个元素，这将是父容器中的最后一个子元素。

以下是当`a`元素是父容器中的最后一个子元素时如何选择它:

```
a:last-child {
    property: value;
} 
```

`:nth-child()`选择器根据子元素在一组同级元素中的位置来选择容器中的子元素。

它接受一个整数作为参数，并根据给定值选择一个元素。选择器的一般语法如下所示:

```
a:nth-child(n) {
    property: value;
} 
```

当您想要基于表达式选择元素时，例如选择偶数或奇数元素时,`:nth-child()`选择器非常有用:

```
a:nth-child(even) {
    property: value;
} 
```

第一个类型选择器`:first-of-type`，选择父容器中该特定类型的第一个元素。

例如，下面是如何选择一个`div`中的第一个`p`:

```
p:first-of-type {
    property: value;
} 
```

最后一个类型选择器`:last-of-type`，选择父容器中该特定类型的最后一个元素。

例如，下面是如何选择一个`div`中的最后一个`p`:

```
p:last-of-type {
    property: value;
} 
```

## 伪元素选择器

伪元素选择器用于样式化元素的特定部分——您可以使用它们来插入新内容或更改内容的特定部分的外观。

例如，您可以使用伪元素以不同的方式设计元素的第一个字母或第一行。您还可以使用伪元素在所选元素之前或之后添加新内容。

与前面有一个`:`字符的伪类相反，伪元素前面有一个`::`字符。

`::`字符后跟一个关键字，允许您对所选元素的特定部分进行样式化。

一般语法如下所示:

```
element::pseudo-element-selector {
    property:value;
} 
```

确保在使用伪元素选择器时使用`::`字符而不是`:`字符——这将有助于区分伪类和伪元素。

现在，让我们看看您将遇到的一些最常见的伪元素。

### `::before`伪元素

您可以使用`::before`伪元素在元素前插入内容:

```
p::before {
    property: value;
} 
```

### `::after`伪元素

您可以使用`::after`伪元素在元素的末尾插入内容:

```
p::after {
    property: value;
} 
```

### `::first-letter`伪元素

您还可以使用`::first-letter`伪元素来选择段落的第一个字母，这在您希望以某种方式设计第一个字母的样式时很有帮助:

```
p::first-letter {
    property: value;
} 
```

### `::first-line`伪元素

您可以使用`::first-line`伪元素来选择段落的第一行:

```
p::first-line {
    property: value;
} 
```

## 结论

希望你发现这个最广泛使用的 CSS 选择器的备忘单是有帮助的。

要了解更多关于网页设计的知识，请查看 freeCodeCamp 的[响应式网页设计认证](https://www.freecodecamp.org/learn/2022/responsive-web-design/)。在互动课程中，您将通过构建 15 个练习项目和 5 个认证项目来学习 HTML 和 CSS。

感谢您的阅读，祝您编码愉快！