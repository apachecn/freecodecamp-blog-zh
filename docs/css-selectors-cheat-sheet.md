# CSS 选择器备忘单

> 原文：<https://www.freecodecamp.org/news/css-selectors-cheat-sheet/>

在 CSS 中，选择器是用来选择 DOM 元素的模式。

下面是一个使用选择器的例子。在下面的代码中，`a`和`h1`是选择器:

```
a {
  color: black;
}

h1 {
  font-size 24px;
}
```

## **常用选择器备忘单**

`head`选择带有`head`标签的元素

`.red`选择所有“红色”类别的元素

`#nav`选择带有“导航”Id 的元素

`div.row`选择带有`div`标签和“row”类的所有元素

`[aria-hidden="true"]`选择属性`aria-hidden`为“真”的所有元素

*   通配符选择器。选择所有 DOM 元素。请参阅下文，了解它与其他选择器的配合使用

## 我们可以用有趣的方式组合选择器

### 一些例子:

`li a` DOM 后代组合子。作为`li`标签的子标签的所有`a`标签

`div.row *`选择所有带有`div`标签和“row”类的元素的后代(或子元素)

`li > a`差分组合子。选择直接后代，而不是像后代选择器那样选择所有后代

`li + a`相邻的组合子。它选择紧接在前一个元素之前的元素。在这种情况下，只有每个`li`后的第一个`a`。

`li, a`选择所有`a`元素和所有`li`元素。

`li ~ a`兄弟组合子。选择跟在`li`元素后面的`a`元素。

## 伪选择器或伪结构类

这些对于从 DOM 中选择结构元素也很有用。

### 以下是其中的一些:

`:first-child`将第一个元素直接定位在另一个元素的内部(或子元素)

`:last-child`将另一个元素(或其子元素)内的最后一个元素作为目标

将第 n 个元素作为另一个元素(或其子元素)的目标。接受整数、`even`、`odd`或公式

`a:not(.name)`选择所有不属于`.name`类的`a`元素

允许从 CSS 而不是 HTML 向页面插入内容。虽然最终结果实际上并不在 DOM 中，但它似乎在页面上。该内容在 HTML 元素之后加载。

允许从 CSS 而不是 HTML 向页面插入内容。虽然最终结果实际上并不在 DOM 中，但它似乎在页面上。该内容在 HTML 元素之前加载。

我们可以使用伪类来定义 DOM 元素的特殊状态。但是它们本身并不指向一个元素。

### 一些例子:

`:hover`选择一个被鼠标指针悬停的元素

`:focus`通过键盘或程序选择接收焦点的元素

`:active`选择被鼠标指针点击的元素

`:link`选择所有尚未点击的链接

`:visited`选择已经点击的链接

## 关于第 n 个子选择器的更多信息

`nth-child`选择器是一个 css 伪类，采用一种模式来匹配一个或多个元素相对于它们在兄弟元素中的位置。

### 句法

```
 a:nth-child(pattern) {
    /* Css goes here */
  }
```

### **图案**

`nth-child`接受的模式可以以关键字的形式出现，或者以 An+B 的形式出现。

#### **关键词**

##### **奇数**

Odd 返回给定类型的所有奇数元素。

```
 a:nth-childe(odd) {
    /* CSS goes here */
  }
```

##### **偶数**

Even 返回给定类型的所有偶数元素。

```
 a:nth-childe(even) {
    /* CSS goes here */
  }
```

#### **安+乙**

对于 n 的每个正整数值(除 0 之外)，返回与公式 An+B 匹配的所有元素。

例如，以下内容将匹配每第三个锚元素:

```
 a:nth-childe(3n) {
    /* CSS goes here */
  }
```

## **游戏**

CSS Diner 是一款网页游戏，几乎教授了所有关于组合选择器的知识。

## **附加参考文献**

还有很多 CSS 选择器！在 [CodeTuts](http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048) 、[CSS-tricks.com](https://css-tricks.com/almanac/selectors/)或 [Mozilla 开发者网络](https://developer.mozilla.org/en/docs/Web/Guide/CSS/Getting_started/Selectors)了解它们。