# 语句 vs 表达式——编程有什么区别？

> 原文：<https://www.freecodecamp.org/news/statement-vs-expression-whats-the-difference-in-programming/>

如果你想有效地使用语言，学习编程语言的语法是关键。对于新的和有经验的开发者来说都是如此。

在学习编程语言时，需要注意的最重要的事情之一是，你要处理的代码是一个语句还是一个表达式。

在编程中区分语句和表达式有时会令人困惑。因此，本文旨在简化这些差异，以便您可以提高编程技能，成为更好的开发人员。

## 什么是编程中的表达式？

![Senior caucasian man holding blank empty banner covering mouth with hand, shocked and afraid for mistake. surprised expression ](img/020ef9f4b7b53475f8e0573f3395c663.png)

Photo by [krakenimages](https://unsplash.com/@krakenimages?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

一个表达式是一个值的任何单词或一组单词或符号。在编程中，表达式是一个值，或者任何执行并最终成为值的东西。

要明白一个值是唯一的。例如，`const`、`let`、`2`、`4`、`s`、`a`、`true`、`false`和`world`是值，因为它们中的每一个都具有独特的含义或特征。

让我们看一些代码作为例子:

```
const price = 500; 
```

从上面的代码来看，`const`、`price`、`=`和`500`都是表达式，因为它们中的每一个都有一个明确且唯一的含义或值。但是如果我们把他们都放在一起`const price = 500`——那么我们就有一个声明。

让我们看另一个例子:

```
let multiply = function (firstNumber, secondNumber) {
    return firstNumber * secondNumber;
} 
```

看上面的代码，你可以看到一个匿名函数被赋给了一个变量。哦，等等！你可能知道任何函数都是一个语句。也可以是一种表达方式吗？

是啊！“函数”和“类”都是语句和表达式，因为它们可以执行动作(执行或不执行任务)并且仍然执行一个值。

这让我们想到了陈述——那么它们是什么呢？

## 编程中的语句是什么？

语句是一组表达式和/或语句，用于执行任务或操作。

陈述是双面的——也就是说，它们要么完成任务，要么不完成任务。任何可以返回值的语句都被自动限定为表达式。这就是为什么函数或类是一个语句，也是 JavaScript 中的一个表达式。

如果您查看表达式部分下的函数示例，您可以看到它被赋值并执行给传递给变量的值。这就是为什么在那种情况下它是一个表达式。

## 编程中的语句示例

### 内嵌语句

```
let amount = $2000; 
```

上面的整个代码是一个语句，因为它执行将`$2000`赋值给`amount`的任务。可以肯定地说，一行代码是一条语句，因为大多数编译器或解释器不执行任何独立的表达式。

![Happy man portraits ](img/640590370c44400bc54c6c5fe1be1cdd.png)

Photo by [Nimi Diffa](https://unsplash.com/@1nimidiffa_?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

### 块语句

看下面的`if`语句:

```
if( iLoveYou ) {
    let status = "you should know I mean it"; 

    console.log(status)
} 
```

`if statement`是一个语句，因为它帮助我们检查`I love you`是否存在。正如我以前说过的，它是双面的:这个代码发现“我爱你”与否，这就是为什么它是一个声明。此外，它不返回任何值，但会产生副作用。

这里有一个`loop`声明:

```
for( ) {
   //code block
}

while ( counter < 5 ) {
   console.log(' less than 5' );
} 
```

简而言之，任何循环都是一个语句，因为如果它只能完成它应该完成或不完成的任务，那么它就会循环和不循环。但是一个循环最终不能执行到一个值。它们只能在 JavaScript 中产生副作用。一旦它们可以执行编程语言中的值，那么它们也可以用作表达式。

例如，可以使用 forloop 和 if 语句作为 Python 中的表达式。

```
# Define a list of numbers
numbers = [1, 2, 3, 4, 5]

# compute the sum of the numbers
total = sum([num for num in numbers])
```

Python 中还有一个“如果”表达式。这意味着在一种语言中是语句的东西在另一种语言中可以是表达式(或者既有语句又有表达式)。

```
a = 1
result = 'even' if a % 2 == 0 else 'odd'
print(result) 
```

请看下面的函数语句:

```
//we define the function as a statement
function add(firstNumber, secondNumber) {
    return firstNumber * secondNumber;
}

//we call it as a statement
add(2, 3); 
```

我们声明函数`add(firstNumber, secondNumber)`，它返回一个值。通过声明用两个参数调用函数，如在`add(2, 3)`中，所以它是一个语句。如果你密切注意，你会意识到调用函数作为一个声明是没有用的，因为它没有副作用。

嘿，停下来！怎么才能把它变成一种表达？哦，是的，我们可以这样做:

```
 const five = add(2, 3) 
```

虽然这个函数现在是一个表达式，但整个代码仍然是一个语句。

看看这个类声明:

```
let User = class Person {
  sayHi() {
    alert("Hi");
  }
}; 
```

你可以看到我们声明了类“Person”,**实例化并立即将**赋给“User”。所以，它被用作一种表达方式。

现在，让我们把它作为一个声明:

```
//We write class as a statement
class Person {
  sayHi() {
    alert("Hi");
  }
}

// we instantiate it as a statement.
new Person().sayHi();

// we instantiate it as an expression
let User = Person(); 
```

类在某种意义上类似于函数，它可以像类一样被声明、赋值或用作操作数。因此，类是一个语句和/或表达式。

## 编程中表达式和语句的主要区别

表达式可以赋值或用作操作数，而语句只能声明。

语句创建副作用是有用的，而表达式是值或执行到值。

表达式在意义上是唯一的，而语句在执行上是双面的。例如，1 具有某个值，而`go( )`可以被执行或不被执行。

语句是整个结构，而表达式是积木。例如，一行或一段代码就是一条语句。

## 为什么你应该知道区别

首先，理解语句和表达式的区别应该会让学习新的编程语言不那么令人惊讶。如果您习惯于 JavaScript，您可能会对 Python 将 If 语句赋值为变量的能力感到惊讶，这在 JavaScript 中是不可能的。

其次，它使得跨不同编程语言使用编程范式变得容易。

例如，JavaScript“if 语句”不能用作表达式，因为它不能执行值——它只能产生副作用。然而，如果您想避免在 JavaScript 中使用 if 语句的副作用，您可以使用三元运算符。

出于这个原因，你可以理解为什么有些程序员在 JavaScript 中使用三元运算符来避免 if 语句。这是因为他们想避免副作用。

这也让你意识到为什么无论何时使用一个语句，你都必须小心变量的范围。这是真的，因为语句通常会有副作用，所以理解变量和操作的范围是合理的。举个例子，

```
let counter = 0;

function addFive() {
    counter += 5
    console.log(counter)
}

function addTwo() {
    counter += 2
    console.log(counter)
}

addFive(counter);// what will this show in the console?
addTwo(counter);// what will this show in the console?
```

嘿等等！如果运行上面的代码，控制台中会记录什么？

先告诉自己答案然后把代码粘贴到控制台里确认。如果你错了，你需要学习更多关于范围和副作用的知识。但是如果你是对的，试着让这些函数更好一点，以避免它们可能产生的混乱。

了解这种差异还有助于您轻松识别编程语言中不可组合和可组合的语法(函数、类、模块等等)。这使得将您的经验从一种编程语言移植到另一种编程语言变得更加有趣和直接。

## **结束**

既然您已经理解了编程中表达式和语句的区别，并且知道了理解这些区别的重要性，那么您就可以在编码时将代码段识别为表达式或语句了。

下一次，我们将更进一步，帮助学习第二种编程语言变得更容易。

现在就去把事情做完！回头见。

我打算在 2023 年分享很多关于编程的技巧和教程。如果你正在努力构建项目，或者你想与我的文章和视频保持联系，请在[YouTube cancode](https://youtoocancode.aweb.page)加入我的列表，或者在[订阅我的 YouTube 频道，你也可以在 YouTube](https://www.youtube.com/c/youtoocancode) 上编码。