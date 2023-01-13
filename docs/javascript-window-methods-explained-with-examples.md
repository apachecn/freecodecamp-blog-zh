# 用例子解释 JavaScript 窗口方法

> 原文：<https://www.freecodecamp.org/news/javascript-window-methods-explained-with-examples/>

## **窗口定位方法**

对象可以用来获取当前页面地址(URL)的信息，并将浏览器重定向到一个新页面。

可以不使用`window`前缀来编写`window.location`对象，就像使用`location`一样。

## 一些例子:

*   `window.location.href`返回当前页面的 href (URL)
*   `window.location.hostname`返回网络主机的域名
*   `window.location.host`返回主机名和任何关联的端口
*   `window.location.pathname`返回当前页面的路径和文件名
*   `window.location.protocol`返回使用的 web 协议(http:或 https:)
*   `window.location.assign()`载入新文件

### 更多信息:

[MDN](https://developer.mozilla.org/docs/Web/API/Window/location)

## **窗口 setInterval 方法**

`setInterval()`方法以指定的时间间隔(以毫秒为单位)调用一个函数或计算一个表达式。

```
setInterval(function(){ 
  alert("Hello");
}, 3000); 
```

`setInterval()`方法将继续调用该函数，直到`clearInterval()`被调用，或者窗口被关闭。

`setInterval()`方法可以向函数传递额外的参数，如下例所示。

```
setInterval(function, milliseconds, parameter1, parameter2, parameter3); 
```

由`setInterval()`返回的 ID 值被用作`clearInterval()`方法的参数。

小贴士:

*   1000 毫秒= 1 秒。
*   要在指定的毫秒数后只执行一次函数，请使用`setTimeout()`方法。

## 窗口设置超时方法

`setTimeout()`方法以毫秒为单位设置一个计时器，然后在计时器超时时调用一个函数或计算一个表达式。

注意事项:

*   `setTimeout()`使用毫秒，1000 毫秒等于 1 秒
*   该方法只执行一次传递给它的函数或表达式。如果需要多次重复执行，使用`setInterval()`方法
*   要停止传递给它的函数或表达式，使用`clearTimeout()`方法

`setTimout()`方法的语法如下:

```
setTimeout(function, milliseconds, param1, param2, ...);
```

例如:

```
setTimeout(function() { 
  alert("Hello");
}, 3000);
```

关于`setTimeout()`需要记住的一件非常重要的事情是，它是异步执行的:

```
console.log("A");
setTimeout(function() { console.log("B"); }, 0);
console.log("C");

// The order in the console will be
// A
// C
// B
```

控制台日志的顺序可能与您预期的不同。要解决这个问题并确保代码同步执行，只需在函数中嵌套最后一个`console.log`语句:

```
console.log("A");
setTimeout(function() {
    console.log("B");
    console.log("C");
}, 0);

// The order in the console will be
// A
// B
// C
```

## **窗口 clearTimeout 方法**

`clearTimeout()`方法用于停止用`setTimeout()`方法设置的定时器。

```
 clearTimeout(setTimeout_ID); 
```

为了能够使用`clearTimeout()`方法，你必须使用一个全局变量。参见下面的例子

`clearTimeout()`方法通过使用由`setTimeout()`返回的 id 来工作。因此，使用一个全局变量来存储`setTimeout()`通常是个好主意，然后在必要时清除它:

```
const myTimeout = setTimeout(function, milliseconds);

...

// Later, to clear the timeout
clearTimeout(myTimeout);
```

## **窗口间隙法**

`clearInterval()`方法用于清除用`setInterval()`方法设置的定时器。

```
clearInterval(setInteval_ID); 
```

`clearTimeout()`方法通过使用由`setInterval()`返回的 id 来工作。因此，使用一个全局变量来存储`setInterval()`通常是个好主意，然后在必要时清除它:

```
const myInterval = setInterval(function, milliseconds);

...

// Later, to clear the timeout
clearInterval(myInterval);
```

## 窗口本地存储方法

为您的 web 应用程序提供了一种在用户浏览器中本地存储键/值对的方法。

在 HTML5 和`localStorage`之前，web app 数据必须存储在 cookies 中。每个 HTTP 请求都包含 cookies，这曾经是在客户端设备上本地存储应用程序数据的合法方法。然而，许多相同的数据是用 cookies 传输的，由于它们被限制在 4 KB 左右，所以很难存储应用程序需要的所有内容。

`localStorage`的存储限制是每个域 10 MB 的数据，这被认为是更有效的，因为存储在其中的信息不会随着每个请求传输到服务器。

### **网络存储的类型**

`localStorage`是浏览器在本地存储数据的两种现代方法之一:

*   `localStorage`:存储没有截止日期的数据。甚至当用户的浏览器关闭并重新打开时，`localStorage`中的数据仍然存在。
*   `sessionStorage`:类似于`localStorage`，除了它只存储一个会话的数据。一旦用户关闭浏览器，这些数据就会被删除。

### **HTML5 本地存储方法**

附带了一些不同的 JavaScript 方法，使得它非常容易使用。

要设置数据:

```
localStorage.setItem('Name', 'somevalue');
```

要从存储中检索一些数据:

```
localStorage.getItem('Name');
```

要移除或删除某些数据:

```
localStorage.removeItem('Name');
```

要清除仓库中的所有物品(不仅仅是单个物品):

```
localStorage.clear();
```

要获取存储中的属性数:

```
localStorage.length;
```

注意:上述所有方法也适用于`sessionStorage`。简单地用`sessionStorage`代替`localStorage`。

## 窗口打开方法

Window `open()`方法用于打开新的浏览器窗口或标签，具体取决于参数和用户的浏览器设置。这种方法通常用于弹出窗口，在许多现代浏览器中默认情况下被阻止。

`open()`方法的语法是:

```
const window =  window.open(url, windowName, windowFeatures);
```

### 因素

*   `url`:要加载的资源的字符串。
*   `windowName`:表示新窗口或标签的目标名称的字符串。请注意，这不会用作新窗口/选项卡的标题。
*   `windowFeatures`:可选的逗号分隔的特性字符串列表，如新窗口的大小、位置、是否显示菜单栏等等。

### 例子

```
let windowObjectReference;
const strWindowFeatures = "menubar=yes,location=yes,resizable=yes,scrollbars=yes,status=yes";

function openRequestedPopup() {
  windowObjectReference = window.open("https://www.freecodecamp.org/", "fCC_WindowName", strWindowFeatures);
}

openRequestedPopup();
```

上述代码将尝试打开一个 freeCodeCamp 登录页面的弹出窗口。

## **窗口确认方法**

您可以使用`confirm`方法让用户仔细检查网页上的决定。当您调用此方法时，浏览器将显示一个对话框，其中有两个选项，分别是“确定”和“取消”

例如，假设某人刚刚点击了删除按钮。您可以运行以下代码:

```
if (window.confirm("Are you sure you want to delete this item?")) {
  // Delete the item
}
```

消息“您确定要删除此项目吗？”将出现在对话框中。如果用户点击 OK，confirm 方法将返回`true`，浏览器将运行 If 语句中的代码。如果他或她单击 Cancel，该方法将返回`false`,其他什么都不会发生。这为防止有人意外点击删除提供了一些保护。