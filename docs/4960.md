# 如何在 JavaScript 中克隆数组

> 原文：<https://www.freecodecamp.org/news/how-to-clone-an-array-in-javascript-1d3183468f6a/>

JavaScript 有很多方法可以做任何事情。我已经写了用 JavaScript 写 pipe/compose 的 [10 种方法，现在我们做数组。](https://www.freecodecamp.org/news/10-ways-to-write-pipe-compose-in-javascript-f6d54c575616/)

### 1.扩展运算符(浅层复制)

自从 ES6 下降以来，这一直是最受欢迎的方法。这是一个简短的语法，当使用 React 和 Redux 这样的库时，你会发现它非常有用。

```
numbers = [1, 2, 3];
numbersCopy = [...numbers]; 
```

**注意:**这不会安全地复制多维数组。数组/对象值由*引用*复制，而不是由*值*复制。

这很好

```
numbersCopy.push(4);
console.log(numbers, numbersCopy);
// [1, 2, 3] and [1, 2, 3, 4]
// numbers is left alone 
```

这不好

```
nestedNumbers = [[1], [2]];
numbersCopy = [...nestedNumbers];

numbersCopy[0].push(300);
console.log(nestedNumbers, numbersCopy);
// [[1, 300], [2]]
// [[1, 300], [2]]
// They've both been changed because they share references 
```

### 2.旧 for()循环(浅层复制)

鉴于函数式编程在我们的圈子里变得如此流行，我想这种方法是最不受欢迎的。

不管是纯的还是不纯的，声明式的还是命令式的，它都能完成任务！

```
numbers = [1, 2, 3];
numbersCopy = [];

for (i = 0; i < numbers.length; i++) {
  numbersCopy[i] = numbers[i];
} 
```

**注意:**这不会安全地复制多维数组。由于您使用了`=`操作符，它将通过*引用*而不是*值*来分配对象/数组。

这很好

```
numbersCopy.push(4);
console.log(numbers, numbersCopy);
// [1, 2, 3] and [1, 2, 3, 4]
// numbers is left alone 
```

这不好

```
nestedNumbers = [[1], [2]];
numbersCopy = [];

for (i = 0; i < nestedNumbers.length; i++) {
  numbersCopy[i] = nestedNumbers[i];
}

numbersCopy[0].push(300);
console.log(nestedNumbers, numbersCopy);
// [[1, 300], [2]]
// [[1, 300], [2]]
// They've both been changed because they share references 
```

### 3.旧 while()循环(浅层复制)

和`for`一样——不纯的，命令式的，等等，等等，这很有效！？

```
numbers = [1, 2, 3];
numbersCopy = [];
i = -1;

while (++i < numbers.length) {
  numbersCopy[i] = numbers[i];
} 
```

**注意:**这也通过*引用*而不是*值*来分配对象/数组。

这很好

```
numbersCopy.push(4);
console.log(numbers, numbersCopy);
// [1, 2, 3] and [1, 2, 3, 4]
// numbers is left alone 
```

这不好

```
nestedNumbers = [[1], [2]];
numbersCopy = [];

i = -1;

while (++i < nestedNumbers.length) {
  numbersCopy[i] = nestedNumbers[i];
}

numbersCopy[0].push(300);
console.log(nestedNumbers, numbersCopy);
// [[1, 300], [2]]
// [[1, 300], [2]]
// They've both been changed because they share references 
```

### 4.Array.map(浅层副本)

回到现代领域，我们将找到`map`函数。[植根于数学](https://en.wikipedia.org/wiki/Morphism)，`map`是将一个集合转化为另一种类型的集合，同时保留结构的概念。

在英语中，这意味着`Array.map`每次都返回相同长度的数组。

要将数字列表加倍，请将`map`与`double`函数一起使用。

```
numbers = [1, 2, 3];
double = (x) => x * 2;

numbers.map(double); 
```

#### 克隆呢？？

没错，这篇文章是关于克隆数组的。要复制一个数组，只需在您的`map`调用中返回元素。

```
numbers = [1, 2, 3];
numbersCopy = numbers.map((x) => x); 
```

如果想更数学一点的话，`(x) => x`叫做 [*身份*](https://en.wikipedia.org/wiki/Identity_function) 。它返回给它的任何参数。

`map(identity)`克隆人名单。

```
identity = (x) => x;
numbers.map(identity);
// [1, 2, 3] 
```

**注意:**这也通过*引用*而不是*值*来分配对象/数组。

### 5.Array.filter(浅层复制)

这个函数返回一个数组，就像`map`一样，但是不保证长度相同。

如果你在过滤偶数呢？

```
[1, 2, 3].filter((x) => x % 2 === 0);
// [2] 
```

输入数组长度为 3，但结果长度为 1。

然而，如果您的`filter`的谓词总是返回`true`，您将得到一个副本！

```
numbers = [1, 2, 3];
numbersCopy = numbers.filter(() => true); 
```

每个元素都通过了测试，所以它被返回。

**注意:**这也通过*引用*而不是*值*来分配对象/数组。

### 6.Array.reduce(浅层复制)

使用`reduce`来克隆一个数组，我几乎感觉很糟糕，因为它比那要强大得多。但是我们开始了…

```
numbers = [1, 2, 3];

numbersCopy = numbers.reduce((newArray, element) => {
  newArray.push(element);

  return newArray;
}, []); 
```

`reduce`在遍历列表时转换初始值。

这里的初始值是一个空数组，我们用每个元素填充它。该数组必须从函数中返回，以便在下一次迭代中使用。

**注意:**这也通过*引用*而不是*值*来分配对象/数组。

### 7.Array.slice(浅层拷贝)

`slice`根据您提供的开始/结束索引返回一个数组的*浅*副本。

如果我们想要前三个元素:

```
[1, 2, 3, 4, 5].slice(0, 3);
// [1, 2, 3]
// Starts at index 0, stops at index 3 
```

如果我们想要所有的元素，不要给任何参数

```
numbers = [1, 2, 3, 4, 5];
numbersCopy = numbers.slice();
// [1, 2, 3, 4, 5] 
```

**注意:**这是一个*浅*副本，所以它也通过*引用*而不是*值*来分配对象/数组。

### 8.JSON.parse 和 JSON.stringify(深层复制)

将一个对象变成一个字符串。

将一个字符串变成一个对象。

将它们组合在一起可以将一个对象转换成一个字符串，然后反转这个过程来创建一个全新的数据结构。

**注意:这个** **安全地复制深度嵌套的对象/数组**！

```
nestedNumbers = [[1], [2]];
numbersCopy = JSON.parse(JSON.stringify(nestedNumbers));

numbersCopy[0].push(300);
console.log(nestedNumbers, numbersCopy);

// [[1], [2]]
// [[1, 300], [2]]
// These two arrays are completely separate! 
```

### 9.Array.concat(浅层复制)

`concat`将数组与数值或其他数组组合。

```
[1, 2, 3].concat(4); // [1, 2, 3, 4]
[1, 2, 3].concat([4, 5]); // [1, 2, 3, 4, 5] 
```

如果你什么都不给或者给一个空数组，那么返回一个浅拷贝。

```
[1, 2, 3].concat(); // [1, 2, 3]
[1, 2, 3].concat([]); // [1, 2, 3] 
```

**注意:**这也通过*引用*而不是*值*来分配对象/数组。

### 10.Array.from(浅层复制)

这可以将任何可迭代的对象转换成数组。给一个数组返回一个浅拷贝。

```
numbers = [1, 2, 3];
numbersCopy = Array.from(numbers);
// [1, 2, 3] 
```

**注意:**这也通过*引用*而不是*值*来分配对象/数组。

### 结论

嗯，这很有趣？

我试着只用一个步骤来克隆。如果你采用多种方法和技术，你会发现更多的方法。