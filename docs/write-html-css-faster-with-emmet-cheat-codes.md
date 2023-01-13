# 如何用 Emmet 作弊代码更快地编写 HTML/CSS

> 原文：<https://www.freecodecamp.org/news/write-html-css-faster-with-emmet-cheat-codes/>

Emmet 是我最喜欢的 VS 代码内置特性之一。它也可以作为其他代码编辑器的扩展。

把埃米特想象成速记。有了它，你可以轻松快速地编写大量代码。它大大加快了你的 HTML & CSS 工作流程。

理解如何使用 Emmet 确实是一种超能力。有些人甚至称之为编码欺骗代码。？

而这只是 VS 代码众多令人惊叹的内置特性之一。

最近我推出了一个全面详细的课程叫做 ****[成为 VS 代码超级英雄](https://courses.codestackr.com/vs-code-superhero?coupon=LAUNCH) ****。******** 它非常深入地涵盖了 VS 代码的方方面面。

本文基于课程 11 节之一: ******写作&格式化代码****。**

## 超文本标记语言

有了 Emmet，你可以在眨眼之间快速创建 HTML 样板文件。在一个 HTML 文件中，只需输入`!`，你就会看到 Emmet 作为一个建议出现了。现在打`Enter`。就这样，一个基本的空白 HTML 网页已经准备好了。

如果你以前从未见过 HTML，也不知道这是什么，不要担心。我只是在演示 VS Code 和 Emmet 的能力。你现在不需要知道这些意味着什么。

### 基本标签

要创建基本的 HTML 标签，只需键入标签的名称并点击`Enter`。请注意 Emmet 是如何将光标放在标签内的。现在，您可以继续在标签中输入内容了。

*   试着输入:`div`然后`Enter`，或者`h1 Enter`，或者`p Enter`。
*   这些工作也一样:`<blockquote>`的`bq`，`<header>`的`hdr`，`<footer>`的`ftr`，`<button>`的`btn`，以及`sect`的一个部分。

### 班级

如果你熟悉 CSS，Emmet 使用相同的方式引用使用`.`的类。要定义一个带有标签的类，只需像这样添加它:

*   `div.wrapper` — > `<div class="wrapper"></div>`
*   `h1.header.center` — > `<h1 class="header center"></h1>`

### ID 的

Id 的工作大同小异:

*   `div#hero` — > `<div id="hero"></div>`

### 结合

你可以把这些串起来:

*   `div#hero.wrapper` — > `<div id="hero" class="wrapper"></div>`

### 属性

我们还可以为标签指定属性:

*   `img[src="cat.jpg"][alt="Cute cat pic"]` — > `<img src="cat.jpg" alt="Cute cat pic" />`

### 内容

要在标签中包含内容，我们只需将内容放在大括号中，`{ }`。

*   `p{This is a paragraph}` — > `<p>This is a paragraph</p>`

### 兄弟姐妹和孩子

越来越好了。我们还可以使用`+`和`>`字符来指定兄弟姐妹和孩子。

*   `section+section` — > `<section></section><section></section>`
*   `ul>li` — > `<ul><li></li></ul>`

### 向上爬

当你键入 Emmet 速记的时候，你必须试着在你的脑海中描绘出你正在构建的东西。在这个例子中，我们将使用`^`来“爬上”树。

`div+div>p>span+em^bq`

结果:

```
<div></div>
<div>
    <p><span></span><em></em></p>
    <blockquote></blockquote>
</div> 
```

这里，我们希望块引用和段落在同一层。正因为如此，我们需要“爬上去”。否则，它会出现在段落中。

### 分组

如果您的结构非常复杂，您可能希望对标签进行分组，而不是通过攀爬来遍历。

在这个例子中，我们将使用括号`( )`创建一个页眉和页脚。

`div>(header>ul>li>a)+footer>p`

结果:

```
<div>
    <header>
        <ul>
            <li><a href=""></a></li>
        </ul>
    </header>
    <footer>
        <p></p>
    </footer>
</div> 
```

### 乘法和美元

我们可以通过乘以(`*`)并使用美元符号(`$`)按顺序对项目进行编号来生成多个标签。

*   `ul>li*5` — > `<ul><li></li><li></li><li></li><li></li><li></li></ul>`
*   `ul>li{Item $}*3` — > `<ul><li>Item 1</li><li>Item 2</li><li>Item 3</li></ul>`

您甚至可以自定义编号顺序，用零填充，从一个特定的数字开始，甚至颠倒方向。

用零填充:`ul>li.item$$$*5`

结果:

```
<ul>
    <li class="item001"></li>
    <li class="item002"></li>
    <li class="item003"></li>
    <li class="item004"></li>
    <li class="item005"></li>
</ul> 
```

从一个特定的数字开始:`ul>li.item$@3*5`

结果:

```
<ul>
    <li class="item3"></li>
    <li class="item4"></li>
    <li class="item5"></li>
    <li class="item6"></li>
    <li class="item7"></li>
</ul> 
```

反向:`ul>li.item$@-*5`

结果:

```
<ul>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
    <li class="item2"></li>
    <li class="item1"></li>
</ul> 
```

从特定数字开始反向:`ul>li.item$@-3*5`

结果:

```
<ul>
    <li class="item7"></li>
    <li class="item6"></li>
    <li class="item5"></li>
    <li class="item4"></li>
    <li class="item3"></li>
</ul> 
```

### 隐式标记名

有一些标签不需要输入，可以隐含。

*   最初定义的没有标签的类将被隐含为`<div>`。
    `.wrapper` — > `<div class="wrapper"></div>`
*   强调标签中定义的类将被隐含为`<span>`。
    `em>.emphasis` — > `<em><span class="emphasis"></span></em>`
*   列表中定义的类将隐含为列表项。
    `ul>.item`——>`<ul><li class="item"></li></ul>`
*   在表中定义的类将被隐含为一个`<tr>`，在行中定义的类将被隐含为一个`<td>`。
    `table>.row>.col`——>`<table><tr class="row"><td class="col"></td></tr></table>`

### 用标签换行

有时候，您需要用标签来包装现有的代码。我们可以轻松搞定埃米特。

只需突出显示您想要包装的代码，然后打开命令托盘(`F1`)。然后搜索`Emmet: Wrap with Abbreviation`。然后会出现一个对话框，您可以在其中输入元素。

`test` — > `<div>test</div>`

您也可以在此对话框中使用标准 Emmet 语法。尝试用`span.wrapper`将一些文本换行。

默认情况下，此功能不会分配给键盘快捷键。因此，如果您发现自己经常使用它，您可能希望为它添加一个自定义快捷方式。

### Lorem Ipsum

“Lorem Ipsum”是开发人员用来表示页面数据的虚拟文本。只需键入`lorem`并点击`Enter`。Emmet 将生成 30 个单词的假文本，您可以在您的项目中用作填充。

也可以定义生成的单词量。

*   `lorem10`会给你 10 个字的随机文字。

### 把所有的放在一起

让我们用几个我们到目前为止学到的东西。试试这个:

`ul.my-list>lorem10.item-$*5`

结果:

```
<ul class="my-list">
  <li class="item-1">Lorem ipsum dolor sit amet.</li>
  <li class="item-2">Numquam repudiandae fuga porro consequatur?</li>
  <li class="item-3">Culpa, est. Tenetur, deleniti nihil?</li>
  <li class="item-4">Numquam architecto corrupti quam repudiandae.</li>
</ul> 
```

## 半铸钢ˌ钢性铸铁(Cast Semi-Steel)

在 CSS 中，Emmet 对每个属性都有一个缩写。我不打算全部列出，但我会指出我最常用的。要查看完整列表，请参考 Emmet [备忘单](https://docs.emmet.io/cheat-sheet/)。

### 位置

*   `pos` — > `position:relative;`(默认为相对)
*   `pos:s` — > `position:static;`
*   `pos:a` — > `position:absolute;`
*   `pos:r` — > `position:relative;`
*   `pos:f` — > `position:fixed;`

### 显示

*   `d` — > `display:block;`(默认为封锁)
*   `d:n` — > `display:none;`
*   `d:b` — > `display:block;`
*   `d:f` — > `display:flex;`
*   `d:if` — > `display:inline-flex;`
*   `d:i` — > `display:inline;`
*   `d:ib` — > `display:inline-block;`

### 光标

*   `cur` — > `cursor:pointer;`

### 颜色

*   `c` — > `color:#000;`
*   `c:r` — > `color:rgb(0, 0, 0);`
*   `c:ra` — > `color:rgba(0, 0, 0, .5);`
*   `op` — > `opacity: ;`

### 边距和填充

*   `m` — > `margin: ;`
*   `m:a` — > `margin:auto;`
*   `mt` — > `margin-top: ;`
*   `mr` — > `margin-right: ;`
*   `mb` — > `margin-bottom: ;`
*   `ml` — > `margin-left: ;`
*   `p` — > `padding: ;`
*   `pt` — > `padding-top: ;`
*   `pr` — > `padding-right: ;`
*   `pb` — > `padding-bottom: ;`
*   `pl` — > `padding-left: ;`

### 盒子尺寸

*   `bxz` — > `box-sizing:border-box;`

### 宽度

*   `w` — > `width: ;`
*   `h` — > `height: ;`
*   `maw` — > `max-width: ;`
*   `mah` — > `max-height: ;`
*   `miw` — > `min-width: ;`
*   `mih` — > `min-height: ;`

### 边境

*   `bd` — > `border: ;`
*   `bd+` — > `border:1px solid #000;`
*   `bd:n` — > `border:none;`

### 埃米特太棒了。

使用 Emmet，您可以用一行代码创建一个非常复杂的 HTML 结构。真的很牛逼。并且，它也可以和 CSS 一起工作。

你可以看到 Emmet 如何在编写 HTML 和 CSS 时大幅提高你的效率和速度。

如果你想进一步提高使用 VS 代码的效率和速度，请查看我的课程 **[成为 VS 代码超级英雄](https://courses.codestackr.com/vs-code-superhero?coupon=LAUNCH)。**

本课程更深入地探究这些概念，并帮助你成为一个快速、高效的超级英雄开发者:)

![Jesse Hall (codeSTACKr)](img/b090f5cecaf26a1ed03bc31eac287d77.png)

我是来自德克萨斯州的杰西。查看我的其他内容，让我知道如何帮助你成为一名 web 开发人员。

*   [订阅我的 YouTube](https://youtube.com/codeSTACKr)
*   打个招呼！ [Instagram](https://instagram.com/codeSTACKr) | [推特](https://twitter.com/codeSTACKr)
*   [注册我的简讯](https://codestackr.com/)