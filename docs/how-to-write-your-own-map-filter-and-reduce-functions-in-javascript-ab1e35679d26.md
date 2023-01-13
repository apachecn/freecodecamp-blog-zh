# 如何用 JavaScript 写自己的 map，filter，reduce 函数

> 原文：<https://www.freecodecamp.org/news/how-to-write-your-own-map-filter-and-reduce-functions-in-javascript-ab1e35679d26/>

作者:哼哼和奈尔

# 如何用 JavaScript 写自己的 map，filter，reduce 函数

#### Javascript 中函数式编程和高阶函数的一瞥。

![8rPml1LfKKppm-gvbSSoKziPC-RHoF0CsVKg](img/959ecf59aee6aa0bf6090ce971a4f339.png)

Photo by [Christopher Robin Ebbinghaus](https://unsplash.com/photos/pgSkeh0yl8o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/javascript?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

每当我听到函数式编程，我首先想到的是高阶函数。对于不了解高阶函数的人，维基百科是这么说的:

一个 [**高阶函数**](https://en.wikipedia.org/wiki/Higher-order_function) 是一个至少完成下列之一的函数:

*   接受一个或多个函数作为参数，
*   返回一个函数作为结果。

高阶函数可以通过映射、过滤和归约函数来最好地描述。默认情况下，Javascript 有自己的函数实现。今天，我们将编写自己的地图，过滤和减少功能。

**注意:请记住，map、filter 和 reduce 方法的这些实现可能不会反映其 Javascript 对应物的本机实现。**

#### 地图

从 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) :

> `**map()**`方法创建一个新数组，其结果是在调用数组中的每个元素上调用一个提供的函数。

似乎很简单。现在让我们看看 Javascript `map()`的运行情况！

```
let arr = [1, 2, 3, 4, 5];
```

```
// pass a function to mapconst squareArr = arr.map(num => num ** 2);
```

```
console.log(squareArr); // prints [1, 4, 9, 16, 25]
```

刚刚发生了什么？我们编写了一个返回数字平方的函数，并将该函数作为参数传递给我们的`map()`。让我们一步一步地看如何创建我们自己的地图功能。

1.  创建一个空数组`mapArr`。
2.  循环遍历数组元素。
3.  以当前柠檬为参数调用函数`mapFunc`。
4.  将`mapFunc`函数的结果推送到`mapArr`数组。
5.  遍历完所有元素后返回`mapArr`数组。

现在让我们编写`map()`的实现

```
// map takes an array and function as argumentfunction map(arr, mapFunc) {    const mapArr = []; // empty array        // loop though array    for(let i=0;i<arr.length;i++) {        const result = mapFunc(arr[i], i, arr);        mapArr.push(result);    }    return mapArr;}
```

现在，如果您在前面的示例代码中调用新的`map()`，

```
const squareArr2 = map(arr, num => num ** 2);
```

```
console.log(squareArr2); // prints [1, 4, 9, 16, 25]
```

很酷吧？接下来让我们进入`filter()`。

#### **过滤器**

从 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) :

> `**filter()**`方法创建一个新数组，其中所有元素都通过了由提供的函数实现的测试。

让我们看一个例子:

```
let arr = [1, 2, 3, 4, 5];
```

```
// pass a function to filterconst oddArr = arr.filter(num => num % 2 === 0);
```

```
console.log(oddArr); // prints [2, 4]
```

filter 函数采用了一个函数，如果数字是偶数，该函数将返回`true`。`filter()`根据元素是真还是假来“过滤”输入数组。让我们一步一步地了解一下`filter()`是如何工作的。

1.  创建一个空数组`filterArr`。
2.  遍历数组元素。
3.  用当前元素作为参数调用了`filterFunc`函数。
4.  如果结果为真，则将元素推送到`filterArr`数组。
5.  遍历完所有元素后返回`filterArr`数组。

是时候写我们自己的了`filter()`

```
// filter takes an array and function as argumentfunction filter(arr, filterFunc) {    const filterArr = []; // empty array        // loop though array    for(let i=0;i<arr.length;i++) {        const result = filterFunc(arr[i], i, arr);        // push the current element if result is true        if(result)             filterArr.push(arr[i]);     }    return filterArr;}
```

让我们看看我们的新`filter()`是否与前面的例子一样:

```
const oddArr2 = filter(arr, num => num % 2 === 0);
```

```
console.log(oddArr2); //prints [2, 4]
```

整洁！我把最好最难的留到了最后。接下来我们来看`reduce()`。

#### **减少**

从 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) :

> `**reduce()**`方法对数组的每个成员执行一个**缩减器**函数(您提供的),产生一个输出值。

有道理？没有吗？这里有一个例子可以帮助你理解:

```
let arr = [1, 2, 3, 4];const sumReducer = (accumulator, currentValue) => accumulator + currentValue;
```

```
// 1 + 2 + 3 + 4const sum = arr.reduce(sumReducer);console.log(sum);// prints 10
```

```
// 5 + 1 + 2 + 3 + 4const sum2 = arr.reduce(sumReducer);console.log(sum2);// prints 15
```

开始有印象了吗？先说清楚。在深入研究`reduce()`方法之前，您可能需要熟悉一下减速器的功能。

如果您过去使用过 [**redux**](https://redux.js.org) ，您可能会对什么是减速器功能有所了解。在上面的例子中，reducer 函数被写成累加器和当前值之和。当您将 reducer 函数传递给`reduce()`方法时，它将遍历数组中的每个数字，并将其添加到累加器(开始时为 0)，累加器本身将成为下一次迭代的新累加器。这一直持续到数组的末尾，并返回累加器的结果。

如果我必须在上述示例的每一步中输出累加器的值，它将是这样的:

*   在迭代开始之前，`accumulator = 0`
*   第一次迭代，`accumulator += 1; // accumulator = 1`
*   第二次迭代，`accumulator += 2; // accumulator = 3`
*   第三次迭代，`accumulator += 3; // accumulator = 6`
*   第四次迭代，`accumulator += 4; // accumulator = 10`

您的 **reducer** 函数的返回值被分配给累加器，累加器的值会在整个数组的每次迭代中被记住。它最终成为最终的单个结果值。

如果您仍然停留在某一点，尝试用内置的`reduce()`方法编写一些操作。每当您觉得自己准备好了，就按照下面的步骤来实现您的定制`reduce()`:

1.  用来自`reduce()`的 0 或`initalValue`参数初始化`accumulator`变量。
2.  遍历数组元素。
3.  用`accumulator`和当前元素作为参数调用`reducer`函数。
4.  遍历完所有元素后返回`accumulator`。

好了，该编码了。

```
// reducer takes an array, reducer() and initialValue as argumentfunction reduce(arr, reducer, initialValue) {    let accumulator = initialValue === undefined ? 0 : initialValue        // loop though array    for(let i=0;i<arr.length;i++)        accumulator = reducer(accumulator, arr[i], i, arr);    return accumulator;}
```

嗯，这比预期的要容易。让我们看看它是否有效。

```
const sum = reduce(arr, sumReducer);
```

```
console.log(sum); // prints 10
```

```
const sum2 = reduce(arr, sumReducer, 5);
```

```
console.log(sum2);// prints 15
```

非常管用！

**就这样:)**

如果你有任何问题，请在下面评论。