# 递归生成器以及如何使用它们不占用所有内存

> 原文：<https://www.freecodecamp.org/news/recursive-generator-f8bc30e5e412/>

不久前，我写了一篇关于组合学的文章。那篇文章的部分代码使用了一个[组合器](https://gist.github.com/JeffML/0cee0d09d32347ea95e0f9cb4f851cd8)对象，它生成选择的组合并将它们存储在一个数组中。

组合运算的问题是，随着每一个额外选择的增加，组合的数量[会爆炸式地快速增长](https://en.wikipedia.org/wiki/Combinatorial_explosion)——在某些情况下，比指数增长还要快。

如果我有三个项目，并允许选择其中的 0、1、2 或 3 个，如果我**不考虑顺序，不允许重复，并包括空集**，我会得到 8 个唯一的选择。增加一倍到六项，你会有 64 个选择(8*8)。再翻一倍(12 项)，有 4096 个选择(64*64)。在这种情况下，由于上面提到的限制，组合的数量是 2 的 n 次方个选择，所以它只增长(！)指数级。

对于大量的条目，将每个组合存储在一个数组中可能会导致内存耗尽。不是让组合器在所有组合生成后才返回一个数组，而是根据需要逐个返回每个组合，这样如何？既然组合子是由*生成*个组合，那么它是否可以转化为一个[生成器](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)？

### 原创 Combinator.js

在原始代码中，通过调用 **combine()** 创建的每个组合都存储在一个**组合**数组中:

```
var Combinator = function (opts) {
    var combinations = [];

    function combine(current, remainder) {
        if (remainder.length === 0) {
            if (current.length >= (opts.min || 0) &&
                current.length <= (opts.max || current.length))
                combinations.push(current);
        } else {
            combine(current.concat(remainder[0]), remainder.slice(1, remainder.length));
            combine(current, remainder.slice(1, remainder.length));
        }
        return this;
    }
    return {
        combinations: combinations,
        combine: combine
    }
}

module.exports = Combinator;
```

该算法增加了最小/最大选项，限制了包含最少**最小**，最多**最大**元素的组合数量。我可以这样使用:

```
var menu = {
   threeItems: {
        min: 0,
        max: 3,
        values: [1, 2, 3]
    }
}

var threeCombos = new Combinator({
            min: menu.threeItems.min,
            max: menu.threeItems.max
        })
        .combine([], menu.threeItems.values)
        .combinations;
```

**menu.threeItems.values** 属性有(惊喜！)三个价值观。**最小**和**最大**属性决定了要生成的组合集。在这种情况下，我们要求从 0 长度(空集)到完整长度(整个值集)的集合。请记住，我们对顺序不感兴趣，也不允许重复。让我们来看看它的实际应用:

```
console.log('threeCombos.length =', threeCombos.length, threeCombos);

-- output --

threeCombos.length = 8 [ [ 1, 2, 3 ], [ 1, 2 ], [ 1, 3 ], [ 1 ], [ 2, 3 ], [ 2 ], [ 3 ], [] ]
```

现在，我们不使用数组来存储所有组合，而是将这段 JavaScript 代码转换成使用新的 ES6 生成器功能。生成器是一个有状态的函数，它以迭代的方式一个接一个地产生值。

### 天真的尝试

使用**函数*** 而不是**函数来声明生成器函数。**在生成器函数中调用 **yield** 操作符，将单个值返回给调用者。生成器会记住前一个调用的状态，因此后续的 **yield** s 将返回下一个逻辑值。调用者使用 **next()** 方法从生成器函数中获取每个后续值。不需要数组！

![1*TEk49bwXt313Cj-_aCL4Bg](img/87752dea92688886a4879e8a4977f3a4.png)

我有时会很懒，所以我选择了 TL；dr 接近关于生成器的 JavaScript [文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*),并对其进行处理。第一次尝试是:

```
var CombinatorGenerator = function (opts) {
    function* combine(current, remainder) {
        if (remainder.length === 0) {
            if (current.length >= (opts.min || 0) &&
                current.length <= (opts.max || current.length)) {
                yield(current);
            }
        } else {
            combine(current.concat(remainder[0]), remainder.slice(1, remainder.length))
            combine(current, remainder.slice(1, remainder.length))
        }
    }
    return {
        combine: combine
    }
}
```

这是有道理的吧？我没有将一组选择推送到一个数组中，而是给出一个值。在客户端代码中，我一直调用 next()，直到生成器告诉我它完成了。

```
var menu = require('./menu');
var Combinator = require('./Combinator-generator-naive');

function run() {
    var threeCombos = new Combinator({
            min: menu.threeItems.min,
            max: menu.threeItems.max
        })
        .combine([], menu.threeItems.values);

    for (;;) {
        var it = threeCombos.next();
        if (it.done) {
            console.log("done!")
            break;
        }
        console.log("choice", it.value);
    }
}

run();
```

唉，我的希望破灭了。输出是:

```
PS C:\Users\Jeff\workspace\Generator> node .\test-generated.js

done!
```

好了，显然新的组合子在第一个收益之前返回了，所以我们“完成了！”在我们真正完成之前。

### 直觉尝试

仍然讨厌阅读文档，我下一步尝试凭直觉修复错误。那么，如果我只是从内部的**组合**调用中让步，会发生什么——逻辑上，不是吗？而不是:

```
} else {
            combine(current.concat(remainder[0]), remainder.slice(1, remainder.length))
            combine(current, remainder.slice(1, remainder.length))
        }
```

我尝试放弃递归调用:

```
} else {
   yield combine(current.concat(remainder[0]), remainder.slice(1, remainder.length)).next()
   yield combine(current, remainder.slice(1, remainder.length)).next()
}
```

真的，这是可行的。让我们运行它:

```
PS C:\Users\Jeff\workspace\Generator> node .\generated.js
choice { value: { value: { value: [Object], done: false }, done: false },
  done: false }
choice { value: { value: { value: [Object], done: false }, done: false },
  done: false }
done!
```

嗯……这不好——返回的是递归生成器的状态，而不是来自 **yield** 操作的实际值。

### 深思熟虑的尝试

好了，该认真了。在“递归生成器”上稍微搜索一下，就会找到 Python 的 **yield from。该语法将产出调用委托给另一个生成器。JavaScript 中有对等的吗？**

是啊！—这是 **yield*** 语法。这其实是在关于[发电机](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)的文档链接里；如果我读了这本书，我可能会更早发现这一点(懒惰，就像犯罪一样，并不总是值得的)。正确的语法是:

```
} else {
            yield* combine(current.concat(remainder[0]), remainder.slice(1, remainder.length))
            yield* combine(current, remainder.slice(1, remainder.length))
        }
```

现在，当我调用**合并**方法时，我看到:

```
node .\generated.js
choice [ 1, 2, 3 ]
choice [ 1, 2 ]
choice [ 1, 3 ]
choice [ 1 ]
choice [ 2, 3 ]
choice [ 2 ]
choice [ 3 ]
choice []
done!
```

很好！我一个接一个地找回所有的组合。成功！

这篇文章中使用的全部代码可以在这里找到[。快乐发电！](https://github.com/JeffML/Generator)

***2017 年 2 月 26 日更新***

在阅读了不知疲倦的 Eric Elliott 的这篇文章后，我开始认为我已经把一种资源枯竭(内存)换成了另一种资源枯竭(堆栈)。然而，我用长度为 30 的输入数组运行了组合子，它运行完成了:生成了 2 个⁰组合(超过 10 亿)。注意，该算法

1.  没有使用尾部递归(或者可能是‘分尾’递归？);和
2.  **yield *** ，根据 Eric 的文章，在任何情况下都不应该优化为尾部递归调用

然而，它是有效的。在 git 存储库中运行 generated30.js 可以找到这篇文章的证据。