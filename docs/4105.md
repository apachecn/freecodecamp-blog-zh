# JavaScript 数组方法的终极指南——Reduce

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-javascript-array-methods-reduce/>

`reduce()`方法将一组值减少到只有一个值。返回的单个值可以是任何类型。

`reduce()`犹如瑞士军刀的阵法。虽然其他的像`map()`和`filter()`提供了特定的功能，但是`reduce()`可以用来将输入数组转换成您想要的任何输出，同时保留原始数组。

## 句法

```
const newValue = arr.reduce(function(accumulator, currentValue, index, array) {
  // Do stuff with accumulator and currentValue (index, array, and initialValue are optional)
}, initialValue);
```

*   `newValue` -返回的新数字、数组、字符串或对象
*   `arr` -被操作的数组
*   `accumulator` -前一次迭代的返回值
*   `currentValue` -数组中的当前项
*   `index` -当前项目的索引
*   `array`-`reduce()`被调用的原始数组
*   `initialValue` -作为最终输出的初始值的数字、数组、字符串或对象

## 例子

### 是五个

```
var numbers = [1, 2, 3]; 

var sum = numbers.reduce(function(total, current) {
  return total + current;
}, 0);

console.log(numbers); // [1, 2, 3]
console.log(sum); // 6
```

### 是六个

```
const numbers = [1, 2, 3];

const sum = numbers.reduce((total, current) => {
  return total + current;
}, 0);

const sumOneLiner = numbers.reduce((total, current) => total + current, 0);

console.log(numbers); // [1, 2, 3]
console.log(sum); // 6
console.log(sumOneLiner); // 6
```

## 关于`initialValue`的一切

### `initialValue`提供

`initialValue`参数是可选的。如果提供，它将在第一次调用回调函数时用作初始累加器值(`total`):

```
const numbers = [2, 3, 4];
const product = numbers.reduce((total, current) => {
  return total * current;
}, 1);

console.log(product); // 24
```

由于在回调函数之后提供了为 1 的`initialValue`，所以`reduce()`从数组的开头开始，并将第一个元素(2)设置为当前值(`current`)。然后，它遍历数组的其余部分，一路更新累加器值和当前值。

### `initialValue`略

如果没有提供`initialValue`，迭代将从数组中的第二个元素(索引 1 处)开始，`accumulator`等于数组中的第一个元素，`currentValue`等于第二个元素:

```
const numbers = [2, 3, 4];
const product = numbers.reduce((total, current) => {
  return total * current;
});

console.log(product);
```

在这个例子中，没有提供`initialValue`，所以`reduce()`将数组的第一个元素设置为累加器值(`total`等于 2)，将数组的第二个元素设置为当前值(`currentValue`等于 3)。然后，它遍历数组的其余部分。

减少字符串数组时:

```
const strings = ['one', 'two', 'three'];
const numberString = strings.reduce((acc, curr) => {
  return acc + ', ' + curr;
});

console.log(numberString); // "one, two, three"
```

如果您的`reduce()`方法将返回一个数字或一个简单的字符串，那么很容易省略`initialValue`参数，如果它将返回一个数组或对象，那么您应该包含一个参数。

## 返回一个对象

将一个字符串数组转换成一个显示每个字符串在数组中出现多少次的对象是很简单的。只需传递一个空对象(`{}`)作为`initialValue`:

```
const pets = ["dog", "chicken", "cat", "dog", "chicken", "chicken", "rabbit"];

const petCounts = pets.reduce(function(obj, pet) {
  if (!obj[pet]) {
    // if the pet doesn't yet exist as a property of the accumulator object,
    //   add it as a property and set its count to 1
    obj[pet] = 1;
  } else {
    // pet exists, so increment its count
    obj[pet]++;
  }

  return obj; // return the modified object to be used as accumulator in the next iteration
}, {}); // initialize the accumulator as an empty object

console.log(petCounts);
/*
{
  dog: 2, 
  chicken: 3, 
  cat: 1, 
  rabbit: 1 
}
*/
```

## 返回 and 数组

一般来说，如果你打算返回一个数组，`map()`通常是一个更好的选择。它告诉编译器(以及阅读您代码的其他人)原始数组中的每个元素都将被转换并作为一个等长的新数组返回。

另一方面，`reduce()`表示原始数组的所有元素都将被转换成一个新值。这个新值可以是一个数组，它的长度可能与原来的不同。

假设您有一个字符串数组形式的购物清单，但是您想从清单中删除所有您不喜欢的食品。你可以用`filter()`过滤掉所有你不喜欢的东西，用`map()`返回一个新的字符串数组，或者你可以只使用`reduce()`:

```
const shoppingList = ['apples', 'mangoes', 'onions', 'cereal', 'carrots', 'eggplants'];
const foodsIDontLike = ['onions', 'eggplants'];
const newShoppingList = shoppingList.reduce((arr, curr) => {
  if (!foodsIDontLike.includes(curr)) {
    arr.push(curr);
  }

  return arr;
}, []);

console.log(newShoppingList); // ["apples", "mangoes", "cereal", "carrots"]
```

这就是你需要知道的关于`reduce()`方法的全部内容。就像瑞士军刀一样，它并不总是这项工作的最佳工具。但是当你真正需要它的时候，你会很高兴把它放在你的后口袋里。