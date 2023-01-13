# 为什么你的遗留软件很难维护——以及该怎么做。

> 原文：<https://www.freecodecamp.org/news/legacy-software-maintenance-challenges/>

信不信由你，即使有更新更通用的选择，一些组织仍然依赖遗留软件来执行操作。我们知道“旧是金”，但遗留应用程序不可能永远闪闪发光。因此，这些旧的和过时的程序变得难以维护。

最近，美国政府问责局(GAO)发布了一份[报告](https://www.gao.gov/products/gao-19-471#summary)，指出了需要现代化的最关键的联邦遗留系统。这是因为它们基于过时的编程语言，容易出现安全漏洞，并且难以维护。

你穿同样的鞋子吗？

在这篇文章中，我将坦率地谈论您在维护遗留软件应用程序时将面临的挑战，以及如何克服这些挑战。

## 遗留系统效率低下

软件维护很重要——它有助于提高产品的效率，减少出错的机会。如果没有足够的维护，应用程序可能会变得低效和难以操作。

首先，维护[一个遗留系统](https://www.freecodecamp.org/news/conquer-legacy-code-f9e23a6ab758/)可能是困难的，因为与任何现代软件中使用的代码相比，所使用的代码是旧的。旧代码通常很庞大、冗长，并且与大多数现代系统不兼容。

例如，2015 年在 ES6 中引入的 JavaScript 箭头函数，为开发人员提供了一种编写更短、更简洁的函数语法的方法，更易于维护。

```
let total = (x, y) => x + y;

/* This arrow function is a shorter form of:
let total = function(x, y) {
  return x + y;
};
*/

console.log(total(5, 2) ); 
// 7 
```

## 维护成本高

大多数遗留系统面临的另一个挑战是高昂的维护成本，这对大多数企业来说可能是可望而不可及的。

例如，根据上述美国政府问责局的报告，美国政府计划在 2019 年花费超过 900 亿美元用于 IT 服务，其中大部分用于维护老化的系统。

此外，随着技术的发展，法规遵从性正成为保护面向消费者的应用程序的一个主要问题。实现与传统系统的兼容既耗时又昂贵。与通常默认兼容的新应用程序相比，这不会发生得那么快。它还需要大量测试，以确保遗留基础架构符合给定的法规。

维护遗留系统的成本通常会随着时间的推移而增加，因为随着技术的进步，它会慢慢变得过时。此外，对现有系统的修改是一种冒险，需要大量的时间和资源。

## 缺乏足够的技能

为了维护遗留软件，你需要一个熟悉其操作的开发人员。然而，大多数开发人员都在用新技术来检验他们的应用程序的未来。因此，找到一个可以使用旧系统的人可能是一个挑战。

在某些情况下，您可能需要重新培训开发人员如何使用遗留系统，这会增加公司的运营成本。

此外，管理和控制软件中发生的变化可能很困难。保持系统运行需要大量的时间和精力，这既昂贵又耗时。

## 与其他 IT 解决方案不兼容

目前，有一些现代工具可以用来实现软件的快速和平稳的维护。然而，大多数传统 IT 基础设施与此类解决方案不兼容，这使得它们的维护变得复杂。

如果遗留系统的功能与新 IT 解决方案的功能不兼容，开发人员可能会发现很难将它们集成到他们的环境中。

向遗留系统引入新功能的难度也增加了维护的难度。由于大多数遗留系统很容易崩溃，试图重组它们并使它们更易于维护可能不会像预期的那样奏效。

## 挑战的解决方案

为了在当今动态的 IT 环境中更好地竞争，传统技术需要现代化。更新的遗留应用程序可以提高用户的工作效率，降低维护成本，并带来更有益的体验。

根据[埃维诺](https://www.avanade.com/en/media-center/press-releases/it-modernization-research)最近的研究，IT 系统现代化可以带来约 14%的收入增长。因此，选择[不同的软件现代化选项](https://www.scnsoft.com/services/application/modernization)可以为您的企业带来显著的好处。

重要的是要注意，软件不应该仅仅因为旧就被宣布过时。一些“旧”软件可能仍然包含丰富的功能，这些功能对于应用程序的最佳运行是有用的。

因此，为了克服维护老化软件的挑战，开发人员可以选择重构系统的源代码。通过这种方式，他们可以使用干净的现代代码，这些代码可重用且易于调试。

在重构中，你改变你的软件系统来增强它的内部结构。但是你不干涉代码的外部行为。这样，由于代码的内部改进，软件的特性得到了优化。

当重构遗留代码时，应该对更新和修改进行充分的测试，以避免应用程序的损坏和不良运行。例如，可以进行回归测试来确保一切都按预期运行。

此外，在资源允许的情况下，开发人员可以选择重写软件的整个源代码，同时采用现代编程方法。

如果您想继续维护您的遗留代码而不破坏它，您可以使用以下三种方法中的任何一种:

1.  识别代码中的变化点
2.  隔离您的代码
3.  包装代码

让我们来谈谈每一种方法。

### 识别代码中的变化点

正如前面指出的，维护遗留代码是一项挑战。有时，问题可能是由编程不当的部分引起的。因此，您可以通过确定一个允许您在不改变源代码的情况下更改应用程序行为的位置来解决这个问题。

例如，假设您在连接到数据库的遗留应用程序中有以下 JavaScript 代码:

```
export class DataConnection {
     //some code here

  connector() {
    // some code to connect to database
  }
}
```

如果您想对上面的代码运行一些测试，但是 **connector()** 方法引起了问题，您可以在不影响源代码的情况下确定在哪里修改代码行为。

在这种情况下，您可以扩展 **DataConnection** 类，并阻止它建立到实际数据库的连接:

```
class FakeConnection extends DataConnection {

    connector() {
    // solve the issues of making calls to DB

    console.log("Establishing a connection")
  }
}
```

因此，在不影响源代码的情况下修改代码行为之后，您可以对代码运行测试并维护它，而不会出现任何问题。

### 隔离您的代码

另一种可以让您轻松维护遗留代码的技术是隔离并在不同的环境中进行任何更改。您只需要确定一个插入点，在那里您可以从现有的遗留代码中调用更改后的代码。

这里有一个例子:

```
class BooksData {
  // some code here

  addBooks(books) {
    for (let book of books) {
      book.addDate()
    }

    // some code here

    booksRecords.getNumberOfBooks().add(books)
  }

  // some code here
}
```

假设你想优化上面遗留代码中的 **books** 引用，但是 **addBooks()** 给你带来了问题。

所以，你可以用另一种新方法隔离代码，比如 **newBooks()。**

然后，您可以成功地在这个新方法上运行测试，因为它与代码的其余部分是分离的。之后，您可以在现有的未更改的代码中包含对新方法的调用。这样，对遗留代码的改动和风险就最小了。

这是:

```
class BooksData {
  // some code here

  newBooks(books) {
    // some smart and testable logic to optimize books
  }

  addBooks(books) {
    const newBooks = this.newBooks(books)

    for (let book of newBooks) {
      book.addDate()
    }

    // some code here

     booksRecords.getNumberOfBooks().add(books)
  }

  // some code here
}
```

### 包装代码

如果您希望在现有代码之前或之后进行更改，包装它也是另一种解决方案。

您可以通过给打算包装的旧方法起一个新名称，添加一个与旧方法具有相同名称和签名的新方法，并从新方法中调用新方法来实现这一点。最后，您应该将新的逻辑放在之前的方法调用之前或之后。

有了新的逻辑，您可以运行测试或进行任何想要的更改，而不会影响源代码。

例如，下面是我们之前使用的代码:

```
class BooksData {
  // some code here

  addBooks(books) {
    for (let book of books) {
      book.addDate()
    }

    // some code here

    booksRecords.getNumberOfBooks().add(books)
  }

  // some code here
}
```

以下是如何通过包装解决问题:

```
class BooksData {
  // some code here

  addBooks(books) {
    // some smart logic to get books
    this.addMoreNewBooks(moreBooks)
  }

  addMoreNewBooks(books) {
    for (let book of books) {
      book.addDate()
    }

    // some code here

    booksRecords.getNumberOfBooks().add(books)
  }

  // some code here
}
```

## 结论

尽管使古董软件现代化是复杂的、高要求的和有风险的，但结果通常是值得冒险的。继续依赖传统的 IT 系统就像继续使用邮局发送紧急信息一样，而电子邮件只需点击一下按钮就能达到目的。

再者，作为一名程序员，你是否用现代编码技能武装自己？或者，您仍然依赖旧的方法吗？

例如，在令人兴奋的 JavaScript 编程世界中，我们见证了像 React 和 Angular 这样的框架的激增，它们定义了语言的未来。花些时间了解它们可以防止你变得过时。

你同意吗？