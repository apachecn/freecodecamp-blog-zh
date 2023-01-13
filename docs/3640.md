# 如何在 CSS 中设置链接样式

> 原文：<https://www.freecodecamp.org/news/how-to-style-links-in-css/>

## **造型链接**

链接可以用任何 CSS 属性进行样式化，比如`color`、`font-family`、`font-size`和`padding`。这里有一个简单的例子:

```
a {
    color: hotpink;
}
```

此外，根据链接所处的状态，链接可以有不同的样式。

链接也有 4 种状态，许多程序员对每种状态都有不同的风格。这四种状态是:

*   未访问、未点击的链接
*   `a:visited`:被访问、点击的链接
*   `a:hover`:当用户的鼠标在上面时的链接
*   `a:active`:点击时的链接

`<a href="">`属性负责创建 URL，并且可以使用许多 CSS 样式属性进行修改，尽管默认情况下它有几个:

1.  强调
2.  蓝色

您可以通过添加更改`color`和`text-decoration`属性来更改这些属性。

```
 color: black;
   text-decoration: none;
```

您还可以使用这些属性(也称为链接状态)基于交互来设置链接的样式:

*   答:链接-一个正常的，未被访问的链接
*   答:已访问——用户访问过的链接
*   答:悬停——当用户鼠标悬停在链接上时的链接
*   答:活动——点击时的链接

下面是一些使用 4 种状态的 CSS 示例:

```
a:link { color: red; }
a:visited { color: blue; }
a:hover { color: green; }
a:active { color: blue; }
```

****注意**** 当你设置链接状态的样式时，有一些*排序规则*。

*   `a:hover`必须在`a:link`和`a:visited`之后

`a:active`必须在`a:hover`之后

答:链接-一个正常的，未访问的链接答:访问过的-一个用户访问过的链接答:悬停-一个当用户鼠标经过时的链接答:激活-一个被点击时的链接

```
/* unvisited link */
a:link {
    color: red;
}

/* visited link */
a:visited {
    color: green;
}

/* mouse over link */
a:hover {
    color: hotpink;
}

/* selected link */
a:active {
    color: blue;
} 
```

## 关于 CSS 样式的更多信息:

*   [如何在 CSS 中直接设计 HTML 标签的样式](https://www.freecodecamp.org/news/inline-css-guide-how-to-style-an-html-tag-directly/)
*   [如何用 CSS 设计列表样式](https://www.freecodecamp.org/news/how-to-style-lists-with-css/)
*   [如何用 CSS 设计按钮的样式](https://www.freecodecamp.org/news/a-quick-guide-to-styling-buttons-using-css-f64d4f96337f/)