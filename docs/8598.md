# JavaScript ES6 函数:好的部分

> 原文：<https://www.freecodecamp.org/news/es6-functions-9f61c72b1e86/>

布万·马利克

# JavaScript ES6 函数:好的部分

![P-RQTFiCQPKUDGiSPTvlGT4tQsRWTyHHryS3](img/41d5d9b2a1c95a985c2e65ae2cec7cbb.png)

ES6 提供了一些很酷的新功能特性，使得 JavaScript 编程更加灵活。让我们来讨论其中的一些——具体来说，默认参数、rest 参数和箭头函数。

**有趣的提示**:你可以将这些例子/代码复制粘贴到[巴别 REPL](https://babeljs.io/repl/) 中，你可以看到 ES6 代码是如何转换成 ES5 的。

![NIpmekw9kO1zjEdmWmnWAvnmoFQ8Eh96Jcgk](img/eeefdb71d270f4f2ca61b9e25333ed8a.png)

### **默认参数值**

JavaScript 函数有一个独特的特性，允许您在函数调用期间传递任意数量的参数(实际参数)，而不管函数定义中存在多少个参数(形式参数)。因此，您需要包含默认参数，以防有人忘记传递其中一个参数。

#### **如何在 ES5 中实现默认参数**

当我们运行它时，上面看起来很好。`number2`没有被传递，如果第一个操作数为 falsy，我们使用“`||`”操作符返回第二个操作数。因此，“10”被指定为默认值，因为`number2`未定义。

不过，还有一个问题。如果有人将' 0 '作为第二个参数传递呢？⚠

上面的方法会失败，因为我们的默认值' 10 '将被分配，而不是传递的值，如' 0 '。为什么？因为“0”被评估为 falsy！

让我们改进上面的代码，好吗？

啊！代码太多了。一点都不酷。我们来看看 ES6 是怎么做到的。

#### **ES6 中的默认参数**

```
function counts(number1 = 5, number2 = 10) {  // do anything here}
```

`number1`和`number2`分别被赋予默认值‘5’和‘10’。
嗯，是的，差不多就是这样。又短又甜。没有额外的代码来检查丢失的参数。这使得函数的主体简洁明了。？

注意:当一个值`undefined`被传递给一个带有默认实参的参数时，正如所料，传递的值是**无效**并且**默认参数值被赋值**。但是，如果通过了`null`，则认为**有效**，并且不使用**默认参数**，并且将 null 分配给该参数。

默认参数的一个很好的特性是，默认参数不一定必须是一个原始值，我们也可以执行一个函数来检索默认参数值。这里有一个例子:

前面的参数也可以是后面的参数的默认参数，如下所示:

```
function multiply(first, second = first) {  // do something here}
```

但是反过来会抛出一个错误。也就是说，如果将第二个参数指定为第一个参数的默认值，则会导致错误，因为第二个参数在被指定给第一个参数时尚未定义。

```
function add(first = second, second) {  return first + second;}console.log(add(undefined, 1)); //throws an error
```

### 休息参数

> 一个 *rest* 参数是一个简单的命名参数，前面有三个点(…)。这个命名参数变成了一个数组，其中包含了函数调用过程中传递的其余参数(即命名参数之外的参数)。

请记住，只能有一个 *rest* 参数，并且必须是最后一个参数。rest 参数后不能有命名参数。
这里有一个例子:

正如您所看到的，我们使用 rest 参数从传递的对象(第一个参数)中提取所有的键/属性。

rest 参数和“arguments object”之间的区别在于，后者包含函数调用期间传递的所有实际参数，而“rest 参数”只包含函数调用期间传递的非命名参数。

### 箭头功能➡

![9n5l6egKPj2FuvrmoTHpTfNcS0jRzOa-0Ap5](img/e9f670f18503dbf7f35270b44c63b376.png)

箭头函数或“胖箭头函数”引入了一种非常简洁的定义函数的新语法。我们可以避免键入像`function`、`return`这样的关键词，甚至是花括号`{ }`和圆括号`()`。

#### 句法

根据我们的用法，语法有不同的风格。所有的变奏都有一个共同点**，即它们都以**论元**开始，然后是****箭头**(`=&`gt；)，后面是 t **he 函数 b** ody。****

****论点和正文可以有不同的形式。下面是最基本的例子:****

```
**`let mirror = value => value;`**
```

```
**`// equivalent to:`**
```

```
**`let mirror = function(value) {  return value;};`**
```

****上面的例子采用了一个参数“value”(在箭头之前)，并简单地返回该参数(`=> val`UE；).正如你看到的**，只有返回值，所以不需要返回关键字或卷曲胸罩** ces 包装函数体。****

****因为只有**个参数**，所以**也不需要括号**“()”。****

****这里有一个不止一个参数的例子来帮助你理解这一点:****

```
**`let add = (no1, no2) => no1 + no2;`**
```

```
**`// equivalent to:`**
```

```
**`let add = function(no1, no2) {  return no1 + no2;};`**
```

****如果根本没有参数，则必须包括空括号，如下所示:****

```
**`let getMessage = () => 'Hello World';`**
```

```
**`// equivalent to:`**
```

```
**`let getMessage = function() {  return 'Hello World';}`**
```

****对于只有一个返回语句的函数体，花括号是可选的。
对于一个不仅仅有返回语句的函数体，你需要像传统函数一样用花括号把函数体括起来。****

****这是一个简单的计算函数，有两个操作——加和减。它的主体必须用花括号括起来:****

```
**`let calculate = (no1, no2, operation) => {    let result;    switch (operation) {        case 'add':            result = no1 + no2;            break;        case 'subtract':            result = no1 - no2;            break;    }    return result;};`**
```

****如果我们想要一个简单返回对象的函数呢？编译器会搞不清楚花括号是对象的花括号(`()=&**g**t;{id: num**b**` er})还是函数体的花括号。****

****解决方法是用括号将对象括起来。方法如下:****

```
**`let getTempItem = number =&gt; ({ id: number});`**
```

```
**`// effectively equivalent to:`**
```

```
**`let getTempItem = function(number) {    return {        id: number    };};`**
```

****让我们看看最后一个例子。在这个示例中，我们将对一个数字数组使用 filter 函数来返回所有大于 5，000 的数字:****

```
**`// with arrow functionlet result = sampleArray.filter(element => element > 5000);`**
```

```
**`// without arrow functionlet result = sampleArray.filter(function(element) {  return element > 5000;});`**
```

****我们可以看到，与传统函数相比，所需的代码要少得多。****

****关于箭头函数，有几件事需要记住:****

*   ****它们不能用 *new* 调用，不能用作构造函数(因此也缺少原型)****
*   ****箭头函数有自己的作用域，但是没有自己的' *this'* 。****
*   ****没有可用的*参数*对象。不过你**可以使用**休息参数。****

****由于 JavaScript 将函数视为一级函数，箭头函数使得编写 lambda 表达式和 currying 之类的函数代码变得更加容易。****

> ****"箭头函数就像是 JavaScript 中函数式编程爆炸的火箭燃料."—埃里克·埃利奥特****

****好吧，这就对了！也许是时候开始使用这些功能了。****

****像这样的 ES6 特性就像一股清新的空气，开发者喜欢使用它们。****

****![fRbQopusRfJej7QpnRe6umezaPKn-cWs6vgO](img/27214515cc2008824eef0d7456e73ace.png)****

****这里是[链接](https://medium.com/@bhuvanmalik/what-is-variable-hoisting-differentiating-between-var-let-and-const-in-es6-f1a70bb43d)到我之前关于变量声明和提升的帖子！
如果你还没有接受 ES6，我希望这能激励你接受它！****

****和平****