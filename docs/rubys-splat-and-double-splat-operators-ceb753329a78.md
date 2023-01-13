# Ruby * Splat 和 double **Splat 运算符简介

> 原文：<https://www.freecodecamp.org/news/rubys-splat-and-double-splat-operators-ceb753329a78/>

迪克兰·米恩

![1*4Cc4O2WsCRkCaFiy0u92QA](img/29c07b869115dd042e2b8d80f95eb854.png)

photo courtesy clker.com

# Ruby * Splat 和 double **Splat 运算符简介

你有没有想过在不知道一个方法需要多少个参数的情况下定义它？你是否度过了漫长的不眠之夜，希望有一种简单的方法将一个列表分成一个散列？看看 Ruby 的 splat 操作符就知道了！你可以用它们做很多很棒的事情，但是我只打算回顾一下基础知识和我发现的一些巧妙的技巧。

### 单一*Splat

splat 运算符几乎有无限的用途。但是主要的思想是，每当你不想指定参数的数量时，你可以使用 splat 操作符。最简单的例子是这样的:

另一个有用的东西是 splat 操作符可以将一个数组分成几个参数:

```
arr = ["first", "second", "third"]def threeargs(*arr)#makes three arguments
```

您还可以使用 splat 运算符来抓取数组的任何一段:

```
first, *rest, last  = ["a", "b", "c", "d"]p first # "a"p rest # ["b", "c"]p last # "d"
```

您会注意到 rest 变量仍然是一个数组，这非常方便。所以，按照最后一个例子，你仍然可以这样做:

```
first, *rest, last  = ["a", "b", "c", "d"]p rest[0] # "b"
```

这些是单一 splat 操作符的基础，但是我强烈建议您多使用它。它可以做像组合数组，把散列和字符串变成数组，或者从数组中取出项目这样的事情！

### 双* *啪

双 splat 操作符是在 Ruby 2.0 中出现的。它与最初的 splat 非常相似，只有一点不同:它可以用于散列！这是一个双拍子最基本用法的例子。

```
def doublesplat(**nums)  p **numsenddoublesplat one: 1, two: 2 # {:one=>1, :two=>2}
```

### 把所有的放在一起

我希望你能看到，将这两者结合使用，可能性是无穷无尽的。要记住的主要事情是，当您不确定一个方法将使用多少个参数时，您可以使用 splats 作为该方法中的一个参数。

最后，我做了一个小函数，展示了如何使用单个 splat 和两个 splat 过滤掉任何不是键值对的参数。

```
def dubSplat(a, *b, **c)  p cenddubSplat(1,2,3, 4, a: 40, b: 50)#{:a=>40, :b=>50}
```

感谢您的阅读，现在您可以自己尝试使用它了！