# 如何用简单的四步法解决编码问题

> 原文：<https://www.freecodecamp.org/news/how-to-solve-coding-problems/>

我还剩 15 分钟，我知道我要失败了。

我花了两个月的时间准备我的第一次技术面试。

我以为我已经准备好了，但当面试接近尾声时，我突然意识到:我不知道如何解决编码问题。

在我学习编码时上过的所有教程中，没有一个包含解决编码问题的方法。

我必须找到解决问题的方法——我作为开发人员的职业生涯依赖于此。

我立即开始研究方法。我找到了一个。事实上，我发现了一个非常有价值的策略。这是一个经过时间考验的四步方法，在某种程度上在开发者生态系统的雷达下。

在这篇文章中，我将回顾这个四步解决问题的方法，你可以用它自信地开始解决编码问题。

解决编码问题不仅仅是开发人员工作面试过程的一部分——它是开发人员每天都要做的事情。毕竟*写代码就是解决问题。*

## 解决问题的方法

这个方法来自乔治·波利亚的《如何解决》一书。它最早于 1945 年问世，销量已超过 100 万册。

他的问题解决方法已经被许多程序员使用和教授，从计算机科学教授(参见大卫·埃文斯教授讲授的 Udacity 的 CS 入门课程)到像柯尔特·斯蒂尔这样的现代 web 开发教师。

让我们用四步解决问题的方法来解决一个简单的编码问题。这允许我们在学习时看到方法的实际应用。我们将使用 JavaScript 作为我们选择的语言。问题是这样的:

创建一个将两个数相加并返回该值的函数。

解题方法有四个步骤:

1.  理解问题。
2.  设计一个计划。
3.  执行计划。
4.  回头看看。

让我们从第一步开始。

## 第一步:了解问题。

当在面试中遇到一个编码问题时，人们很容易会冲进去编码。这是很难避免的，尤其是如果你有时间限制的话。

然而，试着抵制这种冲动。在开始解决问题之前，确保你真正理解了问题。

通读问题。如果你在面试中，你可以大声读出问题，如果这有助于你放慢速度的话。

当你通读这个问题时，澄清你不理解的部分。如果你在面试，你可以问面试官一些关于问题描述的问题。如果你是一个人，仔细思考和/或搜索问题中你可能不理解的部分。

这第一步至关重要，因为我们通常不会花时间去完全理解问题。当你没有完全理解这个问题时，你解决它会困难得多。

为了帮助你更好地理解这个问题，问问你自己:

### 输入是什么？

什么样的输入会进入这个问题？在这个例子中，输入是我们的函数将接受的参数。

仅仅从阅读到目前为止的问题描述，我们就知道输入将是数字。但是为了更具体地了解输入是什么，我们可以问:

输入将总是只有两个数字吗？如果我们的函数接收三个数字作为输入会发生什么？

在这里，我们可以要求面试官澄清，或者进一步查看问题描述。

编码问题可能会有一个注释说，“你应该只期望函数有两个输入。”如果是这样，你知道如何进行。你可以说得更具体，因为你可能会意识到，你需要就你可能会收到的输入类型提出更多问题。

输入将总是数字吗？如果我们收到输入“a”和“b ”,我们的函数应该做什么？澄清我们的函数是否总是接受数字。

或者，您可以在代码注释中写下可能的输入，以了解它们的样子:

`//inputs: 2, 4`

接下来，问:

### 输出是什么？

这个函数将返回什么？在这种情况下，输出将是两个数字输入的结果。确保你明白你的输出是什么。

### 创造一些例子。

一旦你掌握了问题，知道了可能的输入和输出，你就可以开始研究一些具体的例子了。

这些例子也可以用来测试你最终的问题。你将要工作的大多数代码挑战编辑器(无论是在面试中还是仅仅使用像 Codewars 或 HackerRank 这样的网站)都已经为你写好了例子或测试用例。即便如此，写出你自己的例子可以帮助你巩固对问题的理解。

从一两个可能的输入和输出的简单例子开始。让我们回到加法函数。

让我们称我们的函数为“add”

什么是示例输入？示例输入可能是:

`// add(2, 3)`

这个输出是什么？要编写示例输出，我们可以编写:

`// add(2, 3) ---> 5`

这表明我们的函数将接受 2 和 3 的输入，并返回 5 作为输出。

### 创建复杂的示例。

通过浏览更复杂的示例，您可以花时间寻找可能需要考虑的边缘情况。

例如，如果我们的输入是字符串而不是数字，我们该怎么办？如果我们有两个字符串作为输入，例如 add('a '，' b ')，会怎么样？

如果有任何不是数字的输入，你的面试官可能会告诉你返回一个错误信息。如果是这样，您可以添加代码注释来处理这种情况，如果它有助于您记住您需要这样做的话。

```
// return error if inputs are not numbers.
```

你的面试官可能还会告诉你，假设你的输入总是数字，在这种情况下，你不需要编写任何额外的代码来处理这种特殊的输入边缘情况。

