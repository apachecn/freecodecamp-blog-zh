# jQuery 选择器解释:类选择器、ID 选择器等等

> 原文：<https://www.freecodecamp.org/news/jquery-selectors/>

## **jQuery 选择器**

jQuery 使用 CSS 样式的选择器来选择 HTML 页面的部分或元素。然后，它让您使用 jQuery 方法或函数对元素做一些事情。

要使用其中一个选择器，请键入一个美元符号并在其后加上括号:`$()`。这是`jQuery()`函数的简写。在括号内，添加要选择的元素。您可以使用单引号或双引号。在这之后，在括号和您想要使用的方法后面添加一个点。

在 jQuery 中，类和 ID 选择器类似于 CSS 中的选择器。在我们继续之前，让我们快速回顾一下。

## **CSS 中的 ID 选择器**

CSS ID 选择器将样式应用于特定的 html 元素。CSS ID 选择器必须匹配 HTML 元素的 ID 属性。与可以应用于整个站点中多个元素的类不同，一个特定的 ID 只能应用于站点中的一个元素。CSS ID 将覆盖 CSS 类属性。要选择具有特定 id 的元素，请编写一个哈希(#)字符，后跟元素的 id。

### **语法**

```
#specified_id { /* styles */ }
```

您可以将 ID 选择器与其他类型的选择器结合使用，来设计一个非常特殊的元素。

```
section#about:hover { color: blue; }

div.classname#specified_id { color: green; }
```

### **关于 id 的注意事项**

如果可能，在造型时应避免 ID。因为它有很高的专一性，只有你内联样式，或者添加样式到`<style>`中，它才能被覆盖。ID 的权重覆盖类选择器和类型选择器。

记住，ID 选择器必须匹配 HTML 元素的 ID 属性。

```
<div id="specified_id"><!-- content --></div>
```

### **特异性**

ID 选择器具有很高的特异性，使得它们很难被覆盖。类具有较低的特异性，通常是设计元素样式的首选方式，以避免特异性问题。

下面是一个 jQuery 方法的示例，它选择所有段落元素，并向它们添加一个“selected”类:

```
<p>This is a paragraph selected by a jQuery method.</p>
<p>This is also a paragraph selected by a jQuery method.</p>

$("p").addClass("selected");
```

## 好了，回到 jQuery

在 jQuery 中，类和 ID 选择器与 CSS 中的相同。如果您想要选择具有特定类别的元素，请使用点(`.`)和类别名称。如果您想要选择具有特定 ID 的元素，请使用散列符号(`#`)和 ID 名称。请注意，HTML 不区分大小写，因此保持 HTML 标记和 CSS 选择器小写是最佳实践。

按类别选择:

```
<p class="p-with-class">Paragraph with a class.</p>

$(".p-with-class").css("color", "blue"); // colors the text blue
```

按 ID 选择:

```
<li id="li-with-id">List item with an ID.</li>

$("#li-with-id").replaceWith("<p>Socks</p>");
```

您还可以选择某些元素及其类和 id:

### **按类别选择**

如果您想要选择具有特定类别的元素，请使用点(。)和类名。

```
<p class="pWithClass">Paragraph with a class.</p>
```

```
$(".pWithClass").css("color", "blue"); // colors the text blue
```

为了更加具体，还可以将类选择器与标记名结合使用。

```
<ul class="wishList">My Wish List</ul>`<br>
```

```
$("ul.wishList").append("<li>New blender</li>");
```

### **按 ID 选择**

如果要选择具有特定 ID 值的元素，请使用散列符号(#)和 ID 名称。

```
<li id="liWithID">List item with an ID.</li>
```

```
$("#liWithID").replaceWith("<p>Socks</p>");
```

与类选择器一样，这也可以与标记名结合使用。

```
<h1 id="headline">News Headline</h1>
```

```
$("h1#headline").css("font-size", "2em");
```

### **充当过滤器的选择器**

还有充当过滤器的选择器——它们通常以冒号开头。例如，`:first`选择器选择的元素是其父元素的第一个子元素。下面是一个包含一些列表项的无序列表的例子。列表下方的 jQuery 选择器选择列表中的第一个`<li>`元素——“一”列表项——然后使用`.css`方法将文本变成绿色。

```
 <ul>
      <li>One</li>
      <li>Two</li>
      <li>Three</li>
   </ul>
```

```
$("li:first").css("color", "green");
```

****注意:**** 不要忘记在 JavaScript 中应用 css 并不是一个好的做法。你应该总是在 css 文件中给出你的样式。

另一个过滤选择器`:contains(text)`，选择具有特定文本的元素。将您想要匹配的文本放在括号中。这里有一个两个段落的例子。jQuery 选择器将单词“Moto”的颜色改为黄色。

```
 <p>Hello</p>
    <p>World</p>
```

```
$("p:contains('World')").css("color", "yellow");
```

类似地，`:last`选择器选择作为其父元素最后一个子元素的元素。下面的 JQuery 选择器选择列表中的最后一个`<li>`元素——“三”列表项——然后使用`.css`方法将文本变成黄色。

`$("li:last").css("color", "yellow");`

****注意:**** 在 jQuery 选择器中，`World`在单引号中，因为它已经在一对双引号中。始终在双引号内使用单引号，以避免无意中结束字符串。

****多个选择器**** 在 jQuery 中，可以使用多个选择器将相同的更改应用于多个元素，只需一行代码。您可以通过用逗号分隔不同的 id 来做到这一点。例如，如果您想将 id 分别为 cat、dog 和 rat 的三个元素的背景色设置为红色，只需执行以下操作:

```
$("#cat,#dog,#rat").css("background-color","red");
```

这些只是 jQuery 中可用的几个选择器。有关 jQuery 网站上完整列表的链接，请参见更多信息部分。

#### **更多信息:**

*   [jQuery 选择器的完整列表](http://api.jquery.com/category/selectors/)