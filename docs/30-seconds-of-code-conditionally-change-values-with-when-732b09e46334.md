# 如何在 JavaScript 中用 when()有条件地改变值

> 原文：<https://www.freecodecamp.org/news/30-seconds-of-code-conditionally-change-values-with-when-732b09e46334/>

《30 秒代码》是一个精彩的 JavaScript 片段集，可在≤ 30 秒内消化。任何想要掌握 JavaScript 的人都应该从头到尾看一遍。

受[拉姆达](http://ramdajs.com/docs/#when)的启发，我为 30secondsofcode 的[官方 GitHub 回购](https://github.com/Chalarangelo/30-seconds-of-code/pull/652)贡献了`when()`。这是我最喜欢功能之一。

`when()`取 3 个参数:

1.  `pred`:谓词函数(必须返回`true`或`false`)
2.  `whenTrue`:当`pred`返回`true`时运行的函数。
3.  一个值:`x`。

下面是最基本的实现:

```
when = (pred, whenTrue, x) => {
  if (pred(x)) {
    return whenTrue(x);
  } else {
    return x;
  }
}; 
```

您可以将其简化为:

```
when = (pred, whenTrue, x) => (pred(x) ? whenTrue(x) : x); 
```

假设我们想要三倍的偶数

```
when((x) => x % 2 === 0, (x) => x * 3, 2);
// 6 
```

我们得到了`6`，因为`2`是一个偶数。如果我们通过`11`呢？

```
when((x) => x % 2 === 0, (x) => x * 3, 11);
// 11 
```

### 更进一步

`when`目前一次需要所有 3 个参数——如果我们可以只提供前 2 个，以后再给`x`会怎么样？

```
when = (pred, whenTrue) => (x) => (pred(x) ? whenTrue(x) : x); 
```

这个版本是我提交给[30secondsofcode.org](https://30secondsofcode.org/function#when)的。现在我们的代码更加灵活。

```
tripleEvenNums = when((x) => x % 2 === 0, (x) => x * 3);

tripleEvenNums(20); // 60
tripleEvenNums(21); // 21
tripleEvenNums(22); // 66 
```

### 甚至更远

我们可以稍后传递`x`，因为`when(pred, whenTrue)`返回一个期望`x`的函数。如果我们咖喱`when()`呢？

如果你是奉承的新手，可以看看我在上面的文章。

一个 curried 函数不需要立刻得到所有的参数。您可以提供一些，并获得一个函数来处理其余的，从而实现强大的模式。

#### 一个愚蠢的例子

假设我们有两个人员列表，都包含一个名为`Bobo`的人。

`Bobo`想要每个列表的昵称。

*   如果我们在列表 1 中找到`Bobo`，就把他的名字改成`B Money`。
*   如果我们在列表 2 中找到`Bobo`，就把他的名字改成`Bo-bob`。

Currying `when`允许我们轻松地为每个关注点编写一个函数。

如果你正在跟进，这里有一个来自[30secondsofcode.org](https://30secondsofcode.org/function#curry)的`curry`函数。

```
curry = (fn, arity = fn.length, ...args) =>
  arity <= args.length ? fn(...args) : curry.bind(null, fn, arity, ...args); 
```

我们需要一个谓词来查找`Bobo`。

```
isBobo = (person) => person.name === 'Bobo'; 
```

为了保持我们的功能不变，我们需要一种方法来*不变地*改变一个人的名字。

```
changeName = (newName, obj) => ({
  ...obj,
  name: newName
}); 
```

让我们把它做成咖喱，这样我们就可以供应了。

```
changeName = curry((newName, obj) => ({
  ...obj,
  name: newName
})); 
```

这是我们的名单。

```
list1 = [
  {
    name: 'Bobo',
    id: 1,
    iq: 9001
  },
  {
    name: 'Jaime',
    id: 2,
    iq: 9000
  },
  {
    name: 'Derek',
    id: 3,
    iq: 8999
  }
];

list2 = [
  {
    name: 'Sam',
    id: 1,
    iq: 600
  },
  {
    name: 'Bobo',
    id: 2,
    iq: 9001
  },
  {
    name: 'Peter',
    id: 3,
    iq: 8
  }
]; 
```

让我们来映射一下`list1`。

```
doIfBobo = when(isBobo);
renameToBMoney = changeName('B Money');

list1.map(doIfBobo(renameToBMoney)); 
```

我们的结果:

```
[
  {
    name: 'B Money',
    id: 1,
    iq: 9001
  },
  {
    name: 'Jaime',
    id: 2,
    iq: 9000
  },
  {
    name: 'Derek',
    id: 3,
    iq: 8999
  }
]; 
```

因为`when`，我们只改了`Bobo`，其他人都忽略了！

现在映射过去`list2`。

```
renameToBoBob = changeName('Bo-bob');

list2.map(doIfBobo(renameToBoBob)); 
```

```
Our result:

[{
  "name": "Sam",
  "id": 1,
  "iq": 600
},
 {
 "name": "Bo-bob",
   "id": 2,
   "iq": 9001**
 },
 {
   "name": "Peter",
   "id": 3,
   "iq": 8
 }
]; 
```

我看不错！我们给他起了绰号，没有影响到任何人。

如果您有进一步的兴趣，请考虑以下链接:

*   [30secondsofcode.org 的收藏](https://30secondsofcode.org/array)
*   [我的阿谀文章](https://medium.com/front-end-hacking/how-does-javascripts-curry-actually-work-8d5a6f891499)
*   [拉姆达](http://ramdajs.com/docs/)