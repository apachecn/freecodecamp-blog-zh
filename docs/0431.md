# HTML 新行–如何使用 BR 标签添加换行符

> 原文：<https://www.freecodecamp.org/news/html-new-line-how-to-add-a-line-break-with-the-br-tag/>

在本文中，我将解释什么是换行符，并向您展示如何在 HTML 中创建它们。

## 什么是换行符？

顾名思义，换行符就是一行中的一个分隔符😅。HTML 中的换行符是一行水平结束的地方，下一行从新的一行开始。

在 HTML 中，当你像这样写一个字符串时:

```
<p>
    Hello, I am
    trying to create a
    new line
</p> 
```

空白字符(“Hello”前的制表符、“am”和“trying”、“a”和“new”之间的空格)将被忽略。屏幕上的结果将如下所示:

```
Hello, I am trying to create a new line 
```

解决这个问题的一个方法(虽然不是很有效)是创建三个`<p>`标签，如下所示:

```
<p>Hello, I am</p>
<p>trying to create a</p>
<p>new line</p> 
```

这将导致以下结果:

```
Hello, I am
trying to create a
new line 
```

因为`p`标签创建了`block`元素，它们占据了整个水平空间，下一个元素进入下一行——正如你从上面的结果中看到的。

这个解决方案是无效的，因为你已经创建了三个段落。在屏幕阅读器要解释这一点的情况下，它会将它作为三个段落而不是一个句子来阅读。这可能会影响网站的可访问性。

那么，如何为内联元素添加换行符呢？

## 如何在 HTML 中添加换行符

HTML 有很多用途的标签，包括创建换行符。您可以在 HTML 中使用`br`标签来添加换行符。它可以位于行内元素之间，将元素分成多个部分。

下面是一个带有`br`标签的段落示例:

```
<p>
    Hello, I am
    <br />
    trying to create a
    <br />
    new line
</p> 
```

标签是一个没有结束标签的无效元素。相反，它是一个自结束标记。

上述代码会导致以下结果:

```
Hello, I am
trying to create a
new line 
```

您可以将该标签用于其他形式的内联元素，如链接。例如，看看这段代码:

```
<div>
    <a href="https://google.com">Google</a>
    <a href="https://twitter.com">Twitter</a>
</div> 
```

锚标记`a`是行内元素，所以第二个链接不是显示在下一行，而是显示在同一行，如下所示:

![image-50](img/04430a86dee32f205a3982925f81346d.png)

您可以在链接之间使用`br`标记来断开第一条链接线:

```
<div>
    <a href="https://google.com">Google</a>
    <br />
    <a href="https://twitter.com">Twitter</a>
</div> 
```

![image-51](img/1e1fdaa0df78e3b6f8dce64af0602499.png)

## 结论

HTML 中的`br`标签在新的一行开始下一个元素，类似于字符串中的回车`\n`。

您可以使用换行符标记:`br`，而不是使用 block 元素将元素放入新行。

在像句子这样的情况下，使用`br`标签作为一个可视的换行符，并不影响可访问性。屏幕阅读器将不间断地阅读句子。