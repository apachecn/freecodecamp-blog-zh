# 如何在 JavaScript-JS 中反转数组？反向()函数

> 原文：<https://www.freecodecamp.org/news/how-to-reverse-an-array-in-javascript-js-reverse-function/>

在本文中，我将向您展示在 JavaScript 中反转数组的两种方法。

arrays 的`reverse`方法通过将最后一项作为第一项，并将第一项作为最后一项来反转数组。中间的其他项目也会分别反转。

在向您展示`reverse`方法的例子之前，让我向您展示如何在不使用数组的情况下反转数组。

我有这篇文章的视频版本，你也可以看看。

## 1.如何使用`for`循环反转数组

使用一个`for`循环(或任何其他类型的循环)，我们可以从最后一次到第一项循环遍历一个数组，并将这些值推送到一个新的数组中，这个新的数组就是相反的版本。方法如下:

```
const array = [1, 2, 3, 4]

const reversedArray = []

for(let i = array.length - 1; i >= 0; i--) {
  const valueAtIndex = array[i]

  reversedArray.push(valueAtIndex)
}

console.log(reversedArray)
// [4, 3, 2, 1] 
```

通过使用一个`for`循环，我们开始从最后一个值的索引(`array.length - 1`)循环到第一个值的索引(`0`)。然后我们将相应的值推送到`reversedArray`。

但是有一种更简单的方法来反转数组，那就是使用`reverse`方法。

## 2.如何使用`Array.reverse`反转一个数组

您可以使用`reverse`方法来反转数组，这是一种比`for`循环更容易读写的方法。此方法就地反转数组，这意味着使用它的数组被修改。

让我们看一个例子:

```
const array = [1, 2, 3, 4]

array.reverse()

console.log(array)
// [ 4, 3, 2, 1 ] 
```

正如您在这个例子中看到的，当对其应用`reverse`方法时,`array`被修改。

如果不想修改`array`，可以在应用`reverse`之前克隆它。`reverse`方法也返回反转的数组，因此您可以将该数组赋给一个变量。

以下是复制和反转数组的方法:

```
const array = [1, 2, 3, 4]

const reversed = [...array].reverse()

console.log(reversed)
// [ 4, 3, 2, 1 ]

console.log(array)
// [ 1, 2, 3, 4 ] 
```

在这里使用[扩展操作符](https://dillionmegida.com/p/spread-operator-simplified/)，我们首先克隆`array`，然后在克隆上应用`reverse`方法。返回的反向数组然后被返回给`reverse`变量。

如您所见，通过记录`array`，它没有受到影响，因为我们首先克隆了它。

## 感谢阅读！

所以，如果你需要反转一个数组，我希望这篇文章已经教会了你一些东西。