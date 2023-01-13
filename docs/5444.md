# 如何在 JavaScript 中反转一个数字

> 原文：<https://www.freecodecamp.org/news/js-basics-how-to-reverse-a-number-9aefc20afa8d/>

作者 artismarti

# 如何在 JavaScript 中反转一个数字

#### 使用箭头函数和常规 JS 函数的示例

![0*9bS6yWpHz8_tuY52](img/6775ab0d4d1625ccbaaa7afaad252646.png)

Photo by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

反转字符串或反转数字是编程面试中常见的问题之一。让我们来看看这是如何做到的。

#### **规则/限制**:

*   负数应该保持负数。

***ex。*** `-12345` *变成了* `-54321`

*   应该删除任何前导零。

***ex。*** `321000` *变成了* `123` *&而不是* `000123`

*   该函数可以接受浮点数或整数。

***ex。*** `543.2100` *变成了* `12.345`

*   该函数将整数作为整数返回。

***ex。*** `54321` *变成了* `12345` *&而不是* `12345.00`

#### **用箭头函数求解:**

```
const reversedNum = num => parseFloat(num.toString().split('').reverse().join('')) * Math.sign(num)
```

#### **用常规函数求解:**

```
function reversedNum(num) {
  return (
    parseFloat(
      num
        .toString()
        .split('')
        .reverse()
        .join('')
    ) * Math.sign(num)
  )                 
}
```

#### ***区别一箭函数&常规函数:***

在这个例子中，Arrow 函数和正则函数的唯一区别是正则函数需要提供一个显式的`return`值。

箭头函数有一个隐含的`return`值——如果它们可以写在一行中，而不需要`{}`括号。

[https://repl.it/@artismarti/ReversedNumbers?lite=true](https://repl.it/@artismarti/ReversedNumbers?lite=true)

#### **让我们把步骤分解一下:**

*   **将数字转换成字符串**

`num.toString()`将给定的数字转换成字符串。我们这样做是为了接下来可以对它使用`split`函数。

```
let num = -5432100
num.toString()

// num = '-5432100'
```

*   **将字符串拆分成一个数组**

`num.split('')`将字符串转换成字符数组。我们这样做是为了使用数组反转函数(*，它对字符串*无效)。

```
// num = '-5432100'

num.split('')

// num = [ '-', '5', '4', '3', '2', '1', '0', '0' ]
```

*   **反转阵列**

`num.reverse()`反转数组中项目的顺序。

```
// num = [ '-', '5', '4', '3', '2', '1', '0', '0' ]

num.reverse()

// num = [ '0', '0', '1', '2', '3', '4', '5', '-' ]
```

*   **把它重新连接成一串**

将颠倒的字符重新组合成一个字符串。

```
// num = [ '0', '0', '1', '2', '3', '4', '5', '-' ]

num.join('')

// num = '0012345-'
```

*   [**解析**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) **将输入值转换成浮点数:**

`parseFloat(num)`将`num`从字符串转换成浮点数。

```
// num = '0012345-'

parseFloat(num)

// num = 12345
```

**注意** : `parseFloat` 运行到最后*(即使它在函数的第一行)*反转的数字上，并删除任何前导零。

*   **乘以原始数字的[符号](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/sign)——保留负值。**

`num * Math.sign(num)`将该数字乘以所提供的原始数字的符号。

```
// original value of num = -5432100
// num = 12345

num * Math.sign(-5432100)

// num = -12345
```

而且，你有它！