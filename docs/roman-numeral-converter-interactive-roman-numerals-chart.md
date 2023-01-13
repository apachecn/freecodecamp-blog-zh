# 如何建立一个罗马数字转换器和一个交互式罗马数字图表

> 原文：<https://www.freecodecamp.org/news/roman-numeral-converter-interactive-roman-numerals-chart/>

罗马数字不再是我们日常生活中必不可少的一部分。但是我们在设计纪念碑、时钟，甚至是体育赛事时确实会用到它们。

## 罗马数字是什么？

罗马数字起源于古罗马，许多世纪以来一直是整个欧洲书写数字的通用方式。它们的使用比罗马帝国本身还要长久。它们逐渐被我们今天使用的印度-阿拉伯记数系统所取代——数字 0 到 9。

罗马数字由拉丁字母的组合来表示，在这个系统中用作数字。但是与十进制不同，罗马系统使用 7 个大写拉丁字母 **I，V，X，L，C，D，M** 。

最初，零没有单一的字母名称。相反，他们使用了拉丁词 **Nulla** ，意思是“没有”。

## 罗马数字是如何工作的？

这些字母的印度-阿拉伯表示法如下: **I = 1，V = 5，X = 10，L = 50，C = 100，D = 500，M = 1000** 。

其他的数字是由这些字母按照一定的规则组合而成的:一个符号放在之后**，另一个相同或更大值的符号增加它的值。**

比如 **VI = V + I = 5 + 1 = 6** 或者 **LX = L + X = 50 + 10 = 60** 。记法 VI 和 LX 读作“一比五”和“十比五十”。

放置在较大值之一的前的符号**减去其值。比如 **IX = X - I = 10 - 1 = 9、**和 **XC = C - X = 100 - 10 = 90** 。**

记法九和 XC 读作“一比十小”和“十比一百小”

大于 1，000 的数字由符号上的破折号组成。于是 **V̅ =五千**， **X̅ =一万**， **L̅ =五万**， **C̅ =十万**， **D̅ =五十万**， **M̅ =一百万**。

所谓的“标准”格式不允许同一符号连续使用三次以上。但偶尔也能看到例外。例如，编号为 4 的 IIII、编号为 9 的 VIIII 和编号为 90 的 LXXXX。

## 罗马数字及其组合的交互式图表

将鼠标悬停在每个符号上以显示其对应的印度-阿拉伯符号:

我写了这个交互式罗马数字图表的代码，嵌入到 freeCodeCamp 新闻中。

鉴于 HTML 嵌入功能不是一个完整的代码编辑器，给定的代码不是结构化的，也不是作为单独的 HTML、CSS 和 JavaScript 文件呈现的。相反，它是作为一个单独的 HTML 文件编写的，添加了用于样式和功能的`<style>`和`<script>`元素。

这里是我的交互式罗马数字图表的完整代码库。

## 罗马数字转换器

请输入一个介于 0 和 5，000 之间的非负整数。然后点按“转换”以显示等效的罗马数字。

* * *

5000 或更大的数字没有程序限制。控制转换的算法将照样工作。

显示大数的罗马数字等价物所需的空间变得越来越大，却没有带来揭示新事物的额外好处。

代码本身由一个 HTML 部分组成，该部分用内联样式描述内容以方便交互，并添加了 JavaScript 以实现功能。

是“number”类型的输入元素，用于将输入数据限制为数值和两个按钮。“Convert”按钮连接到执行转换的函数，“Display”按钮输出等价的罗马数字。

为什么通过按钮元素输出？两个按钮一起使用时，样式效果很好。考虑到嵌入的有限功能，它看起来像是一个时间节省器。

为了清楚起见，这些元素被分配给变量:

```
const inputField = document.querySelector('input'); // input element
const convertButton = document.getElementById('convert'); // convert button
const outputField = document.getElementById('display'); // output element
```

函数`convertToRoman()`包含逻辑并呈现结果:

```
function convertToRoman() {
  let arabic = document.getElementById('arabicNumeral').value; // input value
  let roman = '';  // variable that will hold the result
}
```

输入元素的数值保存在一个名为“ **arabic** 的变量中，以供进一步测试。名为“ **roman** 的变量将保存代表阿拉伯语输入的罗马字母的字符串。

接下来，有两个长度相等的数组，一个保存阿拉伯数字，一个保存罗马数字。为了简化减法，两者都按降序排列:

```
// descending order simplifies subtraction while looping
const arabicArray = [5000, 4000, 1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1] 
const romanArray = ['V&#773;', 'MV&#773;','M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I'] 
```

Unicode 表有助于形成大于 1，000 的符号。

最后，这里是测试输入数字并转换它的逻辑。

```
if (/^(0|[1-9]\d*)$/.test(arabic)) {
  // Regular expression tests
  if (arabic == 0) {
    // for decimal points and negative
    outputField.innerHTML = "Nulla"; // signs
  } else if (arabic != 0) {
    for (let i = 0; i < arabicArray.length; i++) {
      while (arabicArray[i] <= arabic) {
        roman += romanArray[i];
        arabic -= arabicArray[i];
      }
    }
    outputField.innerHTML = roman;
  }
} else {
  outputField.innerHTML =
    "Please enter non negative integers only. No decimal points.";
}
```

第一个测试检查小数点和负号。如果找到，消息会要求“仅输入非负整数”

下一个测试检查输入的数字是否等于零。在这种情况下，将显示字符串“Nulla”。

否则，循环在减去阿拉伯数字的同时继续连接罗马字符串，直到后者满足 while 循环的条件。然后，它显示用户输入的罗马字母。

就像交互式图表一样，罗马数字转换器的代码已经设置好，可以复制并嵌入到任何文章中。这里是完整的代码库。