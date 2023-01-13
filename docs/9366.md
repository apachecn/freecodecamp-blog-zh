# 正确看待范围

> 原文：<https://www.freecodecamp.org/news/putting-scope-in-perspective-c9a16974c3be/>

蒂芙尼·怀特

![1*bagtQodHCv1PGtuEX5fMNA](img/a1aff1953364dd8f424cc10da13eca3a.png)

# 正确看待范围

在 JavaScript 中，*词法作用域*处理定义变量的位置，以及代码的其余部分如何访问或不访问这些变量。

当谈到范围时，有两个术语需要考虑:本地和全局。理解这两个术语很重要，因为在声明变量和执行代码时，其中一个可能比另一个更危险。

![1*_9SDjff_XSe6Kl_LSJrsKg](img/723c6b04d83b86cfa4a34dfc64d57d94.png)

#### 全球范围

如果在所有函数之外声明变量，则该变量是全局范围的。例如:

```
//global variable, i.e. global scopevar a = "foo";
```

```
function myFunction() {  var b = "bar";  console.log(a+b);}
```

```
myFunction();
```

当一个变量在全局范围内时，它可以被同一 JavaScript 文件中的所有代码访问。在这个例子中，我正在访问我的 console.log 语句中的变量 *a* ，在 *myFunction* 函数内。

![1*QmksMiei1KJbztSbeb6cCA](img/792569bd726c5e7fa07b5aa1c00ae9f5.png)

#### 局部范围

局部变量只存在于函数内部。它们的作用范围仅限于那个单独的函数。

你可以把局部变量想象成介于左花括号和右花括号之间的任何变量。

这些局部变量不能被它们所属的函数之外的代码访问。

看一下这段代码:

```
//global variable, i.e. global scopevar a = "foo";
```

```
function myFunction() {  //local variable, or local scope  var b = "bar";  console.log(a+b);}
```

```
function yourFunction() {  var c = "JavaScript is fun!";  return c;  console.log(c);}
```

```
myFunction();yourFunction();
```

注意每个变量是如何在独立的函数中声明的。它们都是局部变量，在局部范围内，不能被另一个访问。

比如，我不能返回 *yourFunction 中的 *b* ，*，因为 *b* 属于 *myFunction。* *b* 不能被 *yourFunction、*访问，反之亦然。

如果我在调用*函数*时试图返回 *b* 的值，我会得到“*错误:b 未定义。*“为什么？因为 *b* 不属于 *yourFunction。b* 超出了*你的职能*的范围。

当添加嵌套条件时，作用域变得更加复杂。但是我会把它留到下一次。

但是现在，记住全局作用域和局部作用域的区别。下一次你得到一个“*未定义*”的错误时，检查变量的作用域。

*这个帖子也出现在[https://twhite 96 . github . io](https://twhite96.github.io)*