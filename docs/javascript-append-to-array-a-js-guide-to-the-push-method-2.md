# JavaScript Append to Array:Push 方法 JS 指南

> 原文：<https://www.freecodecamp.org/news/javascript-append-to-array-a-js-guide-to-the-push-method-2/>

有时你需要在数组末尾追加一个或多个新值。在这种情况下,`push()`方法就是你所需要的。

在 JavaScript 中，`push()`方法将在数组末尾添加一个或多个参数:

```
let arr = [0, 1, 2, 3];
arr.push(4);
console.log(arr); // [0, 1, 2, 3, 4]
```

这个方法接受无限数量的参数，您可以在数组末尾添加任意数量的元素。

```
let arr = [0, 1, 2, 3];
arr.push(4, 5, 6, 7, 8, 9);
console.log(arr); // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

`push()`方法也返回数组的新长度。

```
let arr = [0, 1, 2, 3];
let newLength = arr.push(4);
console.log(newLength); // 5
```

## JavaScript 中的`push`示例及常见错误

### 如何重新分配阵列

用来自`push`的输出重新分配数组是一个常见的错误。

```
let arr = [0, 1, 2, 3];
arr = arr.push(4);
console.log(arr); // 5
```

为了避免这个错误，你需要记住`push`改变数组，并返回新的长度。如果你用从`push()`返回的值重新分配变量，你正在覆盖数组值。

### 如何将一个数组的内容添加到另一个数组的末尾

如果你想把一个数组的内容添加到另一个数组的末尾，`push`是一个可以使用的方法。`push`会将您用作参数的任何内容添加为新元素。这对于另一个数组也是一样的，因此该数组必须用 spread 运算符来解包:

```
let arr1 = [0, 1, 2, 3];
let arr2 = [4, 5, 6, 7];
arr1.push(...arr2);
console.log(arr1); // [0, 1, 2, 3, 4, 5, 6, 7]
```

### 如何在类似数组的对象上使用`push`

有些对象类似于数组(比如`arguments`对象——允许访问一个函数的所有参数的对象),但是没有数组拥有的所有方法。

为了能够对它们使用`push`或其他数组方法，首先它们必须被转换成数组。

```
function myFunc() {
   let args = [...arguments];
   args.push(4);
   returns args;
}

console.log(myFunc(0, 1, 2, 3)); // [0, 1, 2, 3, 4]
```

如果不先将类似数组的`arguments`对象改为数组，代码会以`TypeError: arguments.push is not a function`结束。

## 结论

如果你使用数组，不要错过`push`。它在数组末尾添加一个或多个元素，并返回数组的新长度。