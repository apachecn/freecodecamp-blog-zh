# JavaScript URL 编码示例–如何使用 encodeURIcomponent()和 encodeURI()

> 原文：<https://www.freecodecamp.org/news/javascript-url-encode-example-how-to-use-encodeuricomponent-and-encodeuri/>

你可能认为`encodeURI`和`encodeURIComponent`做同样的事情，至少从它们的名字来看是这样。您可能会不知道何时使用哪一个。

在这篇文章中，我将揭开`encodeURI`和`encodeURIComponent`的区别。

### 什么是 URI，它与 URL 有什么不同？

**URI** 代表统一资源标识符。
**URL** 代表统一资源定位符。

唯一标识资源的是它的 URI，如 id、名称或 ISBN 号。URL 指定资源以及如何访问它(协议)。所有的网址都是 URIs 的，但不是所有的 URIs 的网址。

### 为什么我们需要编码？

URL 只能包含标准 128 字符 ASCII 集中的某些字符。不属于此集合的保留字符必须进行编码。

这意味着我们需要在传递到 URL 时对这些字符进行编码。在 url 中输入特殊字符时，如`&`、`space`、`!`，需要进行转义，否则可能会导致不可预知的情况。

使用案例:

1.  用户在表单中提交了可能是字符串格式的值，需要传入，如 URL 字段。
2.  需要接受查询字符串参数才能发出 GET 请求。

### encodeURI 和 encodeURIComponent 有什么区别？

`encodeURI`和`encodeURIComponent`用于通过用一个、两个、三个或四个表示字符的 UTF-8 编码的转义序列替换某些字符来编码统一资源标识符(URIs)。

`encodeURIComponent`应该被用来编码一个**URI**组件——一个应该是 URL 的一部分的字符串。

`encodeURI`应该用来编码一个 **URI** 或者一个现有的 URL。

这里有一个关于字符编码差异的表格

### 哪些字符被编码？

`encodeURI()`不会编码:`~!@#$&*()=:/,;?+'`

`encodeURIComponent()`不会编码:`~!*()'`

字符`A-Z a-z 0-9 - _ . ! ~ * ' ( )`没有被转义。

### 例子

```
const url = 'https://www.twitter.com'

console.log(encodeURI(url))             //https://www.twitter.com
console.log(encodeURIComponent(url))    //https%3A%2F%2Fwww.twitter.com

const paramComponent = '?q=search'
console.log(encodeURIComponent(paramComponent)) //"%3Fq%3Dsearch"
console.log(url + encodeURIComponent(paramComponent)) //https://www.twitter.com%3Fq%3Dsearch 
```

### 何时编码

1.  当接受可能有空格的输入时。

    ```
    encodeURI("http://www.mysite.com/a file with spaces.html") //http://www.mysite.com/a%20file%20with%20spaces.html 
    ```

2.  当从查询字符串参数构建 URL 时。

    ```
     let param = encodeURIComponent('mango')
     let url = "http://mysite.com/?search=" + param + "&length=99"; //http://mysite.com/?search=mango&length=99 
    ```

3.  当接受可能有保留字符的查询参数时。

```
 let params = encodeURIComponent('mango & pineapple')
   let url = "http://mysite.com/?search=" + params; //http://mysite.com/?search=mango%20%26%20pineapple 
```

### 摘要

如果你有一个完整的网址，使用`encodeURI`。但是如果你有一个 URL 的一部分，使用`encodeURIComponent`。

* * *

对我的更多教程和 JSBytes 感兴趣？注册订阅我的时事通讯。或[在推特上关注我](https://twitter.com/shrutikapoor08)