# JavaScript 中的循环嵌套

> 原文：<https://www.freecodecamp.org/news/nesting-for-loops-in-javascript/>

如果你在理解 freeCodeCamp 的[循环嵌套](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/basic-javascript/nesting-for-loops)挑战时有困难，不要担心。我们支持你。

在这个问题中你必须完成`multiplyAll()`函数，并以一个多维数组作为参数。请记住，多维数组，有时称为 2D 数组，只是一个数组的数组，例如，`[[1,2], [3,4], [5,6]]`。

在右边的编辑器中，`multiplyAll()`定义如下:

```
function multiplyAll(arr) {
  var product = 1;
  // Only change code below this line

  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]); 
```

您需要完成这个函数，它将变量`product`乘以参数`arr`的子数组中的每个数字，这是一个多维数组。

有很多不同的方法可以解决这个问题，但是我们将集中讨论使用`for`循环的最简单的方法。

## 设置您的`for`循环

因为`arr`是一个多维数组，所以需要两个`for`循环:一个循环遍历每个子数组，另一个循环遍历每个子数组中的元素。

### 遍历内部数组

为此，像你在之前的挑战中所做的那样，设置一个`for`循环:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {

  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]); 
```

注意，我们在循环中使用`let`而不是`var`来声明`product`。在这个挑战中，你不会注意到两者之间的区别，但通常情况下，尽可能使用 ES6 的`const`和`let`是个好习惯。你可以在这篇文章中阅读更多关于为什么[的内容。](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/)

现在，将每个子阵列记录到控制台:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]); 
```

因为您调用的是底部有`[[1,2],[3,4],[5,6,7]]`的`multiplyAll()`,所以您应该看到以下内容:

```
[ 1, 2 ]
[ 3, 4 ]
[ 5, 6, 7 ]
```

### 遍历每个子数组中的元素

现在，您需要遍历刚刚登录到控制台的子数组中的每个数字。

移除`console.log(arr[i]);`并在你刚刚编写的循环中创建另一个`for`循环:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr[i].length; j++) {

    }
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]); 
```

记住，对于内部循环，我们需要检查`arr[i]`的`.length`，因为`arr[i]`是我们之前看过的子数组之一。

现在将`arr[i][j]`登录到控制台，查看每个单独的元素:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr[i].length; j++) {
      console.log(arr[i][j]);
    }
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]); 
```

```
1
2
3
4
5
6
7
```

最后，将`product`乘以每个子数组中的每个元素:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr[i].length; j++) {
      product *= arr[i][j];
    }
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]);
```

如果您将`product`登录到控制台，您将看到每个测试案例的正确答案:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr[i].length; j++) {
      product *= arr[i][j];
    }
  }
  // Only change code above this line
  console.log(product);
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]);
```

```
6  // [[1], [2], [3]]
5040  // [[1, 2], [3, 4], [5, 6, 7]]
54  // [[5, 1], [0.2, 4, 0.5], [3, 9]]
```

## 近距离观察

如果你仍然不确定为什么上面的代码有效，不要担心——你并不孤单。使用嵌套循环是复杂的，甚至有经验的开发人员也会感到困惑。

在这种情况下，在控制台上记录一些更详细的信息会很有帮助。回到您的代码，在内部`for`循环之前将``Sub-array ${i}: ${arr[i]}``记录到控制台:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    console.log(`Sub-array ${i}: ${arr[i]}`);
    for (let j = 0; j < arr[i].length; j++) {
      product *= arr[i][j];
    }
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]);
```

在外层的`for`循环中，每次迭代都会遍历`arr`中的子数组。您应该会在控制台中看到:

```
Sub-array 0: 1,2
Sub-array 1: 3,4
Sub-array 2: 5,6,7
```

注意，我们使用的是上面的[模板文字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)。``Sub-array ${i}: ${arr[i]}``和`'Sub-array ' + i + ': ' + arr[i]`一样，只是写起来容易多了。

现在在内部`for`循环中，将``Element ${j}: ${arr[i][j]}``记录到控制台:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    console.log(`Sub-array ${i}: ${arr[i]}`);
    for (let j = 0; j < arr[i].length; j++) {
      console.log(`Element ${j}: ${arr[i][j]}`);
      product *= arr[i][j];
    }
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]);
```

内部的`for`循环遍历每个子数组(`arr[i]`)中的每个元素，所以您应该在控制台中看到这个:

```
Sub-array 0: 1,2
Element 0: 1
Element 1: 2
Sub-array 1: 3,4
Element 0: 3
Element 1: 4
Sub-array 2: 5,6,7
Element 0: 5
Element 1: 6
Element 2: 7
```

`i`的第一次迭代抓取第一个子数组`[1, 2]`。然后`j`的第一次迭代遍历子数组中的每个元素:

```
// i is 0
arr[0] // [1, 2];

// j is 0
arr[0][0] // 1
// j is 1
arr[0][1] // 2

-----

// i is 1
arr[1] // [3, 4]

// j is 0
arr[1][0] // 3
// j is 1
arr[1][1] // 4

...
```

这个例子非常简单，但是如果不将多种事情记录到控制台中，`arr[i][j]`仍然很难理解。

我们可以做的一个快速改进是在外`for`循环中声明一个`subArray`变量，并将其设置为等于`arr[i]`:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    const subArray = arr[i];
    for (let j = 0; j < arr[i].length; j++) {
      product *= arr[i][j];
    }
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]); 
```

然后对代码做一些调整，使用新的`subArray`变量代替`arr[i]`:

```
function multiplyAll(arr) {
  let product = 1;
  // Only change code below this line
  for (let i = 0; i < arr.length; i++) {
    const subArray = arr[i];
    for (let j = 0; j < subArray.length; j++) {
      product *= subArray[j];
    }
  }
  // Only change code above this line
  return product;
}

multiplyAll([[1,2],[3,4],[5,6,7]]); 
```

这就是你需要知道的关于多维数组和嵌套循环的一切。现在走出去，与他们中的佼佼者一起迭代！