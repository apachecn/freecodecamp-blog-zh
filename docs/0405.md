# JavaScript 时间戳——如何使用 getTime()在 JS 中生成时间戳

> 原文：<https://www.freecodecamp.org/news/javascript-timestamp-how-to-use-gettime-to-generate-timestamps-in-js/>

在 JavaScript 中，时间戳通常与 Unix 时间相关联。并且有不同的方法来生成这样的时间戳。

当我们使用不同的 JavaScript 方法生成时间戳时，它们返回自 1970 年 1 月 1 日 UTC(Unix 时间)以来经过的毫秒数。

在本文中，您将了解如何使用以下方法在 JavaScript 中生成 Unix 时间戳:

*   `getTime()`法。
*   `Date.now()`法。
*   `valueOf()`法。

## 如何使用`getTime()`在 JS 中生成时间戳

```
var timestamp = new Date().getTime();

console.log(timestamp)
// 1660926192826
```

在上面的例子中，我们创建了一个`new Date()`对象，并将其存储在一个`timestamp`变量中。

我们还使用点符号`new Date().getTime()`将`getTime()`方法附加到了`new Date()`对象上。这会返回该点的 Unix 时间，单位为毫秒:1660926192826。

要获得以秒为单位的时间戳，需要将当前时间戳除以 1000。那就是:

```
var timestamp = new Date().getTime();

console.log(Math.floor(timestamp / 1000)) 
```

## 如何使用`Date.now()`在 JS 中生成时间戳

```
var timestamp = Date.now();

console.log(timestamp)
// 1660926758875
```

在上面的例子中，我们使用`Date.now()`方法获得了特定时间点的 Unix 时间戳。

您在这些示例中看到的时间戳将与您的不同。这是因为您将获得从 UTC 1970 年 1 月 1 日到您当前时间的时间戳。

## 如何使用`valueOf()`在 JS 中生成时间戳

```
var timestamp = new Date().valueOf();

console.log(timestamp)
// 1660928777955
```

就像`getTime()`方法一样，为了生成 Unix 时间戳，我们必须将`valueOf()`方法附加到一个`new Date()`对象上。

不带`getTime()`或`valueOf()`的`new Date()`对象返回关于你当前时间的信息。

## 摘要

在文章中，我们讨论了 JavaScript 中的时间戳。通常有与 Unix 相关的时间。

我们看到了三种不同的方法，可以用代码示例在 JavaScript 中生成时间戳。

编码快乐！