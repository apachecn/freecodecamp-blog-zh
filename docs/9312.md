# JavaScript 模块:初学者指南

> 原文：<https://www.freecodecamp.org/news/javascript-modules-a-beginner-s-guide-783f7d7a5fcc/>

作者:Preethi Kasireddy

# JavaScript 模块:初学者指南

![1*bcZz-qb_DNpvrNNwQBhQmQ](img/b27b005467f57ff573e7ad045f245d50.png)

Source: [https://www.flickr.com/photos/qubodup/16258492451](https://www.flickr.com/photos/qubodup/16258492451)

如果你是 JavaScript 的新手，像“模块捆绑器对模块加载器”、“Webpack 对 Browserify”和“AMD 对 CommonJS”这样的术语会很快变得让人不知所措。

JavaScript 模块系统可能令人生畏，但是理解它对于 web 开发人员来说是至关重要的。

在这篇文章中，我将用简单的英语(和一些代码示例)为你解开这些术语。希望对你有帮助！

注意:为了简单起见，这将分为两个部分:第一部分将解释什么是模块以及我们为什么使用它们。第 2 部分(下周发布)将介绍捆绑模块的含义以及不同的方式。

### 第 1 部分:有人能再解释一下什么是模块吗？

好的作者把他们的书分成章和节；优秀的程序员把他们的程序分成模块。

就像一本书的一章，模块只是一串单词(或者代码，视情况而定)。

然而，好的模块是高度独立的，具有独特的功能，允许它们在必要时被打乱、删除或添加，而不会破坏整个系统。

### 为什么要使用模块？

使用模块有很多好处，有利于一个庞大的、相互依赖的代码库。在我看来，最重要的是:

**1)可维护性:**根据定义，一个模块是自包含的。一个设计良好的模块旨在尽可能地减少对部分代码库的依赖，这样它就可以独立地成长和改进。当一个模块从其他代码中分离出来时，更新这个模块就容易多了。

回到我们的书的例子，如果你想更新你的书的一个章节，如果一个章节的小变化需要你调整其他章节，这将是一个噩梦。相反，你应该以这样一种方式来写每一章，在不影响其他章节的情况下进行改进。

**2) Namespacing:** 在 JavaScript 中，顶级函数范围之外的变量是全局的(也就是说，每个人都可以访问它们)。因此，“名称空间污染”很常见，完全不相关的代码共享全局变量。

在不相关的代码之间共享全局变量是开发中的一大禁忌。

正如我们将在本文后面看到的，模块允许我们通过为变量创建私有空间来避免命名空间污染。

3)可重用性:坦白地说:我们都曾在某个时候将以前编写的代码复制到新项目中。例如，让我们假设您将以前项目中编写的一些实用方法复制到了当前项目中。

这很好，但是如果你找到了一种更好的方法来编写代码的某个部分，你就必须回去并记住在你编写它的任何地方更新它。

这显然是对时间的巨大浪费。如果有——等等——一个我们可以反复重用的模块，不是会容易得多吗？

### 你如何合并模块？

有很多方法可以将模块合并到你的程序中。让我们来看看其中的几个:

#### 模块模式

模块模式用于模仿类的概念(因为 JavaScript 本身不支持类)，这样我们就可以在一个对象中存储公共和私有的方法和变量——类似于 Java 或 Python 等其他编程语言中使用类的方式。这允许我们为我们想要公开的方法创建一个面向公众的 API，同时仍然将私有变量和方法封装在闭包范围内。

有几种方法可以实现模块模式。在第一个例子中，我将使用匿名闭包。这将有助于我们通过将所有代码放在一个匿名函数中来实现我们的目标。(记住:在 JavaScript 中，函数是创建新范围的唯一方式。)

**示例 1:匿名闭包**

```
(function () {
  // We keep these variables private inside this closure scope

  var myGrades = [93, 95, 88, 0, 55, 91];

  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item}, 0);

      return 'Your average grade is ' + total / myGrades.length + '.';
  }

  var failing = function(){
    var failingGrades = myGrades.filter(function(item) {
      return item < 70;});

    return 'You failed ' + failingGrades.length + ' times.';
  }

  console.log(failing());

}());

// ‘You failed 2 times.’
```

有了这个构造，我们的匿名函数就有了自己的求值环境或“闭包”，然后我们立即对它求值。这让我们可以隐藏父(全局)名称空间中的变量。

这种方法的好处在于，您可以在这个函数中使用局部变量，而不会意外覆盖现有的全局变量，但仍然可以访问全局变量，如下所示:

