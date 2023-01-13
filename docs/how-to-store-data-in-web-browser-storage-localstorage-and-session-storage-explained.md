# 如何在 Web 浏览器存储中存储数据-本地存储和会话存储解释

> 原文：<https://www.freecodecamp.org/news/how-to-store-data-in-web-browser-storage-localstorage-and-session-storage-explained/>

为了管理 web 应用程序处理的数据，您不一定需要数据库。Chrome(版本 4 及更高版本)、Mozilla Firefox(版本 3.5 及更高版本)和 Internet Explorer(版本 8 及更高版本)以及一系列其他浏览器(包括 iOS 和 Android 的浏览器)都支持各自的浏览器存储功能。

浏览器存储主要有两种可能性:localStorage 和 sessionStorage。

## **本地存储**

浏览器重启(关闭并再次打开)后，保存到`localStorage`对象的任何内容/数据都将可用。为了将 *****保存为***** 到`localStorage`，可以使用`setItem()`的方法。这个方法必须有一个键和一个值。

```
Example: localStorage.setItem("mykey","myvalue");
```

要 *****从本地存储***** 中取出项目，必须使用方法`getItem`。必须将您想要检索的数据的密钥传递给`getItem`方法:

```
 Example: localStorage.getItem("mykey");
```

您可以使用`removeItem()`方法从`localStorage`中移除一个项目。此方法必须传递要移除的项目的密钥:

```
 Example: localStorage.removeItem("mykey");
```

要清除整个`localStorage`，你应该在`localStorage`对象上使用`clear()`方法:

```
 Example: localStorage.clear();
```

## **会话存储**

保存在`sessionStorage`对象中的项目将一直保留，直到用户关闭浏览器。然后，存储将被清除。

您可以将一个项目保存到`sessionStorage`，请在`sessionStorage`对象上使用`setItem()`方法:

```
Example: sessionStorage.setItem("mykey","myvalue");
```

要让 *****从 sessionStorage***** 中检索项目，必须使用方法`getItem`。必须将您想要检索的数据的密钥传递给`getItem`方法:

```
 Example: sessionStorage.getItem("mykey");
```

您可以使用`removeItem()`方法从`sessionStorage`中移除一个项目。此方法必须传递要移除的项目的密钥:

```
 Example: sessionStorage.removeItem("mykey");
```

要清除整个`sessionStorage`，你应该在`sessionStorage`对象上使用`clear()`方法:

```
 Example: sessionStorage.clear();
```

## **将数组保存到本地存储和会话存储**

您不能只保存单个值到`localStorage`和`sessionStorage`，但是您也可以保存数组的内容。

在本例中，我们有一个数字数组:

```
var ourArray =[1,2,3,4,5];
```

我们现在可以使用`setItem()`方法将其保存到`localStorage`或`sessionStorage`:

```
localStorage.setItem("ourarraykey",JSON.stringify(ourArray));
```

或者，对于`sessionStorage`:

```
sessionStorage.setItem("ourarraykey",JSON.stringify(ourArray));
```

为了保存，必须首先将数组转换为字符串。在上面的例子中，我们使用了`JSON.stringify`方法来完成这个任务。

从`localStorage`或`sessionStorage`中检索数据时，将其转换回数组:

```
var storedArray = localStorage.getItem("ourarraykey");
ourArray = JSON.parse(storedArray);
```

或者，对于`sessionStorage`:

```
var storedArray = sessionStorage.getItem("ourarraykey");
ourArray = JSON.parse(storedArray);
```