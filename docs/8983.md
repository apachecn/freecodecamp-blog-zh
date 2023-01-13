# 如何在媒体中显示代码块

> 原文：<https://www.freecodecamp.org/news/how-to-add-code-to-medium-and-get-syntax-highlighting-d699761a5883/>

#### 以及如何编写内嵌代码和嵌入代码来突出显示语法

Medium 使添加代码块和内联变得容易

#### 中有一个隐藏的快捷方式，将文本变成纯文本…

if (developer === true) {

follow(this . medium publication)；

}

#### …转换成格式化的代码块:

```
if (developer === true) {
```

```
 follow(this.mediumPublication);
```

```
}
```

为此，选择文本，然后按:

*   **Windows** : Control + Alt + 6
*   **Mac** : Command + Option + 6
*   **Linux** : Control + Alt + 6

**2016 年 10 月 19 日更新:** Medium 现在也支持以三个反勾号开始代码块的常见约定。如果你在新的一行输入`` ` `` ', Medium 将切换到代码输入模式。非常感谢[卢克·埃斯特金](https://www.freecodecamp.org/news/how-to-add-code-to-medium-and-get-syntax-highlighting-d699761a5883/undefined)和 Medium 团队的其他成员实现了这一点！

您也可以通过选择代码，然后单击反勾号键(`)来突出显示代码。例如，您可以在句子中间将文本格式化为代码`freeCodeCamp()`。

您也可以通过将 GitHub gistss 的 URL 粘贴到空行中，然后按 enter 键来嵌入 GitHub gist:

### 如何嵌入 web 应用程序

额外的好处是，你可以将可运行的应用嵌入到媒体中。以防这些不能在 Medium 的移动应用程序中正确呈现，我建议在每个嵌入的下面包含链接作为后备。

将 CodePen.io URL 粘贴到介质中，然后按 enter:

*查看我的密码[这里](http://codepen.io/FreeCodeCamp/pen/NNvBQW)。*

您也可以使用 JSFiddle.net 来完成这项工作:

*查看我的 JSFiddle [这里](https://jsfiddle.net/avegt5e5/3/)。*

### 但是不要使用代码的图像。

尽管截取代码截图并粘贴到 Medium 中似乎很方便，但不要这样做。

Medium 还没有 alt-text 选项，所以这将使您的代码对于有视觉障碍的开发人员来说完全不可访问。是的，[他们就在那里](https://medium.freecodecamp.com/looking-back-to-what-started-it-all-731ef5424aec#.fae9jgbx6)。

你也不能改变静态图片的字体大小，这使得在手机上阅读很痛苦(这是大多数人阅读的媒介)。

最后，这使得读者无法复制和粘贴您的代码。

我知道，我知道——无论如何你都不应该复制粘贴代码。

但是让我们面对现实吧——人们确实如此。如果面对手动输入大量代码的前景，大多数人会很快放弃你的教程。

概括一下:

*   使用介质的本机代码块
*   或者嵌入 GitHub gists
*   尽可能使用来自 CodePen 和 JSFiddle 的工作示例
*   但是不要粘贴代码的图像

编码快乐，写关于编码的东西！

我只写编程和技术。如果你在推特上关注我，我不会浪费你的时间。？