# JavaScript 开关案例介绍

> 原文：<https://www.freecodecamp.org/news/introduction-to-javascript-switch-cases/>

在这篇短文中，我将向您介绍 JavaScript switch 案例，以及如何通过实际例子来使用它们。

本文将通过更实际的例子进行更好的解释，以帮助您深入理解开关案例。

### 先决条件。

*   基本的 JavaScript 知识
*   代码编辑器
*   网络浏览器
*   你的大脑:)

一个`switch`语句基本可以代替 JavaScript 中的多个`if`检查。

它提供了一种更具描述性的方法来比较一个值与多个变量。

### **开关语法**

`switch`有一个或多个`case`模块和一个可选的默认情况。

```
switch(x) {
  case 'value1':  // if (x === 'value1')
    //code here
    [break]

  case 'value2':  // if (x === 'value2')
    //code here
    [break]

  default:
    //code here
    [break]
}
```

*   检查`x`的值是否严格等于从第一个`case`(即`value1`)到第二个`value2`的值，依此类推。
*   如果发现相等，`switch`从相应的`case`开始执行代码，直到最近的`break`(或直到`switch`结束)。
*   如果没有匹配的案例，则执行`default`代码(如果存在)。

### **一些真实的例子**

*   *****简单播放&暂停开关*****

`switch`语句可用于基于数字或字符串的多个分支:

```
switch (movie) {
  case 'play':
    playMovie();
    break;
  case 'pause':
    pauseMovie();
    break;
  default:
    doNothing();
}
```

如果不添加一个`break`语句，执行将“失败”到下一级。如果您真的想用注释来帮助调试，那么您有必要特意用注释来标记失败:

```
switch (movie) {
  case 'play': // fallthrough
  case 'pause':
    pauseMovie();
    break;
  default:
    doNothing();
}
```

默认子句是可选的。如果你愿意，你可以在开关部分和格中都有表达式；使用`===`运算符在两者之间进行比较:

```
switch (3 + 7) {
  case 5 + 5:
    correct();
    break;
  default:
    neverhappens();
}
```

*   *****简单数学计算开关*****

```
let average = 2 + 6;

switch (average) {
  case 4:
    alert( 'Too small' );
    break;
  case 8:
    alert( 'Exactly!' );
    break;
  case 10:
    alert( 'Too large' );
    break;
  default:
    alert( "Incorrect values!" );
}
```

这里`switch`开始从第一个`case`变体`4`开始比较`average` 。匹配失败。

然后`8`。这是一个匹配，所以执行从`case 8`开始，直到最近的`break`。

如果 ****没有**** `****break****` ****，那么执行继续进行下一个**** `****case****` ****而不做任何检查。****

下面是一个没有`break`的例子:

```
let average = 2 + 6;

switch (average) {
  case 4:
    alert( 'Too small' );
  case 8:
    alert( 'Exactly!' );
  case 10:
    alert( 'Too big' );
  default:
    alert( "Incorrect values!" );
}
```

在上面的例子中，我们将看到三个`alerts`的顺序执行:

```
alert( 'Exactly!' );
alert( 'Too big' );
alert( "Incorrect values!" );
```

*   *****【getDay】*****方法切换案例

`getDay()`方法以 0 到 6 之间的数字返回星期几。

> *星期日=0，星期一=1，星期二=2，星期三=3，星期四=4，星期五=5，星期六=6*

此示例使用星期几来计算星期名称:

```
switch (new Date().getDay()) {
  case 0:
    day = "Sunday";
    break;
  case 1:
    day = "Monday";
    break;
  case 2:
     day = "Tuesday";
    break;
  case 3:
    day = "Wednesday";
    break;
  case 4:
    day = "Thursday";
    break;
  case 5:
    day = "Friday";
    break;
  case 6:
    day = "Saturday";
}
```

day 的结果将是以 day 格式表示的当前工作日

PS:这将根据你阅读这篇文章的时间而改变

我写这篇文章的时间是 2019 年 6 月 13 日，也就是周四，所以结果应该是:

```
Thursday
```

### 默认关键字

****默认**** 关键字指定在没有大小写匹配的情况下运行的代码，更像是一个 else 语句:

```
switch (new Date().getDay()) {
  case 6:
    text = "Today is Saturday";
    break; 
  case 0:
    text = "Today is Sunday";
    break; 
  default: 
    text = "Its not weekend yet!";
}
```

文本的结果将是:

```
Its not weekend yet!
```

****默认**** 情况不必是开关块中的最后一个情况:

```
switch (new Date().getDay()) {
  default: 
    text = "Its not weekend yet!";
    break;
  case 6:
    text = "Today is Saturday";
    break; 
  case 0:
    text = "Today is Sunday";
}
```

> *****如果 default 不是 switch 块中的最后一个 case，记得用一个 break 结束 default case。*****

### **结论**

开关案例的实际例子太多了，你可以去[google.com](https://google.com/)快速搜索更多的开关案例。

如果这篇文章对你有帮助，请分享出来。

感谢阅读！