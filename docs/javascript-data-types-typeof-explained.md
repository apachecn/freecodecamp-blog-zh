# JavaScript 数据类型:解释的类型

> 原文：<https://www.freecodecamp.org/news/javascript-data-types-typeof-explained/>

`typeof`是一个 JavaScript 关键字，当你调用它时会返回变量的类型。您可以使用它来验证函数参数或检查变量是否已定义。还有其他的用途。

操作符`typeof`很有用，因为它是检查代码中变量类型的一种简单方法。这很重要，因为 JavaScript 是一种动态类型语言。这意味着您不需要在创建变量时为它们分配类型。因为变量不受这种方式的限制，所以它的类型可以在程序运行时改变。

例如:

```
var x = 12345; // number
x = 'string'; // string
x = { key: 'value' }; // object
```

从上面的例子可以看出，JavaScript 中的变量可以在程序执行过程中改变类型。作为程序员，这可能很难理解，这就是`typeof`操作符有用的地方。

`typeof`操作符返回一个表示变量当前类型的字符串。你可以通过输入`typeof(variable)`或`typeof variable`来使用它。回到前面的例子，您可以使用它来检查每个阶段的变量`x`的类型:

```
var x = 12345; 
console.log(typeof x) // number
x = 'string'; 
console.log(typeof x) // string
x = { key: 'value' };
console.log(typeof x) // object
```

这有助于检查函数中变量的类型，并根据需要继续。

下面是一个函数的例子，它可以接受一个字符串或数字变量:

```
function doSomething(x) {
  if(typeof(x) === 'string') {
    alert('x is a string')
  } else if(typeof(x) === 'number') {
    alert('x is a number')
  }
}
```

`typeof`操作符的另一个用处是确保在代码中访问变量之前先定义它。这有助于防止在试图访问未定义的变量时程序中可能出现的错误。

```
function(x){
  if (typeof(x) === 'undefined') {
    console.log('variable x is not defined');
    return;
  }
  // continue with function here...
}
```

当您检查一个数字时，`typeof`操作符的输出可能不总是您所期望的。
数字可以转到值 [NaN(不是数字)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)有多种原因。

```
console.log(typeof NaN); //"number"
```

也许你试图用一个对象乘一个数，因为你忘了访问对象内部的数。

```
var x = 1;
var y = { number: 2 };
console.log(x * y); // NaN
console.log(typeof (x * y)); // number
```

当检查一个数字时，仅仅检查`typeof`的输出是不够的，因为`NaN`和
都通过了这个测试。
这个函数检查数字，也不允许`NaN`值通过。

```
function isNumber(data) {
  return (typeof data === 'number' && !isNan(data));
}
```

即使这是一种有用的验证方法，我们也必须小心，因为 javascript 有一些奇怪的部分，其中之一是由特定指令上的`typeof`导致的。例如，在 javascript 中，许多东西只是`objects`，所以你会发现。

```
var x = [1,2,3,4]; 
console.log(typeof x)  // object

console.log(typeof null)  // object
```