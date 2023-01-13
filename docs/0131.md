# 如何在 JavaScript 中将数字格式化为货币

> 原文：<https://www.freecodecamp.org/news/how-to-format-number-as-currency-in-javascript-one-line-of-code/>

当您处理来自 API 或外部资源的数据时，您将获得一些通用格式的数据。例如，如果您正在建立一个商店，您可能有像价格这样的数据。

此价格数据可能是一个通用数字，如 14340 或任何其他数字，如下面的数组所示:

```
const books = [
    {
        "id": 001,
        "name": "Clean Code",
        "price": 10.99,
    },
    {
        "id": 002,
        "name": "Introduction to Algorithms",
        "price": 1199,
    },
    {
        "id": 003,
        "name": "Programming Pearls",
        "price": 1.05,
    },
    {
        "id": 004,
        "name": "Program or Be Programmed",
        "price": 14340,
    }
] 
```

您不希望将数字直接传递到您的应用程序或网页中，因为读者和用户很难理解它们。

即使您添加了货币符号，也不能解决问题，因为您可能希望在正确的位置添加逗号和小数。您还希望每个价格输出都基于具有适当格式的货币。

例如，14340 可能是$14，340.00(美元)或₹14,340.00(卢比)或 14.340，00(欧元)等等，这取决于您定义的货币、地区和风格。您可以使用 JavaScript 中的`Intl.NumberFormat()`方法将这些数字转换成货币。

如果您有急事，下面是代码的基本示例:

```
const price = 14340;

// Format the price above to USD using the locale, style, and currency.
let USDollar = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
});

console.log(`The formated version of ${price} is ${USDollar.format(price)}`);
// The formated version of 14340 is $14,340.00 
```

在本文中，我将帮助您理解上述每个选项，它们的作用，以及如何正确使用这种方法将数字格式化为货币。

## 如何使用`Intl.NumberFormat()`构造函数将数字格式化为货币

您可以使用`Intl.NumberFormat()`构造函数来创建`Intl.NumberFormat`对象，这些对象支持区分语言的数字格式，比如货币格式。

这个构造函数接受两个主要参数，`locales`和`options`。它们都是可选的。

```
new Intl.NumberFormat(locales, options)

// Or

Intl.NumberFormat(locales, options) 
```

**注意**有没有`new`都可以调用`Intl.NumberFormat()`。两者都将创建一个新的`Intl.NumberFormat`实例。

当您使用`Intl.NumberFormat()`构造函数而不传递任何区域设置或选项时，它只会通过添加逗号来格式化数字。

```
const price = 14340;
console.log(new Intl.NumberFormat().format(price)); // 14,340 
```

如上所述，您不是在寻找常规的数字格式。您希望将这些数字格式化为货币——这样它会以正确的格式返回货币符号，而无需手动连接。

现在让我们来探究这两个参数。

### 第一个参数:语言环境

locale 是一个可选参数，可以作为字符串传递。它代表一个特定的地理、政治或文化区域。它只是根据区域设置格式化数字，而不是货币格式。

```
const price = 143450;

console.log(new Intl.NumberFormat('en-US').format(price)); // 143,450
console.log(new Intl.NumberFormat('en-IN').format(price)); // 1,43,450
console.log(new Intl.NumberFormat('en-DE').format(price)); // 143.450 
```

您会注意到，数字或价格现在根据地区进行了本地格式化。现在让我们研究一下 options 参数，将数字定制为一种货币。

### 第二个参数:选项(风格、货币……)

这是主要参数，您可以使用它来应用更多的格式，如货币格式。这是一个 JavaScript 对象，包含其他参数，如:

*   `style`:你用它来指定你想要的格式类型。这包括小数、货币和单位等值。对于本文，您将使用**货币**，因为这是您想要格式化数字的样式。
*   另一个选择是货币。您可以使用此选项指定您想要格式化的货币，例如`'USD'`、`'CAD'`、`'GBP``'`、`'INR'`等等。这也有助于根据地区在适当的位置提供符号。

```
// format number to US dollar
let USDollar = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
});

// format number to British pounds
let pounds = Intl.NumberFormat('en-GB', {
    style: 'currency',
    currency: 'GBP',
});

// format number to Indian rupee
let rupee = new Intl.NumberFormat('en-IN', {
    style: 'currency',
    currency: 'INR',
});

// format number to Euro
let euro = Intl.NumberFormat('en-DE', {
    style: 'currency',
    currency: 'EUR',
});

console.log('Dollars: ' + USDollar.format(price));
// Dollars: $143,450.00

console.log(`Pounds: ${pounds.format(price)}`);
// Pounds: £143,450.00

console.log('Rupees: ' + rupee.format(price));
// Rupees: ₹1,43,450.00

console.log(`Euro: ${euro.format(price)}`);
// Euro: €143,450.00 
```

还有一些选项您很可能永远不会使用或更改，比如`useGrouping`，它用于使用逗号(或句点，对于某些地区)对数字进行分组。这是一个布尔字段——默认情况下，它被设置为`true`。这就是为什么你的输出在本文中有一个逗号或句号(比如$143，450.00)。

当您将其值设置为`false`时，您会注意到不再有分组:

```
let euro = Intl.NumberFormat('en-DE', {
    style: 'currency',
    currency: 'EUR',
    useGrouping: false,
});

console.log(`Euro: ${euro.format(price)}`);
// Euro: €143450.00 
```

另一个选择是`maximumSignificantDigits`。您可以使用它根据您设置的有效位数对价格变量进行舍入。例如，当您将值设置为`3`时，`143,450.00`将变为`143,000`。

```
let pounds = Intl.NumberFormat('en-GB', {
    style: 'currency',
    currency: 'GBP',
    maximumSignificantDigits: 3,
});

console.log(`Pounds: ${pounds.format(price)}`);
// Pounds: £143,000 
```

## 给你！🚀

我希望这篇文章值得您花费时间。现在您知道了如何在不依赖任何外部库的情况下，用 JavaScript 将数字格式化为货币。

当使用 React、Vue 等库时，您可以将它作为一个实用函数，导入到您的任何组件中，并使用它而不是安装整个库(除非您需要更多的功能)。

祝编码愉快！