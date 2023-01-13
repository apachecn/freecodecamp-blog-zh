# 让'我'成为' const '(蚂蚁)，而不是' var '(可变量)！

> 原文：<https://www.freecodecamp.org/news/let-me-be-a-const-ant-not-a-var-iable-1be52d153462/>

斯里什蒂·古普塔

# 让'我'成为' const '(蚂蚁)，而不是' var '(可变量)！

`var`、`let`和`const`是 JavaScript 中用来声明变量的关键字。`var`属于 ECMAScript 第五版(又名 ES5)，而`let`和`const`属于 ECMAScript 第六版(又名 ES6 和 ES2015)。

因为 JavaScript 没有任何类型检查，所以这两个关键字都可以用来声明 JavaScript 中任何类型(数据类型)的变量。

虽然这三个关键字的用途相同，但它们是不同的。

### let 与 const:

#### **未来值的变化**

> 使用`let`关键字声明的变量可以在将来改变它们的值。

考虑下面给出的例子:

```
let iChange = 11;iChange = 12;
```

```
console.log(iChange);
```

**输出:**

```
12
```

在第一行，变量`iChange`使用`let`关键字声明，并用值 11 初始化。当你进入下一行时，变量`iChange` 再次被赋予一个新值，即 12。允许更改使用`let`关键字声明的变量值。在最后一行，当您试图打印变量`iChange`的值时，您会正确地得到更新后的值 12。

> 使用`const`关键字声明的变量将来不能改变它们的值。这就是为什么你必须总是初始化用关键字`const`声明的变量。

```
const PI = 3.14;PI = 22/7;
```

```
console.log(PI);
```

**输出:**

```
Uncaught TypeError: Assignment to constant variable
```

这里，变量`PI`是使用`const`关键字声明的，并在第一行用值`3.14`初始化。当您进入下一行时，变量`PI`被更新为一个新值`22/7`。不允许改变使用`const`关键字声明的变量值。这就是为什么第二行会抛出输出中显示的错误，因为您试图给常量变量赋一个新值。因此，记住使用`const`关键字声明的变量是只读的&不能被重新赋值。

如前所述，在声明常量时，必须对其进行初始化。让我们看看下面这段代码:

```
const PI;
```

**输出:**

```
Uncaught SyntaxError: Missing initializer in const declaration
```

你知道你不能在将来更新一个常数。如果你在声明一个常量时没有初始化它，你将永远不能给它赋值！这就是为什么当你保留一个未初始化的常量时会得到一个`SyntaxError`。

你认为下面给出的代码的输出会是什么？

```
const passengerBus = {wheels: 8, passengers: 40}passengerBus.passengers = 50;
```

```
console.log(passengerBus);
```

你认为会抛出错误吗？让我们检查输出。开始了。

**输出:**

```
{ wheels: 8, passengers: 50 }
```

> 使用`*const*`关键字声明的对象(包括数组和函数)是可变的。

到目前为止，您已经了解了使用`const`关键字声明的变量不能被赋予任何其他值。虽然这是真的，但故事还有另一面。毫无疑问，您不能给一个常量赋值，但是如果它是一个对象或数组，您可以操作现有的值。

在上面给出的代码中，您并没有改变分配给常量`passengerBus`的整个值，而是操作了其中的一个属性。您可以添加/删除/更新使用`const`关键字声明的对象中的属性。

对数组也可以做类似的事情。您可以在使用`const`关键字声明的数组中添加/删除/更新元素。

```
const android = ['Marshmallow', 'Noughat', 'Oreo'];arr[3] = 'Pie'; // adding new version
```

```
console.log(android);
```

**输出:**

```
['Marshmallow', 'Noughat', 'Oreo', 'Pie']
```

现在，考虑到关键字`let`和`const`属于同一类别，以下几点列举了使用`let` / `const`关键字声明的变量和使用`var`关键字声明的变量之间的差异:

### let '(或' const ')与' var ':

#### **范围**

> 使用`let` / `const`关键字声明的变量是**块范围的**。

考虑下面的例子:

```
function foo() {   for (let i = 0; i < 3; i++) {      console.log(i); // statement 1   }   console.log(`All eyes here please: ${i}`); // statement 2}
```

```
foo();
```

**输出:**

```
012Uncaught ReferenceError: i is not defined
```

变量 *i* 是在 for 循环块中使用`let`关键字声明的。这意味着当 for 循环块结束时，变量 *i* 失去了它的作用域，并且在 for 循环块的花括号外不再可访问。因此，当您试图访问变量 *i* 并将它的值打印在语句 2 上时，您会得到一个`ReferenceError: i is not defined`，如输出所示。

