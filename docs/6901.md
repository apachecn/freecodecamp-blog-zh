# 使用 Vavr 更好地处理 Java 8 流中的异常

> 原文：<https://www.freecodecamp.org/news/better-exception-handling-in-java-8-streams-using-vavr-6eda31285ce9/>

作者 Rajasekar Elango

在这篇文章中，我将提供在 Java 8 流中使用函数式 Java 库 [Vavr](http://www.vavr.io/) 进行更好的异常处理的技巧。

### 问题是

为了举例说明，假设我们想要以格式`MM/dd/YYYY`打印给定的日期字符串流的星期几。

### 初始解

让我们从一个初始的解决方案开始，如下图所示，并对其进行迭代改进。

这将输出

*嗯*，如果日期字符串无效，则在第一个 *DateTimeParseException* 时失败，没有继续有效的日期。

### Java 8 可选拯救

我们可以重构`parseDate`来返回`Optional<LocalDa` te >，让它丢弃无效数据并继续处理有效日期。

这将跳过错误并转换所有有效日期。

```
WEDNESDAY Text '01-01-2015' could not be parsed at index 2 THURSDAY Text 'not a date' could not be parsed at index 0 FRIDAY
```

这是一个很大的改进，但是异常必须在`parseDate`方法中处理，并且不能传递回主方法来处理它。我们不能让`parseDate`方法抛出一个检查过的异常，因为 Streams API 不能很好地处理抛出异常的方法。

### 用 Vavr 的 Try Monad 更好的解决方案

[Vavr](http://www.vavr.io/) 是 Java 8+的函数库。我们将使用 Vavr 中的`Try`对象，它可以是`Success`或`Failure`的实例。基本上, [Try](http://www.javaslang.io/javaslang-docs/#_try) 是一个一元容器类型，它表示一个可能导致异常的计算，或者返回一个成功计算的值。下面是使用`Try`修改后的代码:

输出是

```
WEDNESDAY Failed due to Text '01-01-2015' could not be parsed at index 2 THURSDAYFailed due to Text 'not a date' could not be parsed at index 0 FRIDAY
```

现在异常被传递回 main 来处理它。Try 也有 API 来实现恢复逻辑或在出错时返回默认值。

为了演示这一点，假设我们也想支持`MM-dd-YYYY`作为日期的替代字符串格式。下面的例子显示了我们如何轻松地实现恢复逻辑。

输出显示，现在日期`01-01-2015`也成功转换了。

```
WEDNESDAY THURSDAY THURSDAY Failed due to Text 'not a date' could not be parsed at index 0 FRIDAY
```

所以 *Try Monad* 可以用来优雅地处理异常和*快速失败*错误。

***2018 年 12 月 3 日更新:***

代码示例更新为使用 [Doculet](https://doculet.net/) 。

*最初发布于[http://erajasekar . com/posts/better-exception-handling-Java 8-streams-using-javaslang/](http://erajasekar.com/posts/better-exception-handling-java8-streams-using-javaslang/)。*