# 带有示例的 Erlang 概述

> 原文：<https://www.freecodecamp.org/news/an-overview-of-erlang-with-examples/>

Erlang 是由 Ericsson 开发的用于电信应用的函数式编程语言。因为他们认为电信系统长时间停机是不可接受的，所以 Erlang 的设计初衷是:

*   分布式容错*(一个软件或硬件故障不会导致系统瘫痪)*
*   并发*(它可以产生许多进程，每个进程执行一个小的、定义明确的工作，并且彼此隔离，但是能够通过消息传递进行通信)*
*   热插拔*(代码可以在系统运行时交换到系统中，从而实现高可用性和最小的系统停机时间)*

### **语法**

二郎大量使用 ****递归**** 。因为在 Erlang 中数据是不可变的，所以不允许使用`while`和`for`循环(其中变量需要不断改变它的值)。

这里有一个递归的例子，展示了一个函数如何重复地从一个名字的前面去掉第一个字母并打印出来，直到遇到最后一个字母才停止。

```
-module(name).

-export([print_name/1]).

print_name([RemainingLetter | []]) ->
  io:format("~c~n", [RemainingLetter]);
print_name([FirstLetter | RestOfName]) ->
  io:format("~c~n", [FirstLetter]),
  print_name(RestOfName).
```

输出:

```
> name:print_name("Mike").
M
i
k
e
ok
```

还有一种对 ****模式匹配**** 的强调，它经常消除对`if`结构或`case`语句的需要。在下面的示例中，有两个特定名称的匹配项，后面是所有其他名称的匹配项。

```
-module(greeting).

-export([say_hello/1]).

say_hello("Mary") ->
  "Welcome back Mary!";
say_hello("Tom") ->
  "Howdy Tom.";
say_hello(Name) ->
  "Hello " ++ Name ++ ".".
```

输出:

```
> greeting:say_hello("Mary").
"Welcome back Mary!"
> greeting:say_hello("Tom").
"Howdy Tom."
> greeting:say_hello("Beth").
"Hello Beth."
```

## **Erlang 术语存储**

Erlang Term Storage 通常缩写为 ETS，是内置于 OTP 中的内存数据库。它可以在 Elixir 中访问，当您的应用程序在单个节点上运行时，它是 Redis 等解决方案的强大替代方案。

## **快速启动**

要创建一个 ETS 表，你首先需要初始化一个表`tableName = :ets.new(:table_otp_name, [])`，一旦你初始化了一个表，你可以:插入数据，查找值，删除数据，等等。

### **IEX 的 ETS 演示**

```
iex(1)> myETSTable = :ets.new(:my_ets_table, [])
#Reference<0.1520230345.550371329.65846>
iex(2)> :ets.insert(myETSTable, {"favoriteWebSite", "freeCodeCamp"})
true
iex(3)> :ets.insert(myETSTable, {"favoriteProgrammingLanguage", "Elixir"})
true
iex(4)> :ets.i(myETSTable)
<1   > {<<"favoriteProgrammingLanguage">>,<<"Elixir">>}
<2   > {<<"favoriteWebSite">>,<<"freeCodeCamp">>}
EOT  (q)uit (p)Digits (k)ill /Regexp -->
```

## **坚持**

ETS 表不是持久的，一旦拥有它的进程终止，它就会被销毁。如果您想永久存储数据，建议使用传统的数据库和/或基于文件的存储。

## **用例**

ETS 表通常用于缓存应用程序中的数据，例如，从数据库中提取的帐户数据可以存储在 ETS 表中，以减少对数据库的查询量。另一个用例是在 web 应用程序中限制特性的使用速率——ETS 的快速读写速度非常适合这种情况。ETS 表是以尽可能低的硬件成本开发高度并发的 web 应用程序的强大工具。

### **尝试一下**

在一些网站上，您可以尝试运行 Erlang 命令，而无需在本地安装任何东西，例如:

*   [试试看！(一个动手教程)](http://www.tryerlang.org/)
*   [教程点编码背景](https://www.tutorialspoint.com/compile_erlang_online.php)

如果你想把它安装在你的(或虚拟)机器上，你可以在[Erlang.org](https://www.erlang.org/downloads)或 [Erlang Solutions](https://www.erlang-solutions.com/resources/download.html) 上找到安装文件。

#### **更多信息:**

*   [关于二郎](https://www.erlang.org/about)
*   [Erlang(编程语言)](https://en.wikipedia.org/wiki/Erlang_(programming_language))