如果你没有面试官，你只是在解决这个问题，这个问题可能会告诉你当你输入无效输入时会发生什么。

例如，有些问题会说，“如果有零个输入，则返回未定义的。”对于这种情况，你可以选择写一个注释。

`// check if there are no inputs.`

`// If no inputs, return undefined.`

出于我们的目的，我们假设我们的输入总是数字。但是一般来说，考虑边缘情况是有好处的。

计算机科学教授埃文斯说要编写开发者所说的防御型代码。考虑什么可能出错，以及您的代码如何防范可能的错误。

在进入第 2 步之前，让我们总结一下第 1 步，了解问题:

`-Read through the problem.`

`-What are the inputs?`

`-What are the outputs?`

`Create simple examples, then create more complex ones.`

## 2.设计一个解决问题的计划。

接下来，设计一个你将如何解决问题的计划。当你设计一个计划时，用伪代码写出来。

伪代码是算法步骤的简单语言描述。换句话说，你的伪代码就是你如何解决问题的一步一步的计划。

写出你需要采取的解决问题的步骤。对于更复杂的问题，你会有更多的步骤。对于这个问题，你可以这样写:

`// Create a sum variable.`

`Add the first input to the second input using the addition operator`。

`// Store value of both inputs into sum variable.`

`// Return as output the sum variable.`

现在你有了自己解决问题的循序渐进的计划。埃文斯教授指出，对于更复杂的问题，“系统地考虑人类如何解决问题。”也就是说，暂时忘记你的代码可能如何解决问题，想想作为一个人，你会如何解决这个问题。这可以帮助你更清楚地看到步骤。

## 3.执行计划(解决问题！)

![Hand, Rubik, Cube, Puzzle, Game, Rubik Cube](img/43b9a51e5392f49121a0fe487f8cfcef.png)

解决问题策略的下一步是解决问题。使用你的伪代码作为指导，写出你的实际代码。

埃文斯教授建议专注于一个简单的、机械的解决方案。你的解决方案越简单，你就越有可能正确地编程。

拿我们的伪代码来说，我们现在可以这样写:

```
function add(a, b) {
 const sum = a + b;
 return sum;
}
```

埃文斯教授补充道，记住不要过早优化。也就是说，你可能会忍不住开始说，“等等，我正在做这个，这将是低效的代码！”

首先，拿出你简单机械的解决方案。

如果你不能解决整个问题呢？如果有一部分你还不知道怎么解决呢？

柯尔特·斯蒂尔在这里给出了很好的建议:如果你不能解决问题的一部分，忽略那些让你犯错的困难部分。相反，把注意力放在你可以开始写的其他事情上。

暂时忽略问题中你不太理解的困难部分，写出其他部分。一旦这个完成了，回到更难的部分。

这允许你至少完成问题的一部分。通常，当你回头来看问题时，你会意识到如何解决问题的更难的部分。

## 第四步:回顾你所做的事情。

一旦你的解决方案起作用了，花点时间思考一下，找出如何改进的方法。这可能是您将解决方案重构为更有效的解决方案的时候了。

当你审视自己的工作时，柯尔特·斯蒂尔建议你问自己一些问题，看看如何改进你的解决方案:

*   你能得出不同的结果吗？还有其他可行的方法吗？
*   一眼就能看懂吗？有意义吗？
*   你能用这个结果或方法解决其他问题吗？
*   您能提高解决方案的性能吗？
*   你能想到其他重构的方法吗？
*   其他人是如何解决这个问题的？

我们可以重构问题，使代码更简洁的一种方法是:删除变量，使用隐式返回:

```
function add(a, b) {
 return a + b;
}
```

第四步，你的问题可能永远不会结束。即使是伟大的开发人员仍然会写一些他们以后会看到并想要修改的代码。这些是可以帮助你的引导性问题。

如果面试还有时间，可以走完这一步，让自己的解决方案更好。如果你是自己编写代码，花点时间仔细检查这些步骤。

当我独自练习编码时，我几乎总是会寻找比我想出的更优雅或更有效的解决方案。

## 包扎

在这篇文章中，我们回顾了解决编码问题的四步解决策略。

让我们在这里回顾一下:

*   第一步:**了解问题。**
*   第二步:**为你如何解决这个问题制定一个分步计划**。
*   第三步:**执行计划**，编写实际代码。
*   第四步:**回顾**，如果你的解决方案可以做得更好的话，就可能重构它。

实践这种解决问题的方法对我的技术面试和开发工作有很大的帮助。如果你对解决编码问题没有信心，请记住，解决问题是一项技能，任何人都可以通过时间和练习变得更好。

祝你好运！

### 如果你喜欢这篇文章，加入我的[编码俱乐部](https://madisonkanna.us14.list-manage.com/subscribe/post?u=323fd92759e9e0b8d4083d008&id=033dfeb98f)，在那里我们每周日一起应对编码挑战，并在学习新技术时相互支持。

### 如果你对这篇文章有任何反馈或问题，欢迎发微博给我。