```
var global = 'Hello, I am a global variable :)';

(function () {
  // We keep these variables private inside this closure scope

  var myGrades = [93, 95, 88, 0, 55, 91];

  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item}, 0);

    return 'Your average grade is ' + total / myGrades.length + '.';
  }

  var failing = function(){
    var failingGrades = myGrades.filter(function(item) {
      return item < 70;});

    return 'You failed ' + failingGrades.length + ' times.';
  }

  console.log(failing());
  console.log(global);
}());

// 'You failed 2 times.'
// 'Hello, I am a global variable :)'
```

注意，匿名函数两边的括号是必需的，因为以关键字 *function* 开头的语句总是被认为是函数声明(记住，在 JavaScript 中不能有未命名的函数声明。)因此，括号会创建一个函数表达式。如果你好奇，你可以[在这里](http://stackoverflow.com/questions/1634268/explain-javascripts-encapsulated-anonymous-function-syntax)阅读更多。

**例子 2:全局导入**
像 [jQuery](https://github.com/jquery/jquery/tree/master/src) 这样的库使用的另一种流行方法是全局导入。它类似于我们刚刚看到的匿名闭包，只是现在我们将全局变量作为参数传入:

```
(function (globalVariable) {

  // Keep this variables private inside this closure scope
  var privateFunction = function() {
    console.log('Shhhh, this is private!');
  }

  // Expose the below methods via the globalVariable interface while
  // hiding the implementation of the method within the 
  // function() block

  globalVariable.each = function(collection, iterator) {
    if (Array.isArray(collection)) {
      for (var i = 0; i < collection.length; i++) {
        iterator(collection[i], i, collection);
      }
    } else {
      for (var key in collection) {
        iterator(collection[key], key, collection);
      }
    }
  };

  globalVariable.filter = function(collection, test) {
    var filtered = [];
    globalVariable.each(collection, function(item) {
      if (test(item)) {
        filtered.push(item);
      }
    });
    return filtered;
  };

  globalVariable.map = function(collection, iterator) {
    var mapped = [];
    globalUtils.each(collection, function(value, key, collection) {
      mapped.push(iterator(value));
    });
    return mapped;
  };

  globalVariable.reduce = function(collection, iterator, accumulator) {
    var startingValueMissing = accumulator === undefined;

    globalVariable.each(collection, function(item) {
      if(startingValueMissing) {
        accumulator = item;
        startingValueMissing = false;
      } else {
        accumulator = iterator(accumulator, item);
      }
    });

    return accumulator;

  };

 }(globalVariable)); 
```

在这个例子中， ***全局变量*** 是唯一的全局变量。与匿名闭包相比，这种方法的好处在于，您可以提前声明全局变量，让阅读您代码的人一目了然。

例 3:对象接口
还有一种方法是使用自包含的对象接口来创建模块，如下所示:

```
var myGradesCalculate = (function () {

  // Keep this variable private inside this closure scope
  var myGrades = [93, 95, 88, 0, 55, 91];

  // Expose these functions via an interface while hiding
  // the implementation of the module within the function() block

  return {
    average: function() {
      var total = myGrades.reduce(function(accumulator, item) {
        return accumulator + item;
        }, 0);

      return'Your average grade is ' + total / myGrades.length + '.';
    },

    failing: function() {
      var failingGrades = myGrades.filter(function(item) {
          return item < 70;
        });

      return 'You failed ' + failingGrades.length + ' times.';
    }
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.' 
myGradesCalculate.average(); // 'Your average grade is 70.33333333333333.'
```

正如你所看到的，这种方法让我们决定哪些变量/方法是私有的(例如***【my grades】***)以及哪些变量/方法是公开的(例如***average***&***failed***)。

**示例 4:揭示模块模式**
这与上面的方法非常相似，除了它确保所有的方法和变量在显式公开之前都是私有的:

```
var myGradesCalculate = (function () {

  // Keep this variable private inside this closure scope
  var myGrades = [93, 95, 88, 0, 55, 91];

  var average = function() {
    var total = myGrades.reduce(function(accumulator, item) {
      return accumulator + item;
      }, 0);

    return'Your average grade is ' + total / myGrades.length + '.';
  };

  var failing = function() {
    var failingGrades = myGrades.filter(function(item) {
        return item < 70;
      });

    return 'You failed ' + failingGrades.length + ' times.';
  };

  // Explicitly reveal public pointers to the private functions 
  // that we want to reveal publicly

  return {
    average: average,
    failing: failing
  }
})();

myGradesCalculate.failing(); // 'You failed 2 times.' 
myGradesCalculate.average(); // 'Your average grade is 70.33333333333333.'
```

这看起来似乎有很多东西要理解，但是对于模块模式来说，这只是冰山一角。以下是我在自己的探索中发现的一些有用的资源:

*   Addy Osmani 的《学习 JavaScript 设计模式》:令人印象深刻的简洁阅读中的细节宝库
*   Ben Cherry 的文章:这是一个有用的概述，提供了模块模式的高级用法的例子
*   Carl Danley 的博客:模块模式概述和其他 JavaScript 模式的资源。

### CommonJS 和 AMD

所有这些方法都有一个共同点:使用单个全局变量将其代码包装在一个函数中，从而使用闭包作用域为自己创建一个私有名称空间。

虽然每种方法都有自己的有效之处，但它们都有自己的缺点。

首先，作为开发人员，您需要知道加载文件的正确依赖顺序。例如，假设您在项目中使用了 Backbone，那么您在文件中包含了 Backbone 源代码的脚本标签。

然而，由于 Backbone 对下划线. js 有很强的依赖性，所以 Backbone 文件的脚本标签不能放在下划线. js 文件之前。

作为一名开发人员，管理依赖关系并正确处理这些事情有时会令人头疼。

另一个缺点是它们仍然会导致名称空间冲突。例如，如果你的两个模块有相同的名字呢？或者，如果您有一个模块的两个版本，并且您都需要，该怎么办？

所以你可能想知道:我们能不能设计一种不通过全局作用域来请求模块接口的方法？

还好，答案是肯定的。

有两种流行且实现良好的方法:CommonJS 和 AMD。

#### CommonJS

CommonJS 是一个志愿者工作组，设计并实现用于声明模块的 JavaScript APIs。

CommonJS 模块本质上是一个可重用的 JavaScript 片段，它导出特定的对象，使得其他模块可以在程序中*需要*这些对象。如果你曾经在 Node.js 中编程，你会非常熟悉这种格式。

使用 CommonJS，每个 JavaScript 文件都将模块存储在自己唯一的模块上下文中(就像将其封装在闭包中一样)。在这个范围内，我们使用 *module.exports* 对象来公开模块，*需要*来导入它们。

当您定义一个 CommonJS 模块时，它可能看起来像这样:

```
function myModule() {
  this.hello = function() {
    return 'hello!';
  }

  this.goodbye = function() {
    return 'goodbye!';
  }
}

module.exports = myModule;
```

我们使用特殊的对象模块，并将我们函数的引用放入*模块。这让 CommonJS 模块系统知道我们想要公开什么，以便其他文件可以使用它。*

然后当有人想使用 *myModule* 时，他们可以在他们的文件中要求它，就像这样:

```
var myModule = require('myModule');

var myModuleInstance = new myModule();
myModuleInstance.hello(); // 'hello!'
myModuleInstance.goodbye(); // 'goodbye!'
```

与我们之前讨论的模块模式相比，这种方法有两个明显的好处:

1.避免全局命名空间污染
2。明确我们的依赖关系

而且语法很紧凑，我个人很喜欢。

另一件需要注意的事情是，CommonJS 采用服务器优先的方法，同步加载模块。这很重要，因为如果我们有三个其他模块，我们需要*请求*，它将一个接一个地加载它们。

现在，这在服务器上工作得很好，但不幸的是，在为浏览器编写 JavaScript 时，这使得它更难使用。可以这么说，从网络上读取一个模块比从磁盘上读取要花费更多的时间。只要加载模块的脚本在运行，它就会阻止浏览器运行其他任何东西，直到它完成加载。它之所以这样做，是因为 JavaScript 线程在代码加载之前会停止。(我将在第 2 部分讨论模块捆绑时讨论如何解决这个问题。目前，我们只需要知道这些)。

#### 超微半导体公司

CommonJS 很好，但是如果我们想要异步加载模块呢？答案叫做异步模块定义，简称 AMD。

使用 AMD 加载模块看起来像这样:

```
define(['myModule', 'myOtherModule'], function(myModule, myOtherModule) {
  console.log(myModule.hello());
});
```

这里发生的事情是， ***define*** 函数将每个模块依赖项的数组作为它的第一个参数。这些依赖关系在后台加载(以非阻塞的方式)，一旦加载完毕 ***定义*** 调用它被赋予的回调函数。

接下来，回调函数将加载的依赖项作为参数——在我们的例子中是 ***myModule*** 和***myothemodule—***，允许函数使用这些依赖项。最后，依赖项本身也必须使用 ***define*** 关键字来定义。

例如， ***myModule*** 可能是这样的:

```
define([], function() {

  return {
    hello: function() {
      console.log('hello');
    },
    goodbye: function() {
      console.log('goodbye');
    }
  };
});
```

所以，与 CommonJS 不同，AMD 采用了浏览器优先的方法和异步行为来完成工作。(注意，有很多人强烈认为在开始运行代码时动态地逐段加载文件是不可取的，我们将在下一节的模块构建中对此进行更深入的探讨)。

除了异步性，AMD 的另一个好处是你的模块可以是对象、函数、构造函数、字符串、JSON 等多种类型，而 CommonJS 只支持对象作为模块。

也就是说，AMD 与 io、文件系统和其他可通过 CommonJS 获得的面向服务器的特性不兼容，而且与简单的 *require* 语句相比，函数包装语法有点冗长。

#### UMD

对于需要同时支持 AMD 和 CommonJS 特性的项目，还有另一种格式:通用模块定义(UMD)。

UMD 从本质上创造了一种使用这两者之一的方法，同时也支持全局变量定义。因此，UMD 模块能够在客户机和服务器上工作。

下面让我们快速领略一下 UMD 的商业运作方式:

```
(function (root, factory) {
  if (typeof define === 'function' && define.amd) {
      // AMD
    define(['myModule', 'myOtherModule'], factory);
  } else if (typeof exports === 'object') {
      // CommonJS
    module.exports = factory(require('myModule'), require('myOtherModule'));
  } else {
    // Browser globals (Note: root is window)
    root.returnExports = factory(root.myModule, root.myOtherModule);
  }
}(this, function (myModule, myOtherModule) {
  // Methods
  function notHelloOrGoodbye(){}; // A private method
  function hello(){}; // A public method because it's returned (see below)
  function goodbye(){}; // A public method because it's returned (see below)

  // Exposed public methods
  return {
      hello: hello,
      goodbye: goodbye
  }
}));
```

更多 UMD 格式的例子，请查看 GitHub 上的这个[启发性回购](https://github.com/umdjs/umd)。

### 原生 JS

唷！你还在吗？我没有在森林里跟丢你吧？很好！因为在我们完成之前，我们还需要定义一种类型的模块。

您可能已经注意到，上面的模块都不是 JavaScript 自带的。相反，我们通过使用模块模式、CommonJS 或 AMD 创建了模拟模块系统的方法。

幸运的是，TC39(定义 ECMAScript 语法和语义的标准团体)的聪明人已经引入了 ECMAScript 6 (ES6)的内置模块。

ES6 为导入和导出模块提供了多种可能性，其他人已经做了很好的解释——以下是其中的一些资源:

*   [jsmodules .我](http://jsmodules.io/cjs.html)
*   [exploringjs.com](http://exploringjs.com/es6/ch_modules.html)

相对于 CommonJS 或 AMD，ES6 模块的伟大之处在于它如何设法提供两个世界的最佳之处:紧凑和声明性语法*和*异步加载，以及更多好处，如更好地支持循环依赖。

可能我最喜欢的 ES6 模块的特性是导入是导出的实时只读视图。(与 CommonJS 相比，在 CommonJS 中，导入是导出的副本，因此没有生命力)。

这是一个如何工作的例子:

```
// lib/counter.js

var counter = 1;

function increment() {
  counter++;
}

function decrement() {
  counter--;
}

module.exports = {
  counter: counter,
  increment: increment,
  decrement: decrement
};

// src/main.js

var counter = require('../../lib/counter');

counter.increment();
console.log(counter.counter); // 1
```

在这个例子中，我们基本上制作了模块的两个副本:一个在我们导出它的时候，一个在我们需要它的时候。

此外，main.js 中的副本现在与原始模块断开了连接。这就是为什么即使我们增加我们的计数器，它仍然返回 1——因为我们导入的计数器变量是从模块中断开的计数器变量的副本。

因此，递增计数器将在模块中递增它，但不会递增您的复制版本。修改计数器变量的复制版本的唯一方法是手动修改:

```
counter.counter++;
console.log(counter.counter); // 2
```

另一方面，ES6 为我们导入的模块创建了一个实时的只读视图:

```
// lib/counter.js
export let counter = 1;

export function increment() {
  counter++;
}

export function decrement() {
  counter--;
}

// src/main.js
import * as counter from '../../counter';

console.log(counter.counter); // 1
counter.increment();
console.log(counter.counter); // 2
```

很酷的东西，是吧？我发现实时只读视图真正吸引人的地方是它们如何允许你把你的模块分割成更小的部分而不损失功能。

然后你就可以转身再合并，没问题。它只是“工作”

### 展望未来:捆绑模块

哇！时间去了哪里？这是一次疯狂的经历，但我真诚地希望它能让您更好地理解 JavaScript 中的模块。

在下一部分，我将介绍模块捆绑，涵盖核心主题，包括:

*   为什么我们捆绑模块
*   不同的捆绑方法
*   ECMAScript 的模块加载器 API
*   …以及更多。:)

注意:为了简单起见，我在这篇文章中跳过了一些基本细节(想想:循环依赖)。如果我遗漏了任何重要和/或有趣的东西，请在评论中告诉我！