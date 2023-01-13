# 《月球脚本编写快速指南》

> 原文：<https://www.freecodecamp.org/news/a-quick-guide-to-redis-lua-scripting/>

Redis 是一个流行的内存网格，用于进程间通信和数据存储。你可能听说过它让你运行 Lua 脚本，但是你仍然不确定为什么。如果这听起来像你，请继续读下去。

![lua_script](img/9b67613a44e4672881733e45897df2ff.png)

## 先决条件

你应该在你的系统上安装 [Redis 来遵循这个指南。阅读时检查](https://redis.io/topics/quickstart) [Redis 命令参考](https://redis.io/commands)可能会有帮助。

## 为什么我需要 Lua 脚本？

简而言之:性能提升。您在 Redis 中执行的大多数任务都包含许多步骤。您可以在 Redis 内部用 Lua 完成这些步骤，而不是用您的应用程序语言。

*   这可能会导致更好的性能。
*   此外，脚本中的所有步骤都是以原子方式执行的。当脚本正在执行时，不能运行其他 Redis 命令。

例如，我使用 Lua 脚本来更改存储在 Redis 中的 JSON 字符串。我将在本文末尾详细描述这一点。

## 但是我不认识什么卢阿

![dont-know-s](img/9ee3a0870c0a6fc5725a2446561a7e0f.png)

A person who doesn't know any lua

别担心，Lua 不是很难理解。如果你懂[C 族](https://en.wikipedia.org/wiki/List_of_C-family_programming_languages)的任何一种语言，你应该对 Lua 没问题。此外，我在本文中提供了工作示例。

## 给我举个例子

让我们从通过 ****redis-cli**** 运行脚本开始。从以下内容开始:

```
redis-cli
```

现在运行以下命令:

```
eval “redis.call(‘set’, KEYS[1], ARGV[1])” 1 key:name value
```

**命令告诉 Redis 运行下面的脚本。`”redis.call(‘set’, KEYS[1], ARGV[1])”`字符串是我们的脚本，它在功能上与 Redis 的`set`命令相同。脚本文本后面有三个参数:**

1.  **提供的密钥数量**
2.  **键名**
3.  **第一个论点**

**脚本参数分为两组: ****键**** 和 ****ARGV**** 。**

**我们用紧随其后的数字来指定脚本需要多少个键。在我们的例子中，是 ****1**** 。紧接在这个数字之后，我们需要一个接一个地提供这些密钥。它们可以作为脚本中的 ****键**** 表来访问。在我们的例子中，它包含索引 ****1**** 处的单个值`key:name`。**

****注意，Lua 索引的表以索引** ******1、****** **而不是********0********。****

**我们可以在键后提供任意数量的参数，这些参数在 Lua 中作为 ****ARGV**** 表提供。在这个例子中，我们提供了一个单独的****ARGV****-参数:字符串`value`。正如您已经猜到的，上面的命令将键`key:name`设置为值`value`。**

**提供脚本用作 ****键**** 的键，以及用作 ****ARGV**** 的所有其他参数被认为是一个好的做法。所以你不应该指定 ****键**** 为 0，然后提供 ****ARGV**** 表中的所有键。**

**现在让我们检查脚本是否成功完成。我们将通过运行另一个从 Redis 获取密钥的脚本来实现这一点:**

**eval“return rediss . call(' get '，keys[1])”1 key:name**

**输出应该是`”value”`，表示之前的脚本成功设置了密钥`“key:name”`。**

## **你能解释一下剧本吗？**

**![1*Utfw9sl2XFHDpyulDvoIvA](img/98feb01c9e312ac6dab1056f189fcc24.png)

Doge after seeing the script above** 

**我们的第一个脚本由一条语句组成:`redis.call`函数:**

```
`redis.call(‘set’, KEYS[1], ARGV[1])`
```

**使用`redis.call`你可以执行任何 Redis 命令。第一个参数是这个命令的名称，后跟它的参数。在`set`命令的情况下，这些参数是 ****键**** 和 ****值**** 。支持所有 Redis 命令。[根据文档](https://redis.io/commands/eval):**

> ***Redis 使用相同的 Lua 解释器运行所有命令***

**我们的第二个脚本不仅仅是运行一个命令，它还返回一个值:**

```
`eval “return redis.call(‘get’, KEYS[1])” 1 key:name`
```

**脚本返回的所有内容都被发送到调用进程。在我们的例子中，这个过程是 ****redis-cli**** ，您将在您的终端窗口中看到结果。**

## **更复杂的东西？**

**![man-looking-at-board](img/68b1c0a92a7912de89f739d2aa1f632c.png)

A person planning to build a complex Redis script** 

**我曾经使用 Lua 脚本以特定的顺序从哈希映射中返回元素。顺序本身是由存储在一个有序集合中的散列键指定的。**

**让我们首先通过在 ****redis-cli**** 中运行这些命令来设置我们的数据:**

```
`hmset hkeys key:1 value:1 key:2 value:2 key:3 value:3 key:4 value:4 key:5 value:5 key:6 value:6
zadd order 1 key:3 2 key:1 3 key:2`
```

**这些命令在关键字`hkeys`处创建一个散列映射，并在关键字`order`处创建一个排序集，其中包含以特定顺序从`hkeys`中选择的关键字。**

****你可能要查看**[**hmset**](https://redis.io/commands/hmset)**和**[**zadd**](https://redis.io/commands/zadd)**命令参考详情。****

**让我们运行以下脚本:**

```
`eval “local order = redis.call(‘zrange’, KEYS[1], 0, -1); return redis.call(‘hmget’,KEYS[2],unpack(order));” 2 order hkeys`
```

**您应该会看到以下输出:**

```
`“value:3”
“value:1”
“value:2”`
```

**这意味着我们得到了想要的键值，并且顺序正确。**

## **我必须指定完整的脚本文本来运行它吗？**

**不要！Redis 允许您使用 ****脚本加载**** 命令将脚本预加载到内存中:**

```
`script load “return redis.call(‘get’, KEYS[1])”`
```

**您应该会看到如下输出:**

```
`“4e6d8fc8bb01276962cce5371fa795a7763657ae”`
```

**这是脚本的唯一散列，需要提供给 ****EVALSHA**** 命令来运行脚本:**

```
`evalsha 4e6d8fc8bb01276962cce5371fa795a7763657ae 1 key:name`
```

****注意:应该使用实际的********【SHA1】********hash 由** ******脚本加载****** **命令返回，上面的 hash 只是一个例子。****

## **你提到的改变 JSON 是什么？**

**有时人们在 Redis 中存储 JSON 对象。这是不是一个好主意是另一回事，但在实践中，这种情况发生了很多。**

**如果您必须更改这个 JSON 对象中的一个键，您需要从 Redis 获取它，解析它，更改键，然后序列化并将其设置回 Redis。这种方法有几个问题:**

1.  **并发性。另一个进程可以在 get 和 set 操作之间改变这个 JSON。在这种情况下，更改将会丢失。**
2.  **性能。如果你经常做这些改变，如果对象相当大，这可能会成为你的应用程序的瓶颈。通过在 Lua 中实现这个逻辑，您可以获得一些性能。**

**让我们将一个测试 JSON 字符串添加到 Redis 的键`obj`下:**

```
`set obj ‘{“a”:”foo”,”b”:”bar”}’`
```

**现在让我们运行我们的脚本:**

```
`EVAL ‘local obj = redis.call(“get”,KEYS[1]); local obj2 = string.gsub(obj,”(“ .. ARGV[1] .. “\”:)([^,}]+)”, “%1” .. ARGV[2]); return redis.call(“set”,KEYS[1],obj2);’ 1 obj b bar2`
```

**现在我们将在关键字`obj`下有以下对象:**

```
`{“a”:”foo”,”b”:”bar2"}`
```

**您可以使用 ****脚本加载**** 命令来加载该脚本:**

```
`SCRIPT LOAD ‘local obj = redis.call(“get”,KEYS[1]); local obj2 = string.gsub(obj,”(“ .. ARGV[1] .. “\”:)([^,}]+)”, “%1” .. ARGV[2]); return redis.call(“set”,KEYS[1],obj2);’`
```

**然后像这样运行它:**

```
`EVALSHA <your_script_sha> 1 obj b bar2`
```

******一些注释:******

*   **`..`是 Lua 中的字符串连接操作符。**
*   **我们使用正则表达式模式来匹配键并替换它的值。如果你不懂这个正则表达式，[你可以查看我最近的指南](https://medium.freecodecamp.org/simple-regex-tricks-for-beginners-3acb3fa257cb)。**
*   **Lua RegEx 风格与大多数其他风格的一个不同之处在于，我们使用`%`作为 RegEx 特殊符号的反向引用标记和转义字符。**
*   **我们仍然用`\`而不是`%`对`”`进行转义，因为我们转义的是 Lua 字符串分隔符，而不是 RegEx 特殊符号。**

## **我应该总是使用 Lua 脚本吗？**

**不。我建议只有当你能证明它们能带来更好的性能时才使用它们。总是先运行基准测试。**

**如果你想要的只是原子性，那么[你应该检查 Redis 事务而不是](https://redis.io/topics/transactions)。**

**还有，你的剧本不要太长。请记住，当一个脚本正在运行时，其他一切都在等待它完成。如果你的脚本花费了相当多的时间，它会导致瓶颈而不是提高性能。脚本在超时(默认为 5 秒)后停止。**

**![1*KuCJoYrILg1eaBYHpEhQhg](img/24e8d4144d8f22bc28b40b5111eb5f02.png)

Redis scripts should not take too much time** 

## **最后一句话**

**有关 Lua 的更多信息，请查看[lua.org](http://www.lua.org/start.html)。**

**你可以查看 GitHub 上的 [my node.js 库，获取一些 Lua 脚本的例子(见`src/lua`文件夹)。您还可以在 node.js 中使用这个库来更改 JSON 对象，而无需自己编写任何 Lua 脚本。](https://github.com/aikei/redis-json)**

**— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —**

******感谢您阅读本文。非常感谢您的提问和评论。也欢迎你在推特上关注我****[](https://twitter.com/aikei_en)******。********