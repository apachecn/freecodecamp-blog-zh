# 还有另外 10 个用 Reduce 实现的实用函数

> 原文：<https://www.freecodecamp.org/news/yet-another-10-utils-using-reduce/>

总共 30 个功能！

这是我用 Reduce 写的第三篇关于效用函数的文章。

*   [第一部分](https://www.freecodecamp.org/news/10-js-util-functions-in-reduce/) (10 项功能)
*   [第二部分](https://www.freecodecamp.org/news/10-more-js-utils-using-reduce/) (10 项功能)

现在我们总共有*三十个*使用 JavaScript 的`reduce`制作的助手函数。关于`reduce`本身的更多细节，可以考虑阅读我的关于的[教程。](https://www.yazeedb.com/posts/learn-reduce-in-10-minutes)

下面列出的函数的灵感来自令人惊叹的 [RamdaJS](https://ramdajs.com/) 库。我还编写了单元测试来确保正确的行为，你可以在[我的 GitHub](https://github.com/yazeedb/js-utils-using-reduce) 上找到。

## 1.调整

### 因素

1.  `index` -源数组的索引
2.  `fn` -在`index`应用的函数
3.  `list` -项目清单。

### 描述

对给定索引处的值应用函数。如果提供的索引超出界限，则返回原始数组。

### 使用

```
const double = (x) => x * 2;

adjust(1, double, [1, 2, 3]);
// [1, 4, 3]

adjust(4, double, [1, 2, 3]);
// [1, 2, 3] 
```

### 履行

```
const adjust = (index, fn, list) =>
  list.reduce((acc, value, sourceArrayIndex) => {
    const valueToUse = sourceArrayIndex === index ? fn(value) : value;

    acc.push(valueToUse);

    return acc;
  }, []); 
```

## 2.来自巴黎

### 因素

1.  `pairs` -键值对列表。

### 描述

从键值对列表创建对象。

### 使用

```
fromPairs([['a', 1], ['b', 2], ['c', 3]]);
// { a: 1, b: 2, c: 3 } 
```

### 履行

```
const fromPairs = (pairs) =>
  pairs.reduce((acc, currentPair) => {
    const [key, value] = currentPair;

    acc[key] = value;

    return acc;
  }, {}); 
```

## 3.范围

### 因素

1.  `from` -起始号码。
2.  `to` -结束号。

### 描述

返回给定范围内的数字列表。

### 使用

```
range(1, 5);
// [1, 2, 3, 4, 5] 
```

### 履行

```
const range = (from, to) =>
  Array.from({ length: to - from + 1 }).reduce((acc, _, index) => {
    acc.push(from + index);

    return acc;
  }, []); 
```

## 4.重复

### 因素

1.  `item` -要重复的项目。
2.  `times` -重复的次数。

### 描述

返回给定值的列表 N 次。

### 使用

```
repeat({ favoriteLanguage: 'JavaScript' }, 2);

/*
[{
    favoriteLanguage: 'JavaScript'
}, {
    favoriteLanguage: 'JavaScript'
}],
*/ 
```

### 履行

```
const repeat = (item, times) =>
  Array.from({ length: times }).reduce((acc) => {
    acc.push(item);

    return acc;
  }, []); 
```

## 5.倍

### 因素

1.  `fn` -要调用的功能。
2.  `numTimes` -调用该函数的次数。

### 描述

调用给定函数 N 次。提供的`fn`接收当前索引作为参数。

### 使用

```
times((x) => x * 2, 3);
// [0, 2, 4] 
```

### 履行

```
const times = (fn, numTimes) =>
  Array.from({ length: numTimes }).reduce((acc, _, index) => {
    acc.push(fn(index));

    return acc;
  }, []); 
```

## 6.删除重复数据

### 因素

1.  `items` -项目清单。

### 描述

对列表中的项目进行重复数据删除。

### 使用

```
deduplicate([[1], [1], { hello: 'world' }, { hello: 'world' }]);
// [[1], { hello: 'world' }] 
```

### 履行

```
const deduplicate = (items) => {
  const cache = {};

  return items.reduce((acc, item) => {
    const alreadyIncluded = cache[item] === true;

    if (!alreadyIncluded) {
      cache[item] = true;
      acc.push(item);
    }

    return acc;
  }, []);
}; 
```

## 7.反面的

### 因素

1.  `list` -项目清单。

### 描述

不像本地的`Array.reverse`方法，通过返回一个新的列表来反转一个列表*而不需要*改变它。

### 使用

```
reverse([1, 2, 3]);
// [3, 2, 1] 
```

### 履行

```
const reverse = (list) =>
  list.reduce((acc, _, index) => {
    const reverseIndex = list.length - index - 1;
    const reverseValue = list[reverseIndex];

    acc.push(reverseValue);

    return acc;
  }, []); 
```

## 8.插入全部

### 因素

1.  `index` -要插入的索引。
2.  `subList` -要插入的列表。
3.  `sourceList` -信号源列表。

### 描述

将子列表插入到列表中的给定索引处。如果索引太大，则追加到列表的末尾。

### 使用

```
insertAll(1, [2, 3, 4], [1, 5]);
// [1, 2, 3, 4, 5]

insertAll(10, [2, 3, 4], [1, 5]);
// [1, 5, 2, 3, 4] 
```

### 履行

```
const insertAll = (index, subList, sourceList) => {
  if (index > sourceList.length - 1) {
    return sourceList.concat(subList);
  }

  return sourceList.reduce((acc, value, sourceArrayIndex) => {
    if (index === sourceArrayIndex) {
      acc.push(...subList, value);
    } else {
      acc.push(value);
    }

    return acc;
  }, []);
}; 
```

## 9.mergeAll

### 因素

1.  `objectList` -要合并的对象列表。

### 描述

将一系列对象合并成一个。

### 使用

```
mergeAll([
    { js: 'reduce' },
    { elm: 'fold' },
    { java: 'collect' },
    { js: 'reduce' }
]);

/*
{
    js: 'reduce',
    elm: 'fold',
    java: 'collect'
}
*/ 
```

### 履行

```
const mergeAll = (objectList) =>
  objectList.reduce((acc, obj) => {
    Object.keys(obj).forEach((key) => {
      acc[key] = obj[key];
    });

    return acc;
  }, {}); 
```

## 10.xprod

### 因素

1.  `list1` -项目清单。
2.  `list2` -项目清单。

### 描述

给定一个列表，返回所有可能对的新列表。

### 使用

```
xprod(['Hello', 'World'], ['JavaScript', 'Reduce']);
/*
[
  ['Hello', 'JavaScript'],
  ['Hello', 'Reduce'],
  ['World', 'JavaScript'],
  ['World', 'Reduce']
]
*/ 
```

### 履行

```
const xprod = (list1, list2) =>
  list1.reduce((acc, list1Item) => {
    list2.forEach((list2Item) => {
      acc.push([list1Item, list2Item]);
    });

    return acc;
  }, []); 
```

## 想要免费辅导？

如果你想安排一个免费电话来讨论关于代码、面试、职业或任何其他方面的前端开发问题[请在 Twitter 上关注我，并给我发短信](https://twitter.com/yazeedBee)。

之后，如果你喜欢我们的第一次会议，我们可以讨论一个持续的辅导，以帮助你达到你的前端发展目标！

## 感谢阅读

更多类似的内容，请查看[https://yazeedb.com！](https://yazeedb.com)

下次见！