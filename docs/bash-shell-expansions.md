# Bash 和 shell 扩展:懒惰的列表制作

> 原文：<https://www.freecodecamp.org/news/bash-shell-expansions/>

又到了一年的这个时候了！当商店开始张贴五颜六色闪闪发光的塑料片时，我们都开始感受到一点节日的气氛，我说的节日是指让我们去购物。具体来说，节日礼物购物！(给自己的礼物还是礼物，技术上来说。)

只是为了让这一切不完全疯狂，你应该做一些礼物清单。Bash 能帮上忙。

## 支撑膨胀

这些不是大括号:`()`

这些也不是:`[]`

*这些*是大括号:`{}`

大括号告诉 Bash 对它在它们之间找到的任意字符串做一些事情。多个字符串用逗号分隔:`{a,b,c}`。您还可以添加可选的前言和后记，以附加到每个扩展的结果。大多数情况下，这可以节省一些输入，比如公共文件路径和扩展名。

让我们给每个我们想送礼物的人列个清单。以下命令是等效的:

```
touch /home/me/gift-lists/Amy.txt /home/me/gift-lists/Bryan.txt /home/me/gift-lists/Charlie.txt
```

```
touch /home/me/gift-lists/{Amy,Bryan,Charlie}.txt
```

```
tree gift-lists

/home/me/gift-lists
├── Amy.txt
├── Bryan.txt
└── Charlie.txt
```

哦，该死的，“布莱恩”拼写他的名字有一个“我”。我可以解决这个问题。

```
mv /home/me/gift-lists/{Bryan,Brian}.txt

renamed '/home/me/gift-lists/Bryan.txt' -> '/home/me/gift-lists/Brian.txt'
```

## 外壳参数扩展

[Shell 参数扩展](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)允许我们对括号中的参数进行各种修改，比如操作和替换文本。

我们所有的礼物都应该有一些圣诞礼物。让我们把它变成一个变量:

```
STUFF=

现在，在[的帮助下，将这些项目添加到我们的每个列表中，`tee`命令](https://en.wikipedia.org/wiki/Tee_(command))让`echo`和资料片玩得更好。

```
echo "$STUFF" | tee {Amy,Brian,Charlie}.txt

cat {Amy,Brian,Charlie}.txt

socks
lump of coal
white chocolate
socks
lump of coal
white chocolate
socks
lump of coal
white chocolate
```

### 模式匹配替换

转念一想，也许煤块不是一个好礼物。您可以使用`${parameter/pattern/string}`形式的模式匹配替换用更好的东西来替换它:

```
echo "${STUFF/lump of coal/candy cane}" | tee {Amy,Brian,Charlie}.txt

cat {Amy,Brian,Charlie}.txt

