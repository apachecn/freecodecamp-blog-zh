# JavaScript 字符串格式——如何在 JS 中使用字符串插值

> 原文：<https://www.freecodecamp.org/news/javascript-string-format-how-to-use-string-interpolation-in-js/>

ECMAScript 6 (ES6)中增加的模板文字允许我们在 JavaScript 中插入字符串。

更简单地说，我们可以使用占位符将变量注入字符串。您可以在下面的代码片段中看到使用模板文字进行字符串插值的示例:

```
const age = 4.5;
const earthAge = `Earth is estimated to be ${age} billion years old.`;

console.log(earthAge); 
```

首先，你会看到我们对模板文字使用了反勾号。除此之外，我们还有`${placeholder}`的格式，它允许我们在字符串中插入一个动态值。`${}`中的任何内容都被视为 JavaScript。

例如，我们可以写`Earth is estimated to be ${age + 10} billion years old.`，它会像我们写`const age = 4.5 + 10;`一样工作。

### 我们以前是怎么做的？

在模板文字之前，我们应该这样做:

```
const earthAge = "Earth is estimated to be " + age + " billion years old."; 
```

如你所见，我们已经有了很多简单字符串的引号。想象一下我们必须插入一些变量。它可以很快转换成可读性不强的复杂字符串。因此，模板文字使字符串更干净，可读性更好。

然而，这只是一个案例。让我们看看模板文字的更多用例。

## 多行字符串

模板字符串的另一个便利用途是多行字符串。在模板文字之前，我们使用`\n`来表示新行，如在`console.log('line 1\n' + 'line 2')`中。

对于两行，这可能是好的。但是想象一下我们想要五行。同样，字符串变得不必要的复杂。

```
const earthAge = `Earth is estimated to be ${age} billion years old.

Scientists have scoured the Earth searching for the oldest rocks to radiometrically date.

In northwestern Canada, they discovered rocks about 4.03 billion years old.
`; 
```

上面的代码片段再次展示了用模板文字编写多行字符串是多么简单明了。

作为练习，尝试将上面的字符串转换为使用串联和新行`\n`。

## 公式

使用模板文字，我们也可以在字符串中使用表达式。那是什么意思？

例如，我们可以使用数学表达式，比如两个数相乘。下面的片段正好说明了这一点:

```
const firstNumber = 10;
const secondNumber = 39;
const result = `The result of multiplying ${firstNumber} by ${secondNumber} is ${firstNumber * secondNumber}.`;

console.log(result); 
```

如果没有模板文字，我们将不得不这样做:

```
const result = "The result of multiplying " + firstNumber + " by " + secondNumber + " is " + firstNumber * secondNumber + "."; 
```

像上面这样写一个字符串会很快变得复杂和混乱。正如我们一直看到的，模板文字使得在字符串中嵌入表达式变得更加容易和优雅。

## 三元运算符

通过字符串插值，我们甚至可以在字符串中使用三元运算符。这允许我们检查变量的值，并根据该值显示不同的内容。

让我们看看下面的例子:

```
const drinks = 4.99;
const food = 6.65;
const total = drinks + food;

const result = `The total bill is ${total}. ${total > 10 ? `Your card payment will be declined` : `Your card payment will be accepted`}.`

console.log(result); 
```

例如，在上面的例子中，我们检查总金额是否超过十美元。

如果金额超过十，我们让用户知道卡支付将被拒绝。否则，接受卡支付。如你所见，字符串插值允许我们用字符串做很酷的事情。

# 结论

ES6 中增加的模板文字允许我们编写更好、更短、更清晰的字符串。它还让我们能够将变量和表达式注入到任何字符串中。本质上，您在花括号(`${}`)中写的任何内容都被视为 JavaScript。

因此，我们可以使用模板文字来:

*   编写多行字符串
*   嵌入表达式
*   用三元运算符编写字符串

感谢阅读！如果你想保持联系，我们就在 Twitter[@ catalinpit](https://twitter.com/intent/follow?screen_name=catalinmpit)上联系吧。如果你想阅读我的更多内容，我也会定期在我的博客 [catalins.tech](https://catalins.tech) 上发表文章。