考虑另一个使用`const`关键字声明变量的例子:

```
function placeOrder(status) {   if (status) {      const message = "Order placed successfully!";      console.log(message); // statement 1   }   console.log(message); // statement 2}
```

```
placeOrder(true);
```

**输出:**

```
Order placed successfullyUncaught ReferenceError: message is not defined
```

变量`message` 在 if 块中使用`const`关键字声明。这意味着当 if 块结束时，变量`message` 失去了作用域，并且在 if 块的花括号之外不再可访问。这就是为什么当您试图访问变量`message` 并在语句 2 中打印它的值时，您会得到一个`ReferenceError: message is not defined`，如输出所示。

> 使用`var`关键字声明的变量是**函数作用域的**。

考虑我们之前讨论过的例子，在这个例子中，不使用`let`，而是使用`var`关键字来声明变量 *i* :

```
function foo() {   for (var i = 0; i < 3; i++) {      console.log(i); // statement 1   }   console.log(`All eyes here please: ${i}`); // statement 2}
```

```
foo();
```

**输出:**

```
012All eyes here please: 3
```

变量 *i* 是在 for 循环块中使用`var`关键字声明的。因为使用`var`关键字声明的变量是函数范围的，所以当 for 循环块结束时，变量 *i* 不会超出范围，并且可以在函数`foo`范围内的任何地方访问。因此，在语句 2 中，当您试图访问变量 *i* 并打印其值时，您会得到正确的输出结果`3`(在 for 循环的 increment 语句执行后，变量 *i* 的递增值)，如输出所示。

#### 重新申报

> 使用`*let*` / `*const*` 关键字**声明的变量不能在同一个作用域内重新声明**。

你认为下面代码的输出会是什么？

```
let avengers = 'Infinity War';let avengers = 'Endgame';
```

```
console.log(avengers);
```

**输出:**

```
Uncaught SyntaxError: Identifier 'avengers' has already been declared
```

在上面的代码中，您使用关键字`let`声明了一个名为`avengers`的变量，然后在下一行再次声明了它。因此，第二行抛出一个输出中提到的`SyntaxError`。

> 使用`var` 关键字**声明的变量可以在同一个范围内重新声明**。

现在让我们使用关键字`var`声明一个先前已经在同一个作用域中声明的变量。

```
var avengers = 'Infinity War';var avengers = 'Endgame';
```

```
console.log(avengers);
```

**输出:**

```
Endgame
```

从输出中可以明显看出，您可以使用`var`关键字在相同的范围内重新声明具有相同名称的变量。变量中包含的值将是您分配给它的最终值。

#### **吊装**

> 使用`*let*` / `*const*` 关键字声明的变量是**不加**。

这是一个被许多人遗忘的要点，你不会在所有文章中都找到它。为了理解这一点意味着什么，考虑下面给出的例子:

```
console.log(x);let x = 10;
```

**输出:**

```
Uncaught ReferenceError: x is not defined
```

注意，在上面给出的代码的第一行，您试图访问一个变量 *x* ，该变量在下一行被声明并赋值。本质上，你试图访问一个变量，这个变量还没有被分配内存(声明)。由于变量 *x* 是使用`let`关键字声明的，而使用`let` / `const`关键字声明的变量没有被提升，这就抛出了一个`ReferenceError: x is not defined`，如输出所示。

> 使用`var`关键字声明的变量被**提升到其作用域**的顶部。

```
console.log(x);var x = 10;
```

**输出:**

```
undefined
```

所有声明都被移动到范围的顶部。注意，在第一行，您试图访问一个变量 *x* ，该变量在下一行被声明并赋值。现在，由于变量 *x* 是使用`var`关键字声明的，并且使用`var`关键字声明的变量在 JavaScript 中被提升到其作用域的顶部，所以代码被转换为下面给出的代码:

```
var x;console.log(x);x = 10;
```

这里，变量 *x* 在第 1 行声明，没有赋值。如果用户没有明确指定其他值，JavaScript 中的所有变量都用默认值`undefined`初始化。因此，`x`被赋值为`undefined`，这是打印在第二行的内容(在 x 更新为 10 之前)。

#### 更大的问题是——更喜欢什么？

今天，几乎所有的浏览器都支持 ES6(又名 ES2015)。如果您能遵循这种语法，建议使用`let`和`const`关键字来声明代码中的所有变量。

现在，在`let`和`const`中选择哪一个？标题说明了一切。

![rj5mGjbr0EVJzlpIYxLgMluer6heMLZSsG0i](img/b8e95d9fa9fb27db4fb2964758503089.png)