socks
candy cane
white chocolate
socks
candy cane
white chocolate
socks
candy cane
white chocolate
```

这将第一个“块煤”替换为“甘蔗糖”要替换所有实例(如果有多个)，使用`${parameter//pattern/string}`。这不会改变我们的`$STUFF`变量，所以我们仍然可以在以后为某个淘气的人重用原始列表。

### 子链

虽然我们在不断改进，但我们的礼物可能并不都喜欢白巧克力。以防万一，我们最好在我们的清单上加上一些普通的巧克力。因为我超级懒，所以我只需要点击向上箭头，修改之前的 Bash 命令。幸运的是，`$STUFF`变量中的最后一个单词是“chocolate”，它有九个字符长，所以我将使用`${parameter:offset}`告诉 Bash 只保留这一部分。我将使用`tee`的`-a`标志将`a`添加到我现有的列表中:

```
echo "${STUFF: -9}" | tee -a {Amy,Brian,Charlie}.txt

cat {Amy,Brian,Charlie}.txt

socks
candy cane
white chocolate
chocolate
socks
candy cane
white chocolate
chocolate
socks
candy cane
white chocolate
chocolate
```

您还可以:

| 做这个 | 与这个 |
| --- | --- |
| 获取从 *n* 个字符开始的子串 | `${parameter:n}` |
| 获取从 *n* 开始的 *x* 字符的子串 | `${parameter:n:x}` |

那里！现在我们的基本列表完成了。我们喝点蛋奶酒吧。

### 测试变量

你知道，它可能是蛋酒，但我想我昨天为 Amy 创建了一个列表，并把它存储在一个变量中，我可能称之为`amy`。让我们看看我有没有。我会使用`${parameter:?word}`扩展。如果没有`amy`参数，它会将`word`写入标准错误并退出。

```
echo "${amy:?no such}"

bash: amy: no such
```

我想不会。也许是布莱恩呢？

```
echo "${brian:?no such}"

Lederhosen
```

您还可以:

| 做这个 | 与这个 |
| --- | --- |
| 如果`parameter`未设置或为空，则替换`word` | `${parameter:-word}` |
| 如果`parameter`未置位或为空，则替换`word` | `${parameter:+word}` |
| 如果`parameter`未置位或为空，则将`word`分配给`parameter` | `${parameter:=word}` |

### 改变大小写

没错！布莱恩说他想要些皮短裤，所以我给自己写了张便条。这非常重要，所以我会用大写字母将它添加到 Brian 的列表中，并加上`${parameter^^pattern}`扩展。`pattern`部分是可选的。我们只写 Brian 的列表，所以我用`>>`代替`tee -a`。

```
echo "${brian^^}" >> Brian.txt

cat Brian.txt

socks
candy cane
white chocolate
chocolate
LEDERHOSEN
```

您还可以:

| 做这个 | 与这个 |
| --- | --- |
| 第一个字母大写 | `${parameter^pattern}` |
| 首字母小写 | `${parameter,pattern}` |
| 小写所有字母 | `${parameter,,pattern}` |

### 扩展阵列

你知道吗，所有的礼物清单都是很大的工作量。我将把我在商店看到的东西组成一个数组:

```
gifts=(sweater gameboy wagon pillows chestnuts hairbrush)
```

我可以使用`${parameter:offset:length}`形式的子串扩展来简化这个过程。我会把前两个加到艾米的单子上，中间两个加到布莱恩的单子上，最后两个加到查理的单子上。我将使用`printf`来帮助换行。

```
printf '%s\n' "${gifts[@]:0:2}" >> Amy.txt
printf '%s\n' "${gifts[@]:2:2}" >> Brian.txt
printf '%s\n' "${gifts[@]: -2}" >> Charlie.txt
```

```
cat Amy.txt

socks
candy cane
white chocolate
chocolate
sweater
gameboy

cat Brian.txt

socks
candy cane
white chocolate
chocolate
LEDERHOSEN
wagon
pillows

cat Charlie.txt

socks
candy cane
white chocolate
chocolate
chestnuts
hairbrush
```

那里！现在我们有了一套全面的超级个性化礼物清单。谢谢巴什。可惜它也不能帮我们购物。socks\nlump of coal\nwhite chocolate'

echo "$STUFF"
socks
lump of coal
white chocolate
```

现在，在[的帮助下，将这些项目添加到我们的每个列表中，`tee`命令](https://en.wikipedia.org/wiki/Tee_(command))让`echo`和资料片玩得更好。

[PRE5]

### 模式匹配替换

转念一想，也许煤块不是一个好礼物。您可以使用`${parameter/pattern/string}`形式的模式匹配替换用更好的东西来替换它:

[PRE6]

这将第一个“块煤”替换为“甘蔗糖”要替换所有实例(如果有多个)，使用`${parameter//pattern/string}`。这不会改变我们的`$STUFF`变量，所以我们仍然可以在以后为某个淘气的人重用原始列表。

### 子链

虽然我们在不断改进，但我们的礼物可能并不都喜欢白巧克力。以防万一，我们最好在我们的清单上加上一些普通的巧克力。因为我超级懒，所以我只需要点击向上箭头，修改之前的 Bash 命令。幸运的是，`$STUFF`变量中的最后一个单词是“chocolate”，它有九个字符长，所以我将使用`${parameter:offset}`告诉 Bash 只保留这一部分。我将使用`tee`的`-a`标志将`a`添加到我现有的列表中:

[PRE7]

您还可以:

| 做这个 | 与这个 |
| --- | --- |
| 获取从 *n* 个字符开始的子串 | `${parameter:n}` |
| 获取从 *n* 开始的 *x* 字符的子串 | `${parameter:n:x}` |

那里！现在我们的基本列表完成了。我们喝点蛋奶酒吧。

### 测试变量

你知道，它可能是蛋酒，但我想我昨天为 Amy 创建了一个列表，并把它存储在一个变量中，我可能称之为`amy`。让我们看看我有没有。我会使用`${parameter:?word}`扩展。如果没有`amy`参数，它会将`word`写入标准错误并退出。

[PRE8]

我想不会。也许是布莱恩呢？

[PRE9]

您还可以:

| 做这个 | 与这个 |
| --- | --- |
| 如果`parameter`未设置或为空，则替换`word` | `${parameter:-word}` |
| 如果`parameter`未置位或为空，则替换`word` | `${parameter:+word}` |
| 如果`parameter`未置位或为空，则将`word`分配给`parameter` | `${parameter:=word}` |

### 改变大小写

没错！布莱恩说他想要些皮短裤，所以我给自己写了张便条。这非常重要，所以我会用大写字母将它添加到 Brian 的列表中，并加上`${parameter^^pattern}`扩展。`pattern`部分是可选的。我们只写 Brian 的列表，所以我用`>>`代替`tee -a`。

[PRE10]

您还可以:

| 做这个 | 与这个 |
| --- | --- |
| 第一个字母大写 | `${parameter^pattern}` |
| 首字母小写 | `${parameter,pattern}` |
| 小写所有字母 | `${parameter,,pattern}` |

### 扩展阵列

你知道吗，所有的礼物清单都是很大的工作量。我将把我在商店看到的东西组成一个数组:

[PRE11]

我可以使用`${parameter:offset:length}`形式的子串扩展来简化这个过程。我会把前两个加到艾米的单子上，中间两个加到布莱恩的单子上，最后两个加到查理的单子上。我将使用`printf`来帮助换行。

[PRE12]

[PRE13]

那里！现在我们有了一套全面的超级个性化礼物清单。谢谢巴什。可惜它也不能帮我们购物。