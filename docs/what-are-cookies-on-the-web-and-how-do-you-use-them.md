# 网络上的 Cookies 是什么，你如何使用它们？

> 原文：<https://www.freecodecamp.org/news/what-are-cookies-on-the-web-and-how-do-you-use-them/>

## **操作饼干**

获取或设置 cookie 是一个简单的操作，可以通过访问浏览器的 document 对象上的 cookie 属性来实现。

比方说，你找到了一个惊人的、信息丰富的食谱网站，为你的客人做一顿有趣的饭，但它是用外语写的。幸运的是，你可以使用下拉菜单来改变网站的语言。

几天后，你再次访问同一个网站，为你的母亲做一道菜，但现在你默认看到的是你母语的网站。

怎么会这样？网站会记住你上次访问时选择的语言，并以 ****cookie**** 的形式存储下来。现在，它会通过读取 cookie 自动选择您喜欢的语言。

`userLanguage:french`

Cookies 用于在客户端以`name:value`对的形式存储数据。他们让网站在浏览器上存储用户的具体信息以备后用。存储的信息可以是`sessionID`、`userCountry`、`visitorLanguage`等等。

在客户端存储数据的另一种方式是`localstorage`。

### **设置 Cookie**

可以使用下面的语法设置 cookie。但是像最后提到的那种库是强烈推荐的，可以让每个人的开发都更容易。

在设置 cookie 时，您也可以设置它的截止日期。如果您跳过这一步，浏览器关闭时 cookies 会被删除。

****记住**由特定域设置的 **cookie 只能由该域&的子域读取。****

```
// Using vanilla javascript
document.cookie = 'userLanguage=french; expires=Sun, 2 Dec 2017 23:56:11 UTC; path=/';

//Using JS cookie library
Cookies.set('userLanguage', 'french', { expires: 7, path: '/' });
```

上述 cookie 将在 7 天后过期。

### **获取 Cookie**

```
// Using vanilla javascript
console.log(document.cookie)

// => "_ga=GA1.2.1266762736.1473341790; userLanguage=french"

// Using JS cookie library
Cookies.get('userLanguage');

// => "french"
```

### **删除 Cookie**

要删除 cookie，请将到期日期设置为过去的某个时间。

```
// Using vanilla javascript
document.cookie = 'userLanguage; expires=Thu, 01 Jan 1970 00:00:01 GMT; path=/';

//Using JS cookie library
Cookies.remove('userLanguage');
```

如果你发现自己在项目中经常使用 Cookie，请使用像这样的库 [JS Cookie](https://github.com/js-cookie/js-cookie) ，为自己节省大量时间。