# 每个 Web 开发人员都应该知道的 10 个 JavaScript 技巧

> 原文：<https://www.freecodecamp.org/news/javascript-hacks/>

如果您使用这些技巧优化您的 JavaScript 代码，它可以帮助您编写更干净的代码，节省资源，并优化您的编程时间。

据 [RedMonk](https://redmonk.com/sogrady/2020/07/27/language-rankings-6-20/) 介绍，JavaScript 是最流行的编程语言。此外，SlashData 估计大约有 1240 万开发者使用 JavaScript，其中也包括 CoffeeScript 和微软的 TypeScript。

这意味着数百万人使用 JavaScript 作为程序员工作，通过像 [UpWork](https://www.upwork.com/freelance-jobs/javascript/) 和[自由职业者](https://www.freelancer.com/jobs/javascript/)这样的网站从事自由职业，或者甚至开始他们自己的[网络开发业务](https://websitesetup.org/how-to-get-web-design-clients/)。

freeCodeCamp 有一个关于 JavaScript 的极好的[基础课程](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/basic-javascript/)。但是，如果您已经熟悉了基础知识，并且想要提高您的 JavaScript 熟练程度，那么这里有十个您应该学习并整合到您的工作流程中的技巧。

## 1.如何使用条件句的快捷方式

JavaScript 允许你使用某些快捷键来使你的代码看起来更容易。在一些简单的情况下，你可以使用逻辑运算符`&&`和`||`来代替`if`和`else`。

让我们看看操作中的`&&`操作符。

示例片段:

```
// instead of
if (accessible) {
  console.log("It’s open!");
}

// use
accessible && console.log("It’s open!");
```

`||`操作符起着“或”子句的作用。现在，使用这个操作符有点棘手，因为它会阻止应用程序执行。但是，我们可以添加一个条件来绕过它。

示例片段:

```
// instead of
if (price.data) {
  return price.data;
} else {
  return 'Getting the price’';
}

// use
return (price.data || 'Getting the price');
```

## 2.如何使用~~运算符转换成最大整数

使用`Math.floor`返回小于或等于等式中给定数字的最大整数会占用资源，更不用说要记住这是一个相当长的字符串。更有效的方法是使用`~~`操作符

这里有一个例子:

```
// instead of
Math.floor(Math.random() * 50);

// use
~~(Math.random() * 50);

// You can also use the ~~ operator to convert anything into a number value.
// Example snippet:
~~('whitedress') // returns 0
~~(NaN) // returns 0
```

## 3.使用 array.length 调整数组大小或清空数组

有时你需要调整数组的大小或者清空它。最有效的方法是使用`Array.length`。

示例片段:

```
let array = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j'];

console.log(array.length); // returns the length as 10

array.length = 4;

console.log(array.length); // returns the length as 4
console.log(array); // returns ['a', 'b', 'c', 'd']
```

你也可以使用`Array.length`从一个指定的数组中删除所有的值。

示例片段:

```
let array = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j'];

array.length = 0;

console.log(array.length); // returns the length as 0
console.log(array); // returns []
```

## 4.如何在不导致服务器过载的情况下合并数组

合并数组时，可能会对服务器造成严重的压力，尤其是在处理大型数据集时。为了简单起见并保持性能水平，使用`Array.concat()`和`Array.push.apply(arr1, arr2)`函数。

使用这些函数取决于数组的大小。

让我们看看如何处理更小的数组。

示例片段:

```
let list1 = ['a', 'b', 'c', 'd', 'e'];
let list2 = ['f', 'g', 'h', 'i', 'j'];

console.log(list1.concat(list2)); // returns the merged values of both arrays, ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
```

当对较大的数据集使用`Array.concat()`函数时，在创建新数组时会消耗大量内存。为了避免性能下降，使用`Array.push.apply(arr1, arr2)`,它将第二个阵列合并到第一个阵列中，而不创建新的阵列。

示例片段:

```
let list1 = ['a', 'b', 'c', 'd', 'e'];
let list2 = ['f', 'g', 'h', 'i', 'j'];

console.log(list1.push.apply(list1, list2)); // returns 10, the new length of list1
console.log(list1); // returns ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
```

## 5.如何对数组使用过滤器

当您处理多列对应数据时，筛选数组非常有用。在这种情况下，您可以根据描述阵列中组的任何特征来包括和排除数据。

要过滤一个数组，使用`filter()`函数。

示例片段:

```
const cars = [
  { make: 'Opel', class: 'Regular' },
  { make: 'Bugatti', class: 'Supercar' },
  { make: 'Ferrari', class: 'Supercar' },
  { make: 'Ford', class: 'Regular' },
  { make: 'Honda', class: 'Regular' },
]
const supercar = cars.filter(car => car.class === 'Supercar');
console.table(supercar); // returns the supercar class data in a table format
```

您也可以将`filter()`与`Boolean`一起使用，从您的数组中删除所有的`null`或`undefined`值。

示例片段:

```
const cars = [
  { make: 'Opel', class: 'Regular' },
  null,
  undefined
]

cars.filter(Boolean); // returns [{ make: 'Opel', class: 'Regular' }] 
```

## 6.如何提取唯一值

假设您有一个重复值的数据集，并且您需要从这个数据集中产生唯一的值。您可以结合使用扩展语法`...`和对象类型`Set`来实现这一点。这种方法对单词和数字都有效。

示例片段:

```
const cars = ['Opel', 'Bugatti', 'Opel', 'Ferrari', 'Ferrari', 'Opel'];
const unrepeated_cars = [...new Set(cars)];

console.log(unrepeated_cars); // returns the values Opel, Bugatti, Ferrari
```

## 7.如何使用替换功能快捷方式

你应该熟悉`String.replace()`函数。但是，它只会用指定的行替换一个字符串一次，并从此处停止。假设你有一个更大的数据集，你需要多次输入这个函数。过了一段时间就会变得令人沮丧。

为了让您的生活更简单，您可以在正则表达式的末尾添加`/g`，这样函数就可以运行并替换所有匹配条件，而不会在第一个条件处停止。

示例片段:

```
const grammar = 'synonym synonym';

console.log(grammar.replace(/syno/, 'anto')); // this returns 'antonym synonym'
console.log(grammar.replace(/syno/g, 'anto')); // this returns 'antonym antonym'
```

## 8.如何缓存值

当您处理大型数组时，需要使用`getElementById()`通过 ID 请求元素，或者使用`getElementsByClassName()`通过类名请求元素，那么 JavaScript 会在一个循环中遍历数组，处理每个相似的元素请求。这可能会占用大量资源。

但是，如果您知道自己经常使用指定的选择，可以通过缓存一个值来提高性能。

示例片段:

```
const carSerial = serials.getElementById('serial1234');
carSerial.addClass('cached-serial1234');
```

## 9.如何检查对象是否有值

当您处理多个对象时，很难跟踪哪些对象包含实际值，哪些对象可以删除。

这里有一个关于如何用`Object.keys()`函数检查对象是否为空或有值的快速技巧。

示例片段:

```
Object.keys(objectName).length // if it returns 0 then the Object is empty, otherwise it displays the number of values
```

## 10.如何缩小你的 JavaScript 文件

大型 JS 文件会影响页面的加载和响应性能。在编写代码时，您可能会留下不必要的行、注释和死代码。根据文件的大小，它可能会堆积起来，成为一个多余的瓶颈。

有几个工具可以帮助你清理代码，使 JS 文件更简洁——或者用开发人员的话来说，缩小它们。尽管缩小 JS 文件本身并不是一种黑客行为，但是开发人员了解和实现它仍然是有益的。

一个是 [Google Closure 编译器](https://developers.google.com/closure/compiler)，它解析你的 JavaScript，分析它，删除死代码，重写并最小化剩下的部分。另一个是[微软 Ajax Minifier](https://archive.codeplex.com/?p=ajaxmin) ，它能让你通过减少 JavaScript 文件的大小来提高你的网络应用程序的性能。

这就是了。使用这十个技巧使你的代码更整洁，节省服务器资源，并保留编程时间。

### 感谢阅读！

我是一名作家，对数字营销、网络开发和网络安全充满热情。你可以通过 [LinkedIn 这里](https://www.linkedin.com/in/gert-svaiko/)联系到我。

你可能也会喜欢我写的其他一些文章:

*   [谷歌页面体验:2021 年你需要知道的五个步骤](https://www.searchenginewatch.com/2020/12/01/the-google-page-experience-what-you-need-to-know-and-five-steps-for-2021/)
*   [这一季需要警惕的虚拟主机安全威胁](https://www.infosecurity-magazine.com/next-gen-infosec/web-hosting-threats-season/)
*   [如何在 2020 年余下的不寻常的假日季节提升销量](https://www.digitalcommerce360.com/2020/12/08/how-to-boost-sales-during-the-rest-of-2020s-unusual-holiday-season/)