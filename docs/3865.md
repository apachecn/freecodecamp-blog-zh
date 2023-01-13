# 如何安装和开始使用 TypeScript

> 原文：<https://www.freecodecamp.org/news/how-to-install-and-begin-using-typescript/>

TypeScript 是当前 web 开发中的热门话题之一，这是有充分理由的。

它允许我们在声明变量时进行类型转换，这意味着我们显式地设置了我们期望返回的数据类型。然后，如果返回的数据不是我们期望得到的类型，或者函数调用的参数太少或太多，它就会抛出错误。这只是它所提供的一切的一个例子。

如果你想了解数据类型的概况，你会发现阅读我的[上一篇文章](https://jonathansexton.me/blog/learn-typescript-data-types-from-zero-to-hero/)会很有帮助。阅读那篇文章并不是必需的，但它会让您对 TypeScript 的数据类型和语法有更好的理解。

在我们开始之前，重要的是要注意，TypeScript 可以与框架/库结合使用，但也可以独立于框架/库使用。TypeScript 是 Angular 项目中的默认设置，我正在撰写一篇关于如何开始使用它的文章。

另外，本文假设您对 JavaScript 编程有基本的了解。

所以，现在我们已经准备好开始使用 TypeScript，并开始利用它的强大功能。

让我们开始吧！

## 安装 TypeScript

安装 TypeScript 有两种主要方法。第一种是通过 [Visual Studio](https://visualstudio.microsoft.com/vs/) (不要与 [Visual Studio 代码](https://code.visualstudio.com/?wt.mc_id=DX_841432)混淆)，它是一个 [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) 。2015，2017，我相信 2019 版本已经安装了 TypeScript。

这不是我今天要讨论的路线，因为我主要使用 Visual Studio 代码来满足我的所有需求。

第二种方式，也是我们将关注的方式，是通过 [NPM](https://www.npmjs.com/get-npm) (节点包管理器)。

如果您还没有安装 NPM 和/或[节点](https://nodejs.org/en/)(当您安装 Node 时，您将获得 NPM)，现在是这么做的最佳时机，因为这是后续步骤的一个要求(并且是使用 TypeScript 的一个要求)。

![the node js download page](img/6f21144401305e1a34bf73785ca4751b.png)

The Node download page - it's a good idea to use the LTS version as it's the most stable

一旦安装了节点和 NPM，在 VS 代码中打开终端并运行以下命令:

`npm install -g typescript`

安装完成后，您会看到已经添加了 1 个包。您还会看到一条消息，说明所安装的 TypeScript 的版本。

这是开始将 TypeScript 编译成 JavaScript 所需的一切。

现在您已经准备好开始编写 TypeScript 了！

## 启动 TypeScript 项目

让我们创建一个 TypeScript 项目，这样我们就可以利用伴随使用它而来的所有这些伟大的特性。

在你选择的编辑器中(我用的是 VS 代码),让我们创建一个 HTML 文件作为我们代码的可视化部分。下面是我的基本 HTML 文件的样子:

![html text on a dark background](img/0195ba8a97375fcc6f24175019f2204d.png)

Basic HTML boilerplate with some placeholder text

老实说，我们使用这个 HTML 只是为了能在页面上看到一些东西。我们真正关心的是如何使用控制台。

你会注意到我在我们的`index.html`文件的头部链接了`app.js`。

你可能在对自己说*我以为这是一篇关于打字稿的文章？*

沉住气，没错。我只想强调 JavaScript 和 TypeScript 之间的一些区别(您将在下面了解这个文件来自哪里)。

下面您将看到一个简单的变量声明和一个控制台日志语句:

![javascript code showing a username variable declaration](img/16adfa4435136c438d6cab83f7a17921.png)

A simple variable declaration and console log statement

顺便提一下，如果你想禁用一些 [ES-Lint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) 规则，你可以像我上面做的那样，将这些规则放在注释的顶部。或者如果你想**完全**禁用那个文件的 ES-Lint，你可以把`/* eslint-disable */`放在文件的顶部。

这是浏览器控制台:

![the console inside of the firefox browser](img/b9a05f8e50e49bd4c3379000b7bc1f61.png)

Our userName variable inside FireFox

让我们假设我正在构建一个应用程序，对于`userName`我期望总是得到一个字符串。在这个过程中，我可能会犯错误，或者我的数据可能会从其他来源变异。

现在，`userName`是一个数字:(

![javascript code showing a username variable declaration](img/d3c602215aad00c70e0958448b72ef14.png)

Now userName is a number!

这里是浏览器控制台，显示了对`userName`的更改，如果这是一个生产应用程序，我们可能不希望发生这些更改:

![image-7](img/8b6a0fad59d9b6ea62e0415f4eb65e3d.png)

The FireFox console showing the results of the variable mutation

如果返回的`userName`被传递给另一个函数或者被用作一个更大的数据拼图的一部分会怎么样？

这不仅很难找出突变发生在哪里(特别是如果我们有一个更大的应用程序)，而且还会在我们的代码中产生无数的错误。

现在，让我们在 TypeScript 中尝试相同实验。为此，我们需要创建一个扩展名为 `.ts`的新文件来表示一个 TypeScript 文件。

我将我的命名为`app.ts`,以保持与命名约定一致，并将相同的代码从我们的 JavaScript 文件放入我们新的 TypeScript 文件中。

![typescript code on a dark background ](img/157856c1cfd732b38789e5dec10ef927.png)

The same code from our JavaScript copied over into the TypeScript file

你会注意到我现在在声明变量时使用了类型转换，并且我明确地告诉 TypeScript 这个变量应该只指向一个字符串值。

你还会注意到，当我重新分配它的值时，在`userName`下面有一个错误行。

## 使用 CLI 编译 TypeScript

为了在我们的控制台上看到这个，我们必须把它编译成 JavaScript。我们通过在 VS 代码控制台中运行`tsc app.ts`来做到这一点(只要您在正确的目录中，您也可以在任何终端中运行相同的命令)。

当我们运行这个命令时，它会将我们的类型脚本编译成 JavaScript。它还会生成另一个同名文件，只是扩展名为`.js`。

这就是我在文章前面提到的那个`app.js`文件的来源。

要一次编译多个文件，只需在命令中依次提供这些名称:`tsc app.ts header.component.ts`

还可以通过添加`--out`标志将多个 TypeScript 文件编译成一个 JavaScript 文件:

`tsc *.ts --out index.js`

还有一个 watch 命令，它会在检测到更改时自动重新编译所有的 TypeScript。这使您不必一遍又一遍地运行相同的命令:

`tsc *.ts --out app.js --watch`

下面是上面那个`tsc app.ts`命令的输出:

![image-9-1024x408](img/b9cbe97125c63bdd3163ba556e2e6247.png)

The error in my console

这个错误告诉我们`userName`的重新分配有问题。因为我们显式地将变量设置为字符串(*即使我没有将变量设置为字符串，错误仍然会发生，因为 TypeScript 推断数据类型*)我们不能将它重新分配给一个数字。

这是一个很好的特性，因为它迫使我们明确地声明变量，避免我们犯令人讨厌和耗时的错误。如果你期望一个特定类型的数据，你应该得到那个数据，否则你应该得到一个错误。

## 使用显式声明的数组和对象

假设我正在构建一个项目，而不是手动设置导航链接，我想将这些信息存储在一个对象数组中。

我希望存储的信息有一个特定的格式，以便在所有链接中保持一致。

下面是我如何在 TypeScript 中设置一个“复杂”数组:

![image-1-1024x51](img/0588889926e330c3d5ad0968a55c99e7.png)

An array with a specific format

在左侧，我们声明变量的名称`navLinks`，后跟一个冒号。在花括号处，我们开始声明我们期望在这个数组中的信息的格式。

我们告诉 TypeScript，我们希望这个数组包含一个或多个具有这些属性名称和类型的对象。我们期望一个字符串形式的`name`，一个字符串形式的`link`，以及一个同样是字符串形式的`alt`。

与其他[数据类型](https://jonathansexton.me/blog/learn-typescript-data-types-from-zero-to-hero/)一样，如果我们偏离了我们为这个变量建立的格式，我们就会遇到错误。

在这里，我们试图添加一个空白的新条目，我们得到了以下错误:

`Type '{}' is missing the following properties from type '{ name: string; link: string; alt: string; }' : name, link, [alt ts(2739)]`

![image-3-1024x97](img/d3ea16e526b0ea0a9196708cbfc35ed0.png)

如果我们尝试添加另一个包含错误信息类型的条目，也会出现类似的错误:

`{ name: 'Jonathan', link: 15, alt: false }` ❌

`{ name: ['Jon','Marley'], link: `https://link123.net`, alt: null }` ❌

`this.navLinks[0].img = '../../assets/img'` ❌

`this.navLinks[0].name = 'Barnaby'`

不过，你明白了。一旦我们建立了格式，TypeScript 就会让我们遵守该格式，并在我们出错时通知我们。

此外，还有几种定义数组的方法:

`const arr1: Array<any> = ['Dave', 35, true];` *//允许我们拥有任意数量的任意类型的元素*

`const people: [string,string,string] = ['John', 'Sammy', 'Stephanie'];` *//如果数组*中出现 3 个以上的字符串或任何非字符串元素，将抛出错误

`const people: Array<string> = ['Jimmy', 'Theresa', 'Stanley'];`*//允许我们的数组中有任意数量的字符串元素*

对象的工作方式与 TypeScript 中的数组非常相似。它们可以用集合类型显式定义，或者您可以让 TypeScript 做所有的推断。下面是一个对象的基本示例:

`const person: {name:string, address: string, age: number} = {name: 'Willy', address: '123 Sunshine Ln', age: 35}`

同样，在左侧，我们将 person 声明为变量名，第一组花括号定义了我们想要的数据格式。

需要注意的是，在对象中，我们定义属性的顺序不必与格式的顺序相匹配:

![image-5](img/45b50a6916561eb22f512346ce9757be.png)

The properties do not have to match the order in which they were defined

## 函数、参数和自变量

您将在 TypeScript 中看到的一些最大的好处来自于函数的使用。

你是否曾经构建了一个函数来完成一个特定的任务，却发现它并没有像你预期的那样工作？

在使用 TypeScript 时，这并不是因为您没有获得/发送正确的数据类型或使用正确数量的参数/变量。

这里有一个很好的例子:

![image-1024x454](img/a9e2cb04989893b27eea7b6d55037d7f.png)

A TypeScript function that should return a number

在我们的函数中，我们期望在`calculator`执行时收到 3 个参数。然而，如果我们接收到错误数量的参数(太少或太多)，TypeScript 会给我们一个很好的错误:

![image-4](img/737887056d9226db26a1b93aa0bf7639.png)

The error we get when calling a function with the incorrect amount/type of arguments

同样，如果我们在执行此函数时收到错误的数据类型，TypeScript 将生成一个错误，函数将不会运行。

`calculator('12', '11', 'add) ;` ❌

现在你可能会对自己说‘那又怎样？这一切都很好，但似乎没什么大不了的。但是想象一下，你的应用程序是几十个文件，有许多抽象层。

一个很好的例子就是带有服务、数据模型、多级组件以及所有相关依赖的 Angular 应用程序。当您的应用程序变得很大时，确定错误来自哪里变得越来越困难。

## 就这样

希望您现在可以看到 TypeScript 的好处。除了我在这里列出的，还有很多，如果你想找到更多，我鼓励你去阅读文档。

你可以在我的博客上找到这篇文章和其他类似的文章。我很希望你能过来！

既然你在那里，为什么不注册我的**时事通讯**-你可以在主博客页面的右上角注册。我保证我永远不会给你的收件箱发垃圾邮件，你的信息也不会与任何人/网站共享。我喜欢偶尔发送我发现的有趣资源、关于 web 开发的文章以及我的最新帖子列表。

如果你还没有，你也可以在社交媒体上和我联系！我所有的链接也在那个页面的右上方。我喜欢和别人交流，认识新朋友，所以不要害怕打招呼。？

祝你有美好的一天，朋友，祝你编码愉快！