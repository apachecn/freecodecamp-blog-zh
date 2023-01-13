# 超越正则表达式:解析上下文无关文法简介

> 原文：<https://www.freecodecamp.org/news/beyond-regular-expressions-an-introduction-to-parsing-context-free-grammars-ee77bdab5a92/>

克里斯托弗·迪金斯

# 超越正则表达式:解析上下文无关文法简介

![0*yyZM9V_77Q-uAj0F](img/8240cac990d44b471aee8e4f2874a001.png)

Photo by [Johannes Plenio](https://unsplash.com/@jplenio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

一个重要且有用的工具已经成为大多数程序员的武器库的一部分，那就是可信赖的 [**正则表达式**](https://en.wikipedia.org/wiki/Regular_expression) **。除此之外，还有上下文无关的语法。这是一个简单的概念，有一个花哨的名字。**

正则表达式是一种验证和查找文本模式的方法。可以使用正则表达式描述和检测的模式(也称为语法)被称为 [**正则语言**](https://en.wikipedia.org/wiki/Regular_language) 。正则语言是乔姆斯基层次结构[](https://en.wikipedia.org/wiki/Chomsky_hierarchy)**中最简单的正式语言。**

**正则表达式非常适合于查找或验证许多类型的简单模式，例如电话号码、电子邮件地址和 URL。然而，当应用于具有递归结构的模式时，它们就不够了，例如:**

*   **HTML / XML 开始/结束标记**
*   **编程语言中的左/右大括号{/}**
*   **算术表达式中的左/右括号**

**为了解析这些类型的模式，我们需要更强大的工具。我们可以移动到下一级形式语法，称为 [**上下文无关语法**](https://en.wikipedia.org/wiki/Context-free_grammar) (CFG)。**

### **解析数学表达式**

**解析所有数学表达式的集合超出了真正的正则表达式的能力。原因是它们可以包含任意深度的嵌套括号对。**

**例如，考虑表达式:`(2 + (3 * (7–4)))`**

**请注意，算术表达式的结构实际上是一棵树:**

```
 `+ / \ 2   *   / \  3   -     / \     7 4`
```

**作为运行 CFG 解析器的结果而生成的树结构被称为 [**解析树**](https://en.wikipedia.org/wiki/Parse_tree) 。**

### **描述上下文无关的语法**

**有两种流行的表达 CFG 文法的方法:**

1.  **[扩展的 Bachus-Naur 形式](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form)(EBNF)——根据**生产规则**描述 CFG。这些规则在应用时可以生成语言中所有可能的法律短语。**
2.  **[解析表达式语法](https://en.wikipedia.org/wiki/Parsing_expression_grammar)(PEG)——根据**识别规则**描述一个 CFG。这些规则可用于匹配语言中的有效短语。**

**PEG 形式相对于 EBNF 的优势在于到解析器的映射是明确的，并且很容易自动化。**

**下面是一个简单的 PEG [来自维基百科](https://en.wikipedia.org/wiki/Parsing_expression_grammar)页面，描述了对非负
整数应用四则基本运算的数学公式。**

```
`Expr ← SumSum ← Product ((‘+’ / ‘-’) Product)*Product ← Value ((‘*’ / ‘/’) Value)*Value ← [0–9]+ / ‘(‘ Expr ‘)’`
```

**简单地说，我们可以这样理解:**

*   **`Expr`是一个`Sum`**
*   **`Sum`是一个`Product`后跟零个或多个子模式，子模式由一个“+”或“-”后跟一个`Product`组成**
*   **`Product`是后面跟有零个或多个子模式的`Value`，子模式由后面跟有`Value`的“*”或“/”组成**
*   **`Value`是字符集{0，..9}，或者是左括号“(”后跟一个`Expr`和一个右括号”)”。**

### **解析器生成器与解析库**

**假设您不是那种喜欢重新发明轮子的人(并不是说这有什么不对)，创建解析器通常有两种选择:**

**1.**使用解析器生成器** —一种从解析器的抽象定义中为解析器生成源代码的工具。JavaScript 中一些流行的例子包括[姜山](http://jison.org/)、 [PEG.js](https://pegjs.org/) 、[内尔利](http://nearley.js.org/)和 [ANTLR](http://www.antlr.org/) 。**

**2.**使用解析库**——一个允许将解析规则表达为 API 的库。JavaScript 中的一些例子包括[八哥](https://github.com/cdiggins/myna-parser)、[帕西蒙](https://github.com/jneen/parsimmon)和 [Chevrotain](https://github.com/SAP/chevrotain) 。**

**我更喜欢使用解析库，因为它们更容易理解、调试、维护和定制。**

### **使用 [Myna 解析库](https://github.com/cdiggins/myna-parser)用 TypeScript / JavaScript 编写解析器**

**![1*YvovmmxWoIEpHKQ1Fu6cxw](img/df1aaec12983f24cbe583c4d7390db56.png)

Common Myna b[y Mahesh Iyer from Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Common_Myna.svg)** 

**最近，我正在做的一个项目([Heron 语言](https://github.com/cdiggins/heron-language/))需要一个可以在浏览器中运行的解析库。我发现现有库的复杂性和开销太大了。鉴于我以前有过用 C++和 C#编写解析库的经验，我决定用 TypeScript 编写一个名为 **Myna** 的[解析库。](https://github.com/cdiggins/myna-parser)**

**Myna 使用[流畅的语法(方法链接)](https://en.wikipedia.org/wiki/Fluent_interface)使得将解析器定义为一组类似 PEG 语法的规则(子解析器)变得容易。**

**下面的例子是来自八哥 GitHub repo 的[:](https://github.com/cdiggins/myna-parser/blob/master/grammars/grammar_arithmetic.js)**

### **从具体语法树到抽象语法树**

**当解析器处理输入时，每个成功匹配的规则(也称为语法产生)都可以映射到解析树中的一个节点。这种产生式规则到树中节点的字面映射是一棵**具体语法树** (CST)。**

**在某些情况下，CST 的用途有限，因为它包含大量的语法混乱，例如源代码中的注释，或者字符串文字是双引号还是单引号。它可能包含为使语法更易于使用而创建的规则的结果，但并不代表用于分析的预期树结构。**

**最简单的做法是只为特定的规则在输出树中创建节点，而跳过其他规则。这种简化版本的解析树被称为 [**抽象语法树**](https://en.wikipedia.org/wiki/Abstract_syntax_tree) **(AST)** 。为了简化后面的处理步骤，可以在 AST 上执行多次传递，以将其转换为可选的 AST 表示。**

**在 Myna 中，AST 是通过从标有`ast`属性的规则中创建节点来生成的。从技术上讲，这个属性返回一个新的规则，它有一个内部属性集，告诉解析器在解析树中生成一个解析节点。**

### **使用生成的八哥抽象语法树**

**下面是一个在“Node”中使用 Myna 定义的解析器的例子。JS”来计算一个算术表达式:**

### **最后的话**

**如果您有兴趣学习更多关于创建和使用解析器的知识，不管 Myna 库是否满足您的特定需求，我鼓励您花一点时间通读 Myna 解析库的[源代码。](https://github.com/cdiggins/myna-parser/blob/master/myna.ts)**

**Myna 是用 TypeScript(大多数程序员都熟悉它的语法)编写的，包含在一个没有依赖关系的文件中，包括详细的文档在内不到 1200 行。**

**如果你有兴趣看到 Myna 应用于一个更复杂的场景，看看 Chickadee 编程语言。这完全是在 TypeScript 中实现的，只依赖于 [Myna 解析库](https://github.com/cdiggins/myna-parser)。Chickadee 是一种微型编程语言，专门用来帮助人们学习实现编程语言的技术。**

**如果你喜欢这篇文章，请让我知道，并考虑与你的朋友和同事分享。**