# Godate 在 Golang 中的简单日期操作

> 原文：<https://www.freecodecamp.org/news/easy-date-manipulation-in-golang-with-godate-485eef7254a0/>

作者 Kofo Okesola

# Godate 在 Golang 中的简单日期操作

![ApiLHn-tcDHzFYnFZCrql0ne4OwU9tEiK4G1](img/00fdac4d6f3cd1d82ba2883082a79cfb.png)

我一直是并将永远是碳的粉丝，并且知道如此高效地处理日期是多么容易。作为一个 Carbon 的粉丝，同时也是一个 Golang 的粉丝，我想为什么不写一个名为 godate 的[库呢。它对 golang 的作用就像 carbon 对 Php 的作用一样，在本文中我将解释如何使用它。](https://github.com/kofoworola/godate)

### 包装分解

这个包主要是一个带有可用助手方法的`GoDate`结构，它充当一个`Time`结构的包装器。还包括一些初始化的功能，如`Now` `Tomorrow`。

### 使用

#### 装置

```
go get github.com/kofoworola/godate
```

还支持 [go 的新模块系统](https://github.com/golang/go/wiki/Modules)。您可以简单地将其导入到您的项目中并运行。Go 将尝试安装这个包的最新版本，在我写这篇文章的时候是 v1.2.0。

#### 使用

使用当前可用的任何方法创建一个新的 GoDate 结构

注意时区的不同，这就是为什么我建议创建一个传递了`time.Location`对象的 GoDate 结构。

一旦你有了一个结构，你就可以很容易地在这个结构上链接方法来实现你的结果，就像这样:

### 可用的方法

#### 比较

可用的比较方式有`IsBefore`、`IsBefore`和`IsWeekend`。方法名称解释了它们的作用:

#### 差异

下面重点介绍了最重要的差异方法。尽管在这些逻辑中还使用了更多的方法:

将另一个`goDate`作为参数的`Difference`方法将差值计算为`methodOwner — parameter`。负差值表示参数出现在`methodOwner`之后。

#### 字符串格式

这些是当前可用的字符串格式化方法。你也可以通过调用`Format()`方法，用自己的方式[格式化](https://yourbasic.org/golang/format-parse-string-time-date-example/)(如果你不熟悉 golang 中的日期，你可能会想读一下)

#### 助手

下面列出了一些额外的助手方法及其输出:

注意`EndOfWeek`和`StartOfWeek`方法使用`time.Sunday`作为默认的一周开始。可以通过调用`now.SetFirstDay(time.Monday)`来改变当前 godate 结构的行为。

### 结论

这个方案还远未完成(可能永远也不会完成)。目标是提供一个健壮的数据处理 API，类似于甚至比 [Carbon](https://carbon.nesbot.com) 更好(这里有人雄心勃勃……)。所以像我这样的爱好者应该让雨落在[回购](https://github.com/kofoworola/godate)(和明星:)