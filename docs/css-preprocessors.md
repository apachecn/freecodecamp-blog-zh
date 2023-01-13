# CSS 预处理程序解释

> 原文：<https://www.freecodecamp.org/news/css-preprocessors/>

CSS 预处理程序越来越成为前端 web 开发人员工作流程中的中流砥柱。CSS 是一种极其复杂和微妙的语言，为了使它的使用更容易，开发人员经常转向使用预处理程序，如 SASS 或 LESS。

CSS 预处理程序编译使用特殊编译器编写的代码。然后他们用它来创建一个 CSS 文件，这个文件可以被主 HTML 文档引用。

当使用任何 CSS 预处理器时，你将能够用普通的 CSS 编程，就像没有预处理器一样。好的一面是你也有更多的选择。有些，比如 SASS，有特定的样式标准，这意味着使文档的编写更加容易，比如如果你愿意可以省略大括号。

当然，CSS 预处理程序也提供了其他功能。所提供的许多特性在各个预处理程序之间极其相似，只有语法上的细微差别。因此，你可以选择任何一个你想要的，你将能够实现同样的事情。一些更有用的功能包括:

### **变量**

任何编程语言中最常用的一项是变量，这是 CSS 明显缺乏的。通过使用变量，你可以一次定义一个值，然后在整个程序中重用。SASS 中的一个例子是:

```
$yourcolor: #000056
.yourDiv {
  color: $yourcolor;
 }
```

通过声明一次`SASS yourcolor`变量，现在可以在整个文档中重复使用相同的颜色，而不必重新输入定义。

### **循环**

语言中另一个常见的项目是循环，这是 CSS 所没有的。循环可用于多次重复相同的指令，而无需多次重新输入。SASS 的一个例子是:

```
@for $vfoo 35px to 85px {
  .margin-#{vfoo} {
    margin: $vfoo 10px;
   }
 }
```

这个循环使我们不必多次编写相同的代码来改变边距大小。

### **If/Else 语句**

CSS 缺少的另一个特性是 If/Else 语句。只有当给定的条件为真时，它们才会运行一组指令。SASS 中的一个例子是:

```
@if width(body) > 500px {
  background color: blue;
 } else {
  background color: white;
 }
```

这里，背景颜色会根据页面主体的宽度而改变颜色。

这些只是 CSS 预处理程序的几个主要功能。正如你所看到的，CSS 预处理程序是非常有用和多样的工具。很多 web 开发人员都在使用，强烈建议至少学习其中一种。

#### **更多信息:**

*   [最好的 CSS 教程](https://www.freecodecamp.org/news/best-css-and-css3-tutorial/)
*   萨斯文件:[http://sass-lang.com/](http://sass-lang.com/)
*   [SASS，你的网页装饰的预处理器](https://www.freecodecamp.org/news/give-more-oompf-to-your-web-garnishes-with-preprocessors-in-sass-bd379226a114/)
*   少文件:[http://lesscss.org/](http://lesscss.org/)
*   手写笔文档:[http://stylus-lang.com/](http://stylus-lang.com/)