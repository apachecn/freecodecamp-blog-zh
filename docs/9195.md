# 学习 ES6 毒品之道第一部分:常量，let & var

> 原文：<https://www.freecodecamp.org/news/learn-es6-the-dope-way-i-const-let-var-ae828580472b/>

欢迎来到**以毒品的方式学习 ES6 的第一部分，**这是一个帮助你轻松理解 ES6 的系列(ECMAScript 6)！

首先， *const* 、 *let* 和 *var* 是怎么回事？

你可能已经目睹过这些情况之一——让和/或 *const* 代替 *var* ，或者*让*和 *const* 同时在同一个代码中使用，或者更令人困惑的是，*让*、 *const* 和 *var* 同时使用！？

嘿，别担心，我抓住你了。让我们一起驱散迷雾:

#### 常数

好处:

*   如果您正在设置一个不打算更改的变量，这很有用。
*   保护和防止您的变量被重新分配。
*   在编译语言中，与使用 plain 'ol *var* 相比，代码的运行时效率得到了提高，从而整体性能得到了提升。

当心:

*   在 Chrome 和 Firefox 中正常工作。但不是在 Safari。相反，它充当一个普通变量，就好像它是 *var，*一样，因此可以被重新赋值。
*   一般来说，编程惯例是将名称设置为全部大写，以向阅读您代码的其他人表明*const*value*的值不应更改——您将会看到小写和大写的 *const* 编码情况。只是一些需要注意的事情。*

示例:

```
// sometimes used as lowercase as when setting up your server.
const express = require(‘express’);
const app = express();

// sometimes uppercase.
const DONT_CHANGE_ME_MAN = “I ain’t changing for no one, man.”

// change attempt #1 
const DONT_CHANGE_ME_MAN = “I told I ain’t changing for no one.”
// change attempt #2
var DONT_CHANGE_ME_MAN = “Still not changing, bro.”
// change attempt #3
DONT_CHANGE_ME_MAN = “Haha, nice try, still not happening.”

// same error for all 3 attempts, const value stays the same:
Uncaught TypeError: Identifier ‘const DONT_CHANGE_ME_MAN’ has already been declared.

// DONT_CHANGE_ME_MAN still results in “I ain’t changing for no one, man.”
```

这有意义吗？

> 想想康斯特，就像恒定的大海——*永无止境，永不改变……*
> 
> ……除了在 Safari。

#### 让

来自 Ruby 或 Python 背景的学生和有经验的程序员会喜欢 *let，*，因为它加强了块范围！

当你迁移到 ES6 国家，你可能会注意到自己正在适应一种新的*让*蜕变接管你的编码风格，并且发现自己不太可能再使用 *var* 了。有了*就让*吧，现在你的价值观从何而来变得更加清晰，不用担心它们会被提升！

好处:

*   块作用域，你的变量的值在当前作用域中应该是完全一样的，它们不像 *var* 那样被提升。
*   如果你不想你的值被覆盖，就像在 for 循环中一样，这非常有用。

当心:

*   你可能不总是想用 *let* 。例如，在变量不容易被块限制的情况下， *var* 可能更方便。

示例:

```
// When using var what do we get?
var bunny = "eat carrot";

if(bunny) {
  var bunny = "eat twig";
  console.log(bunny) //  "eat twig"
}

console.log(bunny)// "eat twig"

// When using let what do we get?
let bunny = "eat carrot";

if(bunny) {
  let bunny = "eat twig";
  console.log(bunny) // "eat twig"
}

console.log(bunny)// "eat carrot"
```

你看出区别了吗？这都是关于范围的。使用 *var* ，它可以访问它的父/外部作用域，因此可以更改 if 语句中的值。不像 *let* 是块范围的，只能在它所在的当前范围内修改。

*let* 超级直截了当。这就像一个人直接对着你的脸说话，并当场告诉你他们到底需要什么，而 *var* 也这样做，但偶尔可能会回答意想不到的答案——由于提升和访问外部范围变量。视情况而定，任何一种都可能对你有利。

关于*让*带来好处的另一个很好的例子:

假设您想要创建一个包含 30 个 div 的游戏板，每个 div 都有自己的价值。如果您用 *var* 来做这件事，那么对于每次迭代， *i* 索引都将被覆盖——每个 div 的值都将是 30！呀！

另一方面，使用 *let* ，每个 div 都有自己的值，因为每次迭代都会维护自己的 div 范围！了解不同之处:

```
// with var. See example live: https://jsfiddle.net/maasha/gsewf5av/
for(var i= 0; i<30; i++){
  var div = document.createElement('div');
  div.onclick = function() {
    alert("you clicked on a box " + i);
   };
   document.getElementsByTagName('section')[0].appendChild(div);
}

// with let. See example live: https://jsfiddle.net/maasha/xwrq8d5j/
for(let i=0; i<30; i++) {
  var div=document.createElement(‘div’);
  div.onclick = function() {
    alert("you clicked on a box " + i);
   };
   document.getElementsByTagName('section')[0].appendChild(div);
}
```

恭喜你。你已经通过了 **Learn ES6 The Dope Way** Part I，现在你知道 const，let 和 var 之间的主要区别了！呜哇！你摇滚明星，你:)

随着越来越多的**学习 ES6，毒品之路**即将进入中级，通过喜欢和关注保持知识更新！

**第一部分:const、let &有**

**[第二部分:(箭头)= >功能和](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-ii-arrow-functions-and-the-this-keyword-381ac7a32881/)关键字**

**[第三部分:模板文字，传播操作符&生成器！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-iii-template-literals-spread-operators-generators-592765337294/)**

第四部分:默认参数、析构赋值和一个新的 ES6 方法！

**[第五部分:类，翻译 ES6 代码&更多资源！](https://www.freecodecamp.org/news/learn-es6-the-dope-way-part-v-classes-browser-compatibility-transpiling-es6-code-47f62267661/)**

你也可以在 https://github.com/Mashadim 的 github ❤网站上找到我