# 如何从 JavaScript 数组中移除元素——移除 JS 中的特定项目

> 原文：<https://www.freecodecamp.org/news/how-to-remove-an-element-from-a-javascript-array-removing-a-specific-item-in-js/>

在 JavaScript 中，您经常需要从数组中移除元素，无论是为了队列数据结构，还是为了您的 React 状态。

在本文的前半部分，您将学习所有允许您在不改变原始数组的情况下从数组中移除元素的方法。事实上，这是你最想做的事情。例如，如果您不想改变您的反应状态。或者在代码的其他部分使用了该数组，改变它会导致意外的问题。

避免突变总是更好！

但是，为了完整起见，本文的后半部分将列出从数组中移除一个项的方法。这些方法确实会改变数组本身。

如果您想浏览某个特定的部分，可以在这里找到文章内容的摘要。

*   [如何在不改变数组的情况下从数组中移除元素](#how-to-remove-an-element-from-an-array-without-mutating-the-array)
    *   [用`slice`](#remove-the-first-element-of-an-array-with-slice) 删除数组的第一个元素
    *   [用`slice`](#remove-the-last-element-of-an-array-with-slice) 移除数组的最后一个元素
    *   [用`slice`和`concat`](#remove-an-element-at-any-position-of-an-array-with-slice-and-concat) 删除数组中任意位置的元素
    *   [用`filter`](#remove-an-element-of-a-certain-value-with-filter) 去掉某个值的元素
    *   [用`for`循环和`push`](#remove-an-element-from-an-array-with-a-for-loop-and-push) 从数组中移除元素
    *   [用析构和剩余操作符移除数组的第一个元素](#remove-the-first-element-of-an-array-with-destructuring-and-the-rest-operator)
*   [如何在数组变异时从数组中移除元素](#how-to-remove-an-element-from-an-array-while-mutating-the-array)
    *   [用`pop`](#remove-the-last-element-of-an-array-with-pop) 移除数组的最后一个元素
    *   [用`shift`](#remove-the-first-element-of-an-array-with-shift) 删除数组的第一个元素
    *   [用`splice`](#remove-an-element-at-any-index-with-splice) 删除任意索引处的元素
*   [结论](#conclusion)

## 如何在不改变数组的情况下从数组中移除元素

如果你有一个输入数组，比如一个函数参数，最佳实践表明你不应该改变数组。相反，你应该创建一个新的。

有几种方法可以用来在不改变数组的情况下从数组中移除特定的项。

为了避免改变数组，将创建一个不包含要删除的元素的新数组。

您可以使用以下方法:

*   `Array.prototype.slice()`
*   `Array.prototype.slice()`连同`Array.prototype.concat()`
*   `Array.prototype.filter()`
*   一个`for`回路和`Array.prototype.push()`

让我们详细看看如何使用其中的每一个从数组中删除一个元素，而不改变原来的元素。

### 用`slice`删除数组的第一个元素

如果你想删除一个数组中的第一个元素，你可以对一个名为`arr`的数组使用`Array.prototype.slice()`，就像这样:`arr.slice(1)`。

这是一个完整的例子，在这个例子中，你想从包含字母表的前 6 个字母的数组中删除第一个元素。

```
// the starting array
const arrayOfLetters = ['a', 'b', 'c', 'd', 'e', 'f'];

// here the array is copied, without the first element
const copyWithoutFirstElement = arrayOfLetters.slice(1);

// arrayOfLetters is unchanged
console.log(arrayOfLetters) // ['a', 'b', 'c', 'd', 'e', 'f']

// and copyWithoutFirstElement contains the letters from b to f
console.log(copyWithoutFirstElement) // ['b', 'c', 'd', 'e', 'f']
```

`slice`方法可以接受一个数字作为参数，在这种情况下，它从该索引复制到数组的末尾。所以使用`arrayOfLetters.slice(1)`将创建一个排除第一个元素的`arrayOfLetters`数组的副本。

### 用`slice`删除数组的最后一个元素

如果你想移除的元素是数组的最后一个元素，你可以在一个名为`arr`的数组上使用`Array.prototype.slice()`，方法是:`arr.slice(0, -1)`。

这是一个完整的例子，使用了上面相同的字母数组，从一个由前 6 个字母组成的数组开始。

```
const arrayOfLetters = ['a', 'b', 'c', 'd', 'e', 'f'];
const copyWithoutLastElement = arrayOfLetters.slice(0, -1);

// arrayOfLetters is unchanged
console.log(arrayOfLetters) // ['a', 'b', 'c', 'd', 'e', 'f']

console.log(copyWithoutLastElement) // ['a', 'b', 'c', 'd', 'e']
```

`slice`方法最多接受两个参数。`slice`的第一个索引表示从哪个索引开始复制，第二个参数表示复制到哪个元素——但是它不包含。

`slice`接受负索引，从末尾开始计数。这意味着写入`-1`将意味着最后一个索引。因此，从`0`到`-1`意味着从索引`0`到(但不包括)最后一个索引创建一个副本。最终结果是最后一个元素不包含在副本中。

### 用`slice`和`concat`删除数组中任意位置的元素

如果你想创建一个在任何索引处缺少一个元素的副本，你可以这样一起使用`Array.prototype.slice`和`Array.prototype.concat`:`arrayOfLetters.slice(0, n).concat(arrayOfLetters.slice(n+1))`其中`n`是你想删除的元素的索引。

```
const arrayOfLetters = ['a', 'b', 'c', 'd', 'e', 'f'];

const halfBeforeTheUnwantedElement = arrayOfLetters.slice(0, 2)

const halfAfterTheUnwantedElement = arrayOfLetters(3);

const copyWithoutThirdElement = halfBeforeTheUnwantedElement.concat(halfAfterTheUnwantedElement);

// arrayOfLetters is unchanged
console.log(arrayOfLetters) // ['a', 'b', 'c', 'd', 'e', 'f']

console.log(copyWithoutFifthElement) // ['a', 'b', 'd', 'e', 'f']
```

`slice`的这种用法是将前面两种用法放在一起的一种方式。

第一次使用`slice`将从头开始创建一个数组，刚好在您想要删除的元素之前。

第二次使用`slice`创建一个数组，从您想要移除的元素之后到数组末尾。

这两个数组用`concat`连接在一起，形成一个与起始数组相似的数组，但是没有特定的元素。

### 用`filter`删除某个值的元素

如果要删除某个值的元素，可以使用`Array.prototype.filter()`。让我们用同样的`arrayOfLetters`创建一个没有`d`的副本。

```
const arrayOfLetters = ['a', 'b', 'c', 'd', 'e', 'f'];

const arrayWithoutD = arrayOfLetters.filter(function (letter) {
    return letter !== 'd';
});

// arrayOfLetters is unchanged
console.log(arrayOfLetters); // ['a', 'b', 'c', 'd', 'e', 'f']

console.log(arrayWithoutD); // ['a', 'b', 'c', 'e', 'f']
```

接受一个回调，并用该回调测试数组的所有元素。它保留回调返回`true`(或真值)的元素，排除回调返回`false`(或假值)的元素。

在这种情况下，回调函数检查`letter !== "d"`,因此它为字母`d`返回`false`,为所有其他字母返回`true`,导致数组中不包含字母`d`。

对`filter`的回调按顺序传递三个参数:元素本身、元素的索引和整个数组。

您可以根据需要创建比这个示例更复杂的条件。

### 用`for`循环和`push`从数组中删除一个元素

从数组中删除元素而不改变原始数组的最后一种方法是使用`push`方法。

通过这些简单的步骤:

1.  创建一个空数组
2.  遍历原始数组
3.  将想要保留的元素推入空数组

```
const arrayOfLetters = ['a', 'b', 'c', 'd', 'e', 'f'];

const arrayWithoutB = [];

for (let i = 0; i < arrayOfLetters.length; i++) {
    if (arrayOfLetters[i] !== 'b') {
        arrayWithoutH.push(arrayOfLetters[i]);
    }
}

// arrayOfLetters is unchanged
console.log(arrayOfLetters); // ['a', 'b', 'c', 'd', 'e', 'f']

console.log(arrayWithoutB); // ['a', 'c', 'd', 'e', 'f']
```

对于更复杂的语句，`if`语句的条件可以检查索引(`i`)和元素的值。

### 使用析构和 rest 运算符移除数组的第一个元素

数组析构和 rest 操作符是两个有点混乱的概念。

如果你想更深入地了解这个主题，我推荐这篇文章，它讨论了如何析构一个数组。

你可以使用析构来移除第一个元素——比方说一个名为`arr`的数组——并以这种方式创建一个名为`newArr`的新数组:`const [ , ...newarr] = arr;`。

现在，让我们看一个如何使用析构和 rest 操作符的实际例子。

```
const arrayOfFruits = ['olive', 'apricot', 'cherry', 'peach', 'plum', 'mango'];

const [ , ...arrayOfCulinaryFruits] = arrayOfFruits;

// arrayOfFruits is unchanged
console.log(arrayOfFruits); // ['olive', 'apricot', 'cherry', 'peach', 'plum', 'mango']

console.log(arrayOfCulinaryFruits); // ['apricot', 'cherry', 'peach', 'plum', 'mango']
```

在 rest 操作符前加一个逗号表示避开数组中的第一个元素，所有其他元素都被复制到`arrayOfCulinaryFruits`数组中。

## 如何在改变数组时从数组中移除元素

在某些情况下，改变原始数组可能是合适的。在这些情况下，您也可以使用下列变异方法之一。

*   `Array.prototype.pop()`
*   `Array.prototype.shift()`
*   `Array.prototype.splice()`

### 用`pop`删除数组的最后一个元素

你可以用`Array.prototype.pop()`移除数组的最后一项。

如果你有一个名为`arr`的数组，它看起来就像`arr.pop()`。

```
const arrayOfNumbers = [1, 2, 3, 4];

const previousLastElementOfTheArray = arrayOfNumbers.pop();

console.log(arrayOfNumbers); // [1, 2, 3]

console.log(previousLastElementOfTheArray); // 4
```

在数组上使用了`pop`方法，它通过删除数组的最后一项来改变数组。

`pop`方法也返回被移除的元素。

### 用`shift`删除数组的第一个元素

可以在数组上使用`shift`方法来移除数组的第一个元素。

如果你有一个名为`arr`的数组，它可以这样使用:`arr.shift()`。

```
const arrayOfNumbers = [1, 2, 3, 4];

const previousFirstElementOfTheArray = arrayOfNumbers.shift();

console.log(arrayOfNumbers); // [2, 3, 4]

console.log(previousFirstElementOfTheArray); // 1
```

方法删除数组的第一项。

它还返回移除的元素。

### 用`splice`删除任何索引处的元素

您可以使用`splice`方法移除任何索引处的元素。

如果你有一个名为`arr`的数组，它可以以这种方式移除任意索引处的元素:`arr.splice(n, 1)`，其中`n`是要移除的元素的索引。

```
const arrayOfNumbers = [1, 2, 3, 4];

const previousSecondElementOfTheArray = arrayOfNumbers.splice(1, 1);

console.log(arrayOfNumbers); // [1, 3, 4]

console.log(previousSecondElementOfTheArray); // [2]
```

`splice`方法可以接受许多参数。

要移除任何索引处的元素，需要给`splice`两个参数:第一个参数是要移除的元素的索引，第二个参数是要移除的元素的数量。

因此，如果你有一个名为`arr`的数组，为了移除索引 4 处的元素，使用`splice`方法的方式应该是:`arr.splice(4, 1)`。

然后，`splice`方法返回一个包含被删除元素的数组。

## 结论

在 JavaScript 中有许多不同的方法来做同样的事情。

在本文中，您学习了从数组中移除元素的九种不同方法。其中六个没有改变原始数组，三个改变了。

你可能会在某个时候用到它们，也许你会学到更多的方法来做同样的事情。

玩得开心！