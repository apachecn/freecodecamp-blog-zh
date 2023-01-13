# CSS 注释示例–如何注释掉 CSS

> 原文：<https://www.freecodecamp.org/news/comments-in-css/>

注释在 CSS 中用于解释代码块或在开发过程中进行临时更改。注释的代码不执行。

CSS 中的单行和多行注释都以`/*`开始，以`*/`结束，您可以向样式表添加任意多的注释。例如:

```
/* This is a single line comment*/
.group:after {
  content: "";
  display: table;
  clear: both;
}

/*
  This is
  a multi-line
  comment
*/
```

您还可以通过设置注释的样式来增加其可读性:

```
/*
***
* SECTION FOR H2 STYLE 
***
* A paragraph with information
* that would be useful for someone
* who didn't write the code.
* The asterisks around the paragraph 
* help make it more readable.
***
*/
```

## 用注释组织 CSS

在较大的项目中，CSS 文件的大小会快速增长，并且变得难以维护。用目录将 CSS 组织成不同的部分会很有帮助，以便将来更容易找到某些规则:

```
/* 
*  CSS TABLE OF CONTENTS
*   
*  1.0 - Reset
*  2.0 - Fonts
*  3.0 - Globals
*  4.0 - Color Palette
*  5.0 - Header
*  6.0 - Body
*    6.1 - Sliders
*    6.2 - Imagery
*  7.0 - Footer
*/

/*** 1.0 - Reset ***/

/*** 2.0 - Fonts ***/

/*** 3.0 - Globals ***/

/*** 4.0 - Color Palette ***/

/*** 5.0 - Header ***/

/*** 6.0 - Body ***/
h2 {
  font-size: 1.2em;
  font-family: "Ubuntu", serif;
  text-transform: uppercase;
}

/*** 5.1 - Sliders ***/

/*** 5.2 - Imagery ***/

/*** 7.0 - Footer ***/
```

## 关于 CSS 的更多信息: **CSS 语法和选择器**

当我们谈论 CSS 的语法时，我们谈论的是事物是如何布局的。有关于什么去哪里的规则，这样你可以一致地编写 CSS，程序(像浏览器)可以解释它并将其正确地应用到页面。

写 CSS 主要有两种方法。

### **内联 CSS**

关于 CSS 特异性的细节: [CSS 技巧](https://css-tricks.com/specifics-on-css-specificity/)

内联 CSS 将样式应用于单个元素及其子元素，直到遇到覆盖第一个元素的另一个样式。

要应用内联 CSS，请将“style”属性添加到您想要修改的 HTML 元素中。在引号内，包含一个分号分隔的键/值对列表(每个键/值对依次用冒号分隔)，指示要设置的样式。

这里有一个内联 CSS 的例子。单词“一”和“二”的背景颜色为黄色，文本颜色为红色。单词“Three”有一个新的样式，它覆盖了第一个样式，背景色为绿色，文本色为青色。在这个例子中，我们将样式应用于`<div>`标签，但是您可以将样式应用于任何 HTML 元素。

```
<div style="color:red; background:yellow">
  One
  <div>
    Two
  </div>
  <div style="color:cyan; background:green">
    Three
  </div>
</div>
```

### **内部 CSS**

虽然编写内联样式是更改单个元素的快速方法，但有一种更有效的方法可以一次将同一样式应用于页面的多个元素。

内部 CSS 的样式在`<style>`标签中指定，并且嵌入在`<head>`标签中。

这里有一个与上面的“内联”例子有相似效果的例子，除了 CSS 已经被提取到它自己的区域。单词“一”和“二”将匹配`div`选择器，并且是黄色背景上的红色文本。单词“三”和“四”也将匹配`div`选择器，但是它们也匹配`.nested_div`选择器，后者适用于引用该类的任何 HTML 元素。这个更具体的选择器覆盖了第一个选择器，它们最终显示为绿色背景上的青色文本。

```
<style type="text/css">
  div { color: red; background: yellow; }
  .nested_div { color: cyan; background: green; }
</style>

<div>
  One
  <div>
    Two
  </div>
  <div class="nested_div">
    Three
  </div>
  <div class="nested_div">
    Four
  </div>
</div>
```

上面显示的选择器非常简单，但是它们可以变得非常复杂。例如，可以只对嵌套元素应用样式；即，一个元素是另一个元素的子元素。

这里有一个例子，我们指定了一个样式，该样式只应用于作为其他`div`元素的直接子元素的`div`元素。结果是，“二”和“三”将显示为黄色背景上的红色文本，但“一”和“四”将不受影响(最有可能是白色背景上的黑色文本)。

```
<style type="text/css">
  div > div { color: red; background: yellow; }
</style>

<div>
  One
  <div>
    Two
  </div>
  <div>
    Three
  </div>
</div>
<div>
  Four
</div>
```

### **外部 CSS**

所有的样式都有自己的文档，链接在`<head>`标签中。链接文件的扩展名为`.css`