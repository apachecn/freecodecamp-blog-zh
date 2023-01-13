# 用 Reduce 制作的 10 个 JavaScript 实用函数

> 原文：<https://www.freecodecamp.org/news/10-js-util-functions-in-reduce/>

多工具再次出击。

在我的上一篇文章中，我给了你一个挑战，用`reduce`重新创建众所周知的函数。本文将向您展示其中一些是如何实现的，以及一些额外的东西！

总的来说，我们要看十个效用函数。它们在您的项目中非常方便，最棒的是，它们是使用`reduce`实现的！我从 [RamdaJS 库](https://ramdajs.com/)为这一个汲取了很多灵感，所以检查一下！

## 1.一些

### 因素

1.  `predicate` -返回`true`或`false`的函数。
2.  `array` -测试项目清单。

### 描述

如果`predicate`为*的任一*项返回`true`，`some`返回`true`。否则它返回`false`。

### 履行

```
const some = (predicate, array) =>
  array.reduce((acc, value) => acc || predicate(value), false); 
```

### 使用

```
const equals3 = (x) => x === 3;

some(equals3, [3]); // true
some(equals3, [3, 3, 3]); // true
some(equals3, [1, 2, 3]); // true
some(equals3, [2]); // false 
```

## 2.全部

### 因素

1.  `predicate` -返回`true`或`false`的函数。
2.  `array` -测试项目清单。

### 描述

如果`predicate`每项为*返回`true`，`all`返回`true`。否则它返回`false`。*

### 履行

```
const all = (predicate, array) =>
  array.reduce((acc, value) => acc && predicate(value), true); 
```

### 使用

```
const equals3 = (x) => x === 3;

all(equals3, [3]); // true
all(equals3, [3, 3, 3]); // true
all(equals3, [1, 2, 3]); // false
all(equals3, [3, 2, 3]; // false 
```

## 3.没有人

### 因素

1.  `predicate` -返回`true`或`false`的函数。
2.  `array` -测试项目清单。

### 描述

如果`predicate`每项为*返回`false`，`none`返回`true`。否则它返回`false`。*

### 履行

```
const none = (predicate, array) =>
  array.reduce((acc, value) => !acc && !predicate(value), false); 
```

### 使用

```
const isEven = (x) => x % 2 === 0;

none(isEven, [1, 3, 5]); // true
none(isEven, [1, 3, 4]); // false
none(equals3, [1, 2, 4]); // true
none(equals3, [1, 2, 3]); // false 
```

## 4.地图

### 因素

1.  `transformFunction` -在每个元素上运行的函数。
2.  `array` -要转换的项目列表。

### 描述

返回一个新的项目数组，每个项目都根据给定的`transformFunction`进行转换。

### 履行

```
const map = (transformFunction, array) =>
  array.reduce((newArray, item) => {
    newArray.push(transformFunction(item));

    return newArray;
  }, []); 
```

### 使用

```
const double = (x) => x * 2;
const reverseString = (string) =>
  string
    .split('')
    .reverse()
    .join('');

map(double, [100, 200, 300]);
// [200, 400, 600]

map(reverseString, ['Hello World', 'I love map']);
// ['dlroW olleH', 'pam evol I'] 
```

## 5.过滤器

### 因素

1.  `predicate` -返回`true`或`false`的函数。
2.  `array` -要过滤的项目列表。

### 描述

返回一个新数组。如果`predicate`返回`true`，则该项被添加到新数组中。否则，该项将从新数组中排除。

### 履行

```
const filter = (predicate, array) =>
  array.reduce((newArray, item) => {
    if (predicate(item) === true) {
      newArray.push(item);
    }

    return newArray;
  }, []); 
```

### 使用

```
const isEven = (x) => x % 2 === 0;

filter(isEven, [1, 2, 3]);
// [2]

filter(equals3, [1, 2, 3, 4, 3]);
// [3, 3] 
```

## 6.拒绝

### 因素

1.  `predicate` -返回`true`或`false`的函数。
2.  `array` -要过滤的项目列表。

### 描述

就像`filter`一样，只是行为方式相反。

如果`predicate`返回`false`，则该项被添加到新数组中。否则，该项将从新数组中排除。

### 履行

```
const reject = (predicate, array) =>
  array.reduce((newArray, item) => {
    if (predicate(item) === false) {
      newArray.push(item);
    }

    return newArray;
  }, []); 
```

### 使用

```
const isEven = (x) => x % 2 === 0;

reject(isEven, [1, 2, 3]);
// [1, 3]

reject(equals3, [1, 2, 3, 4, 3]);
// [1, 2, 4] 
```

## 7.发现

### 因素

1.  `predicate` -返回`true`或`false`的函数。
2.  `array` -要搜索的项目列表。

### 描述

返回与给定的`predicate`匹配的第一个元素。如果没有匹配的元素，那么返回`undefined`。

### 履行

```
const find = (predicate, array) =>
  array.reduce((result, item) => {
    if (result !== undefined) {
      return result;
    }

    if (predicate(item) === true) {
      return item;
    }

    return undefined;
  }, undefined); 
```

### 使用

```
const isEven = (x) => x % 2 === 0;

find(isEven, []); // undefined
find(isEven, [1, 2, 3]); // 2
find(isEven, [1, 3, 5]); // undefined
find(equals3, [1, 2, 3, 4, 3]); // 3
find(equals3, [1, 2, 4]); // undefined 
```

## 8.划分

### 因素

1.  `predicate` -返回`true`或`false`的函数。
2.  `array` -项目清单。

### 描述

基于`predicate`“分区”或将数组一分为二。如果`predicate`返回`true`，则该项目进入列表 1。否则，该项目将进入列表 2。

### 履行

```
const partition = (predicate, array) =>
  array.reduce(
    (result, item) => {
      const [list1, list2] = result;

      if (predicate(item) === true) {
        list1.push(item);
      } else {
        list2.push(item);
      }

      return result;
    },
    [[], []]
  ); 
```

### 使用

```
const isEven = (x) => x % 2 === 0;

partition(isEven, [1, 2, 3]);
// [[2], [1, 3]]

partition(isEven, [1, 3, 5]);
// [[], [1, 3, 5]]

partition(equals3, [1, 2, 3, 4, 3]);
// [[3, 3], [1, 2, 4]]

partition(equals3, [1, 2, 4]);
// [[], [1, 2, 4]] 
```

## 9.勇气

### 因素

1.  `key` -从对象中选取的键名
2.  `array` -项目清单。

### 描述

从数组中的每个项目中取出给定的`key`。返回这些值的新数组。

### 履行

```
const pluck = (key, array) =>
  array.reduce((values, current) => {
    values.push(current[key]);

    return values;
  }, []); 
```

### 使用

```
pluck('name', [{ name: 'Batman' }, { name: 'Robin' }, { name: 'Joker' }]);
// ['Batman', 'Robin', 'Joker']

pluck(0, [[1, 2, 3], [4, 5, 6], [7, 8, 9]]);
// [1, 4, 7] 
```

## 10.扫描

### 因素

1.  `reducer` -接收两个参数的标准缩减函数-累加器和来自数组的电流元素。
2.  `initialValue` -累加器的初始值。
3.  `array` -项目清单。

### 描述

就像`reduce`一样工作，但是它只返回一个结果，在返回这个结果的过程中，它返回一个每个减少的值的列表。

### 履行

```
const scan = (reducer, initialValue, array) => {
  const reducedValues = [];

  array.reduce((acc, current) => {
    const newAcc = reducer(acc, current);

    reducedValues.push(newAcc);

    return newAcc;
  }, initialValue);

  return reducedValues;
}; 
```

### 使用

```
const add = (x, y) => x + y;
const multiply = (x, y) => x * y;

scan(add, 0, [1, 2, 3, 4, 5, 6]);
// [1, 3, 6, 10, 15, 21] - Every number added from 1-6

scan(multiply, 1, [1, 2, 3, 4, 5, 6]);
// [1, 2, 6, 24, 120, 720] - Every number multiplied from 1-6 
```

## 想要免费辅导？

如果你想安排一个免费电话来讨论关于代码、面试、职业或任何其他方面的前端开发问题[请在 Twitter 上关注我，并给我发短信](https://twitter.com/yazeedBee)。

之后，如果你喜欢我们的第一次会议，我们可以讨论一个持续的辅导，以帮助你达到你的前端发展目标！

## 感谢阅读

更多类似的内容，请查看[https://yazeedb.com！](https://yazeedb.com)

下次见！