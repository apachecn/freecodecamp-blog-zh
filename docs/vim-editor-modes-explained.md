# 解释了 Vim 编辑器模式

> 原文：<https://www.freecodecamp.org/news/vim-editor-modes-explained/>

因为 Vim 像编写新代码一样关注于修改现有代码，所以它被分成几种模式，每种模式有不同的目的。

### **正常模式**

默认情况下，Vim 以“正常”模式启动。按下`Esc`或`<C-[>`可以从其他模式进入正常模式。

在正常模式下，按键不像预期的那样工作。也就是说，他们不在文档中插入文本；相反，某些按键可以:

#### **移动光标**

*   ****h**向左移动一个字符**
*   ****j**** 下移一行
*   ****k**** 上移一行
*   ****l**** 向右移动一个字符

和许多 vim 命令一样，行移动可以以数字为前缀，一次移动几行:

*   ****4j**** 下移 4 行
*   ****6k**上移 6 排** 

基本单词移动:

*   ****w**** 移动到下一个单词的开头
*   ****b**** 移动到上一个单词的开头
*   ****e**** 移动到字尾
*   ****W**** 在一个空格后移动到下一个单词的开头
*   ****B**** 移动到前一个单词的开头，在一个空格之前
*   ****E**** 移动到单词末尾的一个空格前

线运动的开始/结束:

*   ****0**** 移动到行的开头
*   ****$**** 移动到行尾

#### **操纵文本**

#### **进入其他模式**

****正常模式**** 是使用 Vim 时应该花大部分时间的地方。记住，这是 Vim 与众不同的地方。

在正常模式下，有多种方法可以在打开的文件中移动。除了使用光标键四处移动，您还可以使用`h`(左)、`j`(下)、`k`(上)、和`l`(右)来移动。这特别有助于接触那些不喜欢在做改变时离开家的打字员。

您也可以在正常模式下更改单个字符。例如，要替换单个字符，将光标移至该字符上并按下`r`，然后按下要替换的字符。类似地，您可以通过将光标移到单个字符上并按下`x`来删除单个字符。

要执行撤消操作，请在正常模式下按`u`。这将撤消上一次在正常模式下所做的更改。如果您想重做(*即*，撤销您的撤销)，在正常模式下按`Ctrl+r`。

### **插入模式**

这是第二个最常用的模式，也是大多数人最熟悉的行为。一旦进入插入模式，键入就像常规文本编辑器一样插入字符。您可以在正常模式下使用 insert 命令输入它。

插入命令包括:

*   `i`对于' ****i**** nsert '，这立即将 vim 切换到插入模式
*   `a`对于' ****a**** ppend '，这将光标移动到当前字符后，进入插入模式
*   `o`在当前行下方插入新行，并在新行上进入插入模式

这些命令也是大写的:

*   `I`将光标移动到行首，进入插入模式
*   `A`将光标移动到行尾，进入插入模式
*   `O`在当前行的上方插入新行，并在新行上进入插入模式

在 Vim 中插入文本的方式还有很多，这里无法一一列举，但这些是最简单的。此外，注意不要停留在插入模式太久；Vim 并不是设计用来一直在插入模式下使用的。

要退出插入模式并返回正常模式，按下`Esc`或`<C-[>`

### **视觉模式**

视觉模式用于选择文本，类似于用鼠标单击和拖动的行为。选择文本允许命令仅应用于所选内容，如复制、删除、替换等。

要进行文本选择:

*   按`v`进入可视模式，这也将标记一个开始选择点
*   将光标移动到所需的结束选择点；vim 将提供文本选择的视觉突出显示

视觉模式也有以下变体:

*   `V`要进入可视行模式，这将逐行进行文本选择
*   `<C-V>`进入视觉块模式，这将按块进行文本选择；移动光标将对文本进行矩形选择

要退出视觉模式并返回正常模式，按下`Esc`或`<C-[>`。

视觉模式实际上有多个子类型:*视觉*、*块视觉*和*线视觉*

*   *视觉*:如上所述。按下`v`进入
*   *块视觉*:选择任意矩形区域。按下`<ctrl>+v`进入
*   *逐行视觉*:始终选择整行。按下`<shift>+v`进入

### **命令模式**

命令模式有各种各样的命令，可以轻松地完成普通模式无法完成的任务。要进入命令模式，请在正常模式下键入“:”，然后键入您的命令，该命令将出现在窗口的底部。例如，要进行全局查找和替换，请键入`:%s/foo/bar/g`将所有“foo”替换为“bar”

*   `:`进入命令模式
*   `%`表示跨越所有线
*   `s`表示替代品
*   `/foo`是 regex 找东西替换
*   `/bar/`是用正则表达式替换事物
*   `/g`表示全局，否则每行只执行一次

Vim 有许多其他的方法，您可以在帮助文档中读到，`:h`或`:help`。

### **替换模式**

替换模式允许您通过直接在文本上键入来替换现有文本。在进入这个模式之前，进入正常模式，把你的光标放在你想要替换的第一个字符上。然后按“R”(大写 R)进入替换模式。现在，无论您键入什么，都将替换现有文本。光标会自动移动到下一个字符，就像在插入模式下一样。唯一的区别是，您键入的每个字符都将替换现有的字符。