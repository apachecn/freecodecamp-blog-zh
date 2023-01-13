# ES6 发电机

> 原文：<https://www.freecodecamp.org/news/es6-generators-47a9c5290569/>

作者:Sanket Meghani

![1*tBXQMulrsKL21K66SVQ5jA](img/c56a96fd207f44d6917c60c582cd8a88.png)

# ES6 发电机

生成器是 ES6 中引入的关键特性之一。与只能在函数开始时进入的普通函数相反，生成器是可以退出并在以后重新进入的函数，其上下文(变量绑定)在重新进入时保存。换句话说，生成器函数可以中途返回值，然后从中途继续执行。

可以使用后跟星号的 function 关键字来定义生成器。

调用普通函数和迭代器函数的区别在于，调用生成器函数不会立即执行生成器函数。而是为生成器返回一个迭代器对象。为了执行生成器主体，我们需要在返回的迭代器上调用 next()方法。

```
let generator = myFirstGenerator(5);let output = generator.next();
```

当迭代器的 next()方法被调用时，生成器函数的主体被执行，直到第一个 yield 表达式。Yield 表达式指定要返回的值。在上面的示例中，调用 generator.next()将执行第一条 console.log()语句，并返回(a + 5)的输出。next()方法返回一个结构如下的对象。

```
{    value: 10, //Return value of yield expression. I.e 5 + 5    done: false //Weather generator has yielded it's last value}
```

我们可以使用返回对象的 value 属性打印返回的产出值。

```
let generator = myFirstGenerator(5);let output = generator.next(); //output = {value: 10, done: false}
```

```
console.log('Output is: ', output.value); //Output is: 10
```

在迭代器上再次调用 next()将从最后一个 yield 表达式继续执行生成器，直到遇到下一个 yield 表达式或 return 语句。

```
output = generator.next(10); //output = {value: 15, done: true}
```

在我们的示例中，调用 generator.next(10)将从第 4 行恢复生成器执行，最后一个 yield 表达式值为 10(即传递给 next()的值)。因此，第 4 行将被评估为 b = 5 + 10，结果为 b = 15。在第 7 行，它返回 b 的值，由于这是最后一个返回语句(即:没有剩余的产出)，done 被设置为 true。

使用参数调用 next()方法将恢复生成器函数的执行，用 next()的参数替换执行暂停处的 yield 语句。

```
output = generator.next(15); //output = {value: 20, done: true}
```

调用 generator.next(15)将从第 4 行继续执行生成器，用 15 替换最后一个 yield 表达式值。因此，第 4 行将被评估为 b = 5 + 15，结果为 b = 20。

我们可以使用 yield*委托给另一个生成器函数。

### **海报**

了解这一新功能在实践中的应用是非常有趣的。 [Redux-saga](http://yelouafi.github.io/redux-saga/) 使用生成器函数来简化异步流的编写。我们只是触及了表面，还有更多。我希望听到您对 ES6 发电机及其使用案例的评论、建议或问题:)。