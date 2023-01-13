# JavaScript 模块——初学者指南

> 原文：<https://www.freecodecamp.org/news/javascript-modules-beginners-guide/>

JavaScript 模块(也称为 es 模块或 ECMAScript 模块)是为了帮助 JavaScript 代码更有组织性和可维护性而创建的。

理解 ES 模块如何工作将有助于你成为一名更好的 JavaScript 开发人员。在本文中，我们将讨论:

*   [什么是模块？](#module)
*   [什么是 ES 模块？我们为什么要使用它们？](#es-modules)
*   [如何使用 ES 模块](#how-to-use)
*   [JavaScript 中使用的其他模块系统](#other)

让我们开始吧。

# 什么是模块？

JavaScript 中的模块只是一个代码文件。您可以将模块视为可重用的独立代码单元。

模块是代码库的构建块。随着应用程序变得越来越大，您可以将代码分成多个文件，也就是模块。

使用模块可以让你将大型程序分解成更易管理的代码。

# 什么是 ES 模块？我们为什么要使用它们？

ES 模块是 JavaScript 中使用的官方模块系统。JavaScript 中也可以使用其他模块系统，我们将在后面详细讨论。但是现在，我们正在学习 ES 模块而不是其他模块系统，因为它们是 JavaScript 中的标准模块。

作为一名 JavaScript 开发人员，您可能会在日常工作中使用 ES 模块。

以下是开发人员从使用 es 模块中获得的一些优势:

1.  **组织。通过将大的程序分解成更小的相关代码，你可以保持你的程序有条理。**
2.  **可重用性。使用 ES 模块，您可以在一个地方编写代码，并在整个代码库中的其他文件中重用这些代码。例如，您可以在一个模块中编写一个函数，然后将其导入到另一个文件中并在那里使用，而不是到处重写同一个函数。**

让我们深入一个使用 ES 模块的例子。我们将了解 ES 模块是如何工作的，以便您可以在以后的项目中使用它们。当我们使用 ES 模块时，我们将看到上述每个优势的展示。

# 如何使用 ES 模块

让我们从创建一个普通的 JavaScript [Replit](https://replit.com/) 开始。你也可以在这里找到完整的代码。在 Replit 上，我们可以创建一个新项目并选择 HTML、CSS 和 JavaScript。这将创建一个包含一个`index.html`文件、一个`script.js`文件和一个`style.css`文件的起始项目。这是我们准备好的所有东西。

在我们的 index.html 文件中，我们将修改我们的脚本标签以包含`type="module"`。这将允许我们开始在代码中使用 ES 模块。将您的脚本标记修改为:

```
<script type="module" src="script.js"></script>
```

让我们从编写一个简单的 add 函数开始。这个函数将两个数相加，然后返回相加的结果。我们也会调用这个函数。我们将在我们的`script.js`文件中编写这个函数:

```
function add(a, b) {
 return a + b;
};
console.log(add(5, 5)); //outputs 10
```

到目前为止，我们的`script.js`文件很小，只有很少的代码。但是想象一下，这个应用程序越来越大，我们有几十个这样的函数。这个`script.js`文件可能会变得太大，变得难以维护。

让我们通过创建一个模块来避免这个问题。我们可以通过点击回复列表中的“添加文件”来完成。记住，模块只是一个相关代码的文件。

我们称我们的模块为`math.js`。我们将从我们的`script.js`文件中删除这个 add 函数，我们将创建一个新文件`math.js`。这个文件将是我们的模块，我们将在这里保存我们的数学相关的函数。让我们将 add 函数放在这个文件中:

```
// math.js

function add(a, b) {
 return a + b;
};
```

我们决定将这个模块命名为`math.js`，因为稍后我们将在这个文件中创建更多与数学相关的函数。

如果我们打开这个应用程序一看就知道，我们的数学相关逻辑就在这个文件中。我们不需要浪费时间进入这个应用程序，搜索我们的数学函数，并想知道它们在哪里——我们已经将它们整齐地组织到一个文件中。

接下来，让我们在`script.js`文件中使用 add 函数，尽管函数本身现在位于`math.js`文件中。为此，我们需要了解 ES 模块语法。让我们复习一下`export`和`import`关键词。

## export 关键字

当您想让一个模块在它所在的文件之外的其他文件中可用时，您可以使用`export`关键字。让我们在 add 函数中使用`export`关键字，这样我们就可以在`script.js`文件中使用它。

让我们在 math.js 中的 add 函数下添加`export default`:

```
// math.js

function add(a, b) {
 return a + b;
};

export default add;
```

最后一行，我们使这个 add 函数可以在除了`math.js`模块之外的其他地方使用。

使用`export`关键字的另一种方法是在我们定义函数之前添加它:

```
// math.js

export default function add(a, b) {
 return a + b;
};
```

这是使用`export`关键字的两种不同方式，但两者的工作原理是一样的。

你可能想知道`export`后面的`default`关键字是什么。我们一会儿就会谈到这一点。现在，让我们在另一个文件中使用我们的`add`函数，因为我们已经导出了它。

## 导入关键字

我们可以使用 import 关键字将我们的 add 函数导入到我们的`script.js`文件中。导入这个函数仅仅意味着我们将获得对那个函数的访问，并且能够在文件中使用它。一旦函数被导入，我们就可以使用它了:

```
// script.js
import add from './math.js';

console.log(add(2, 5)); //outputs 7
```

在这里，对于`./math.js`，我们使用相对导入。要了解相对路径和绝对路径的更多信息，请查看这个有用的 [StackOverflow](https://stackoverflow.com/questions/21306512/difference-between-relative-path-and-absolute-path-in-javascript) 答案。

当我们运行这段代码时，我们可以看到调用 add 函数`7`的结果。现在，您可以在这个文件中任意多次使用 add 函数。

add 函数本身的代码现在已经看不到了，我们可以使用 add 函数，而不需要查看函数本身的代码。

如果我们暂时注释掉行`import add from './math.js'`，我们会突然得到一个错误:`ReferenceError: add is not defined`。这是因为`script.js`没有访问 add 函数的权限，除非我们明确地将该函数导入到这个文件中。

我们已经导出了 add 函数，将其导入到我们的`script.js`文件中，然后调用该函数。

让我们再看看我们的`math.js`文件。如前所述，当您看到带有关键字`export`的单词`default`时，您可能会感到困惑。再来说说`default`关键词。

## JavaScript 中的命名导出和默认导出

对于 ES 模块，您可以使用命名导出或默认导出。

在我们的第一个例子中，我们使用了一个**默认导出。**使用默认导出，我们只从`math.js`模块中导出了一个值(我们的 add 函数)。

使用默认导出时，如果愿意，您可以给导入重新命名。在我们的`script.js`文件中，我们可以导入我们的 add 函数，并将其称为 addition(或任何其他名称):

```
// script.js
import addition from './math.js';

console.log(addition(2, 5)); //outputs 7
```

另一方面，**命名导出**用于从模块导出*多个值*。

让我们创建一个使用命名导出的例子。回到我们的`math.js`文件，再创建两个函数，减法和乘法，并将它们放在加法函数下面。对于命名导出，您可以删除关键字`default`:

```
// math.js

export default function add(a, b) {
 return a + b;
};

export function subtract(a, b) {
 return a - b;
};

export function multiply(a, b) {
 return a * b;
};
```

在`script.js`中，让我们删除所有之前的代码，导入我们的减法和乘法函数。要导入命名的导出，请用花括号将它们括起来:

```
import { multiply, subtract } from './math.js'; 
```

现在我们可以在我们的`script.js`文件中使用这两个函数:

```
// script.js
import { multiply, subtract } from './math.js';

console.log(multiply(5, 5));

console.log(subtract(10, 4))
```

如果您想重命名一个已命名的导出，您可以使用`as`关键字:

```
import add, { subtract as substractNumbers } from './math.js';

console.log(substractNumbers(2, 5)); 
```

上面，我们已经将我们的`subtract`导入重命名为`subtractNumbers`。

让我们回到我们的 add 函数。如果我们想在我们的`script.js`文件中再次使用它，与我们的`multiply`和`subtract`函数放在一起，该怎么办？我们可以这样做:

```
import add, { multiply, subtract } from './math.js';

console.log(multiply(5, 5));

console.log(subtract(10, 4))

console.log(add(10, 10));
```

现在我们已经学会了如何使用 ES 模块。我们已经学习了如何使用`export`关键字和`import`关键字，还学习了命名导出和默认导出之间的区别。我们还学习了如何重命名默认导出和命名导出。

# JavaScript 中的其他模块系统

在学习模块时，您可能见过甚至使用过不同类型的导入，可能是这样的:

```
var models = require('./models')
```

这就是学习 JavaScript 中的模块会让人感到困惑的地方。让我们深入 JavaScript 模块的简史来澄清这个困惑。

上面使用`require`语句的代码示例是 CommonJS。CommonJS 是另一个可以在 JavaScript 中使用的模块系统。

JavaScript 刚创建的时候，还没有模块系统。因为 JavaScript 没有模块系统，开发者在语言之上创建了他们自己的模块系统。

多年来，不同的模块系统被创建和使用，包括 CommonJS。当在一家公司的代码库中或者在一个开源项目中工作时，您可能会发现使用了不同的模块系统。

最终，ES 模块作为 JavaScript 中的标准化模块系统被引入。

在本文中，我们已经了解了什么是模块以及开发人员为什么使用它们。我们已经了解了 ES 模块是如何工作的，以及 JavaScript 中不同种类的模块系统。

如果你喜欢这篇文章，加入我的[编码俱乐部](https://madisonkanna.us14.list-manage.com/subscribe/post?u=323fd92759e9e0b8d4083d008&id=033dfeb98f)，在那里我们每周日一起应对编码挑战，并在学习新技术时相互支持。

如果你对这篇文章有反馈或问题，或者在 Twitter 上找到我。