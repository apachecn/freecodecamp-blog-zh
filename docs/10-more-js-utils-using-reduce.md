# Reduce 提供了 10 多种实用功能

> 原文：<https://www.freecodecamp.org/news/10-more-js-utils-using-reduce/>

这一次，使用测试套件！

在之前的中，我写了大约 10 个用 JavaScript 的 [reduce 函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)实现的实用函数。它很受欢迎，我带着对这个宏伟的多功能工具更深的欣赏离开了。为什么不多试 10 次呢？

其中很多都是受了令人敬畏的图书馆 [Lodash](https://lodash.com/) 和 [Ramda](https://ramdajs.com/) 的启发！我还编写了单元测试来确保正确的行为。你可以在 [Github repo 这里](https://github.com/yazeedb/js-utils-using-reduce)看到一切。

## 1.管

### 因素

1.  `...functions` -任意数量的功能。

### 描述

执行*从左到右*的功能组合。管道的第一个参数充当初始值，并在通过每个函数时进行转换。

### 履行

```
const pipe = (...functions) => (initialValue) =>
  functions.reduce((value, fn) => fn(value), initialValue); 
```

### 使用

```
const mathSequence = pipe(
    (x) => x * 2,
    (x) => x - 1,
    (x) => x * 3
  );

mathSequence(1); // 3
mathSequence(2); // 9
mathSequence(3); // 15 
```

关于更多细节，我在这里写了一篇关于 pipe 的文章。

## 2.构成

### 因素

1.  `...functions` -任意数量的功能。

### 描述

执行*从右向左*功能组合。管道的第一个参数充当初始值，并在通过每个函数时进行转换。

工作原理类似`pipe`，但方向相反。

### 履行

```
const compose = (...functions) => (initialValue) =>
  functions.reduceRight((value, fn) => fn(value), initialValue); 
```

### 使用

```
const mathSequence = compose(
    (x) => x * 2,
    (x) => x - 1,
    (x) => x * 3
  );

mathSequence(1); // 4
mathSequence(2); // 10
mathSequence(3); // 16 
```

关于更多细节，我在这里写了一篇关于 compose 的文章。

## 3.活力

### 因素

1.  `list1` -项目清单。
2.  `list2` -项目清单。

### 描述

通过索引将两个列表中的项目配对。如果列表长度不相等，则使用较短的列表长度。

### 履行

```
const zip = (list1, list2) => {
  const sourceList = list1.length > list2.length ? list2 : list1;

  return sourceList.reduce((acc, _, index) => {
    const value1 = list1[index];
    const value2 = list2[index];

    acc.push([value1, value2]);

    return acc;
  }, []);
}; 
```

### 使用

```
zip([1, 3], [2, 4]); // [[1, 2], [3, 4]]

zip([1, 3, 5], [2, 4]); // [[1, 2], [3, 4]]

zip([1, 3], [2, 4, 5]); // [[1, 2], [3, 4]]

zip(['Decode', 'secret'], ['this', 'message!']);
// [['Decode', 'this'], ['secret', 'message!']] 
```

## 4.散布

### 因素

1.  `separator` -要插入的项目。
2.  `list` -项目清单。

### 描述

在列表的每个元素之间插入分隔符。

### 履行

```
const intersperse = (separator, list) =>
  list.reduce((acc, value, index) => {
    if (index === list.length - 1) {
      acc.push(value);
    } else {
      acc.push(value, separator);
    }

    return acc;
  }, []); 
```

### 使用

```
intersperse('Batman', [1, 2, 3, 4, 5, 6]);
// [1, 'Batman', 2, 'Batman', 3, 'Batman', 4, 'Batman', 5, 'Batman', 6]

intersperse('Batman', []);
// [] 
```

## 5.插入

### 因素

1.  `index` -插入元素的索引。
2.  `newItem` -要插入的元素。
3.  `list` -项目清单。

### 描述

在给定索引处插入元素。如果索引太大，元素被插入到列表的末尾。

### 履行

```
const insert = (index, newItem, list) => {
  if (index > list.length - 1) {
    return [...list, newItem];
  }

  return list.reduce((acc, value, sourceArrayIndex) => {
    if (index === sourceArrayIndex) {
      acc.push(newItem, value);
    } else {
      acc.push(value);
    }

    return acc;
  }, []);
}; 
```

### 使用

```
insert(0, 'Batman', [1, 2, 3]);
// ['Batman', 1, 2, 3]

insert(1, 'Batman', [1, 2, 3]);
// [1, 'Batman', 2, 3]

insert(2, ['Batman'], [1, 2, 3]);
// [1, 2, ['Batman'], 3]

insert(10, ['Batman'], [1, 2, 3]);
// [1, 2, 3, ['Batman']] 
```

## 6.变平

### 因素

1.  `list` -项目清单。

### 描述

将项目列表平铺一级。

### 履行

```
const flatten = (list) => list.reduce((acc, value) => acc.concat(value), []); 
```

### 使用

```
flatten([[1, 2], [3, 4]]);
// [1, 2, 3, 4]

flatten([[1, 2], [[3, 4]]]);
// [1, 2, [3, 4]]

flatten([[[1, 2]], [3, 4]]);
// [[1, 2], 3, 4]

flatten([[[1, 2], [3, 4]]]);
// [[1, 2], [3, 4]] 
```

## 7.平面地图

### 因素

1.  `mappingFunction` -在每个列表项上运行的功能。
2.  `list` -项目清单。

### 描述

根据给定函数映射每个列表项，然后展平结果。

### 履行

```
// Kind of cheating, because we already implemented flatten ?
const flatMap = (mappingFunction, list) => flatten(list.map(mappingFunction)); 
```

### 使用

```
flatMap((n) => [n * 2], [1, 2, 3, 4]);
// [2, 4, 6, 8]

flatMap((n) => [n, n], [1, 2, 3, 4]);
// [1, 1, 2, 2, 3, 3, 4, 4]

flatMap((s) => s.split(' '), ['flatMap', 'should be', 'mapFlat']);
// ['flatMap', 'should', 'be', 'mapFlat'] 
```

## 8.包含

### 因素

1.  `item` -要检查列表的项目。
2.  `list` -项目清单。

### 描述

检查给定元素的列表。如果找到该元素，则返回`true`。否则返回`false`。

### 履行

```
const includes = (item, list) =>
  list.reduce((isIncluded, value) => isIncluded || item === value, false); 
```

### 使用

```
includes(3, [1, 2, 3]); // true
includes(3, [1, 2]); // false
includes(0, []); // false 
```

## 9.小型的，紧凑的

### 因素

1.  `list` -项目清单。

### 描述

从列表中删除“falsey”值。

### 履行

```
const compact = (list) =>
  list.reduce((acc, value) => {
    if (value) {
      acc.push(value);
    }

    return acc;
  }, []); 
```

### 使用

```
compact([0, null, 1, undefined, 2, '', 3, false, 4, NaN]);
// [1, 2, 3, 4] 
```

## 10.arrayIntoObject

### 因素

1.  `key` -用作新对象关键字的字符串。
2.  `list` -项目清单。

### 描述

使用给定的键作为新对象的键，将数组转换为对象。

### 履行

```
const arrayIntoObject = (key, list) =>
  list.reduce((acc, obj) => {
    const value = obj[key];

    acc[value] = obj;

    return acc;
  }, {}); 
```

### 使用

```
const users = [
    { username: 'JX01', status: 'offline' },
    { username: 'yazeedBee', status: 'online' }
];

arrayIntoObject('username', users);
/*
{
  JX01: {
    username: 'JX01',
    status: 'offline'
  },
  yazeedBee: { username: 'yazeedBee', status: 'online' }
}
*/

arrayIntoObject('status', users);
/*
{
  offline: {
    username: 'JX01',
    status: 'offline'
  },
  online: { username: 'yazeedBee', status: 'online' }
}
*/ 
```

## 想要免费辅导？

如果你想安排一个免费电话来讨论关于代码、面试、职业或任何其他方面的前端开发问题[请在 Twitter 上关注我，并给我发短信](https://twitter.com/yazeedBee)。

之后，如果你喜欢我们的第一次会议，我们可以讨论一个持续的辅导，以帮助你达到你的前端发展目标！

## 感谢阅读

更多类似的内容，请查看[https://yazeedb.com！](https://yazeedb.com)

下次见！