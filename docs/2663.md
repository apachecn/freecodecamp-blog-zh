# JavaScript 拆分字符串示例——如何在 JS 中将一个字符串拆分成一个数组

> 原文：<https://www.freecodecamp.org/news/javascript-split-string-example/>

字符串是表示字符序列的数据结构，数组是包含多个值的数据结构。

你知道吗——一个字符串可以用`split`方法分解成多个字符串的数组。让我们通过一些例子来看看它是如何工作的。

## TL；速度三角形定位法(dead reckoning)

如果你只是想要密码，给你:

```
const publisher = 'free code camp'
publisher.split(' ') // [ 'free', 'code', 'camp' ] 
```

## 句法

根据 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) ，你需要拆分字符串的语法是`str.split([separator[, limit]])`。如果我们把这个应用到上面的例子中:

*   `str`是`publisher`
*   `separator`是`' '`
*   没有`limit`

## 什么时候需要拆分字符串？

### 示例 1:获取字符串的一部分

下面是一个常见的例子，它涉及从 auth 头获取令牌，auth 头是基于令牌的身份验证系统的一部分。

如果这对你没有任何意义，没关系。对于下面的例子，您只需要知道有一个值为`bearer token`的字符串，但是只需要`token`(因为这是标识用户的部分):

```
const authHeader = 'bearer token'
const split = authHeader.split(' ') // (1) [ 'bearer', 'token' ]
const token = split[1] // (2) token
```

下面是上面代码中发生的情况:

1.  以`' '`为分隔符分割字符串
2.  访问数组中的第二个条目

### 示例 2:将数组方法应用于字符串

通常给你的输入是一个字符串，但是你想对它应用数组方法(例如`map`、`filter`或`reduce`)。

例如，假设给你一串莫尔斯电码，你想看看它用英语怎么说:

```
const morse = '-.-. --- -.. .'
// (1)
const morseToChar = {
  '-.-.': 'c',
  '-..': 'd',
  '.': 'e',
  '---': 'o',
}

const morseArray = morse.split(' ') // (2) [ '-.-.', '---', '-..', '.' ]
const textArray = morseArray.map((char) => morseToChar[char]) // (3) [ 'c', 'o', 'd', 'e' ]
const text = textArray.join(") // (4) 
```

下面是上面代码中发生的情况:

1.  创建一个对象文字来将莫尔斯字符映射到英语字母表
2.  莫尔斯电码被拆分成一个数组，用一个`' '`作为分隔符。(如果没有`' '`作为参数，你将得到一个数组，每个`.`和`-`都有单独的条目。)
3.  莫尔斯码数组被映射/转换成文本数组
4.  从数组中创建一个字符串，用`''`作为分隔符。(如果没有`''`作为参数，输出将是`c,o,d,e`。)

## 如何添加拆分限制

根据 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) ，也可以将一个`limit`作为参数传递给`split`。我从来不需要这样做，但是你可以这样应用它:

```
const publisher = 'free code camp'
publisher.split(' ', 1) // [ 'free' ] 
```

在上面的例子中，数组被限制为一个条目。如果没有它，数组的值将是`[ 'free', 'code', 'camp' ]`。

## 在你走之前…

谢谢你读到这里！我写的是我作为一名自学成才的软件开发人员的职业和教育经历，所以请随意查看我的网站或订阅 T2 的时事通讯。

您可能还喜欢:

*   [利用这些资源学习 JavaScript】](https://learnitmyway.com/learn-javascript-with-these-resources/)
*   [学习材料-软件开发](https://www.learnitmyway.com/2016/11/11/learning-material-software-development/)(从 CS 入门开始)