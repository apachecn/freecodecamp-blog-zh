# SQL Where–子句示例

> 原文：<https://www.freecodecamp.org/news/sql-where-clause-examples/>

有时，当您使用 SQL 时，您不需要对整个记录范围进行操作。或者如果你不小心修改或删除了所有内容，那就真的很糟糕了。

在这些情况下，您只需要选择您想要处理的那部分记录，那些满足特定条件的记录。这就是 SQL 的`WHERE`子句有用的地方。

# SQL `WHERE`子句语法

你这样写`WHERE`子句:

```
SELECT column1, column2...
FROM table_name
WHERE condition;
```

注意，这里我使用了`SELECT`语句来编写它，但是它的使用并不局限于`SELECT`。你也可以把它和其他语句一起使用，比如`DELETE`和`UPDATE`。

# 运行中的 SQL `WHERE`子句

让我们以这个`users`表为例来说明如何使用`WHERE`子句。

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 布赖恩 | Fifteen | 密歇根 | [brian@example.com](mailto:brian@example.com) |
| Two | 伦纳德 | Fifty-five | 密西西比河 | [leonard@example.com](mailto:leonard@example.com) |
| three | 砧骨 | Thirty-one | 南达科塔州 | [anvil@example.com](mailto:anvil@example.com) |
| four | 乔 | forty-four | 缅因州 | [jo@example.com](mailto:jo@example.com) |
| five | 梅瑞狄斯 | Forty-three | 特拉华河 | [meredith@example.com](mailto:meredith@example.com) |
| six | 见 BＵＦＦＡＬＯ BIＬＬ | Sixteen | 密歇根 | [cody@example.com](mailto:cody@example.com) |
| seven | 迪拉拉 | Fifty | 俄亥俄州 | [dilara@example.com](mailto:dilara@example.com) |
| eight | 科尔宾 | Forty-seven | 威斯康星州 | [corbin@example.com](mailto:corbin@example.com) |
| nine | 杜松子酒 | Sixty-three | 伊利诺伊 | [gin@example.com](mailto:gin@example.com) |
| Ten | 爱丽丝 | Fifty | 内华达州 | [alice@example.com](mailto:alice@example.com) |
| Eleven | Saint） | Twenty-one | 马萨诸塞州 | [zachery@example.com](mailto:zachery@example.com) |
| Twelve | 德尔马 | fifty-six | 爱达荷 | [delmar@example.com](mailto:delmar@example.com) |
| Thirteen | 丹尼斯 | Ninety-six | 俄亥俄州 | [dennie@example.com](mailto:dennie@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |
| Fifteen | 巴士拉 | Eighteen | 南达科塔州 | [busrah@example.com](mailto:busrah@example.com) |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |
| Eighteen | 中提琴 | Sixty-six | 缅因州 | [viola@example.com](mailto:viola@example.com) |
| Nineteen | Nadya | Twenty-two | 佛罗里达州 | [nadya@example.com](mailto:nadya@example.com) |
| Twenty | 伊莎贝拉！伊莎贝拉 | Sixty-one | 亚利桑那州 | [izabela@example.com](mailto:izabela@example.com) |

## 带有`SELECT`语句的 SQL `WHERE`子句示例

当您希望确保某个事件将影响 50 岁或以上的人时，您可以仅选择具有以下代码的用户:

```
SELECT *
FROM users
WHERE age >= 50;
```

这将给出如下表格，其中仅列出了 50 岁或以上的用户:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| Two | 伦纳德 | Fifty-five | 密西西比河 | [leonard@example.com](mailto:leonard@example.com) |
| seven | 迪拉拉 | Fifty | 俄亥俄州 | [dilara@example.com](mailto:dilara@example.com) |
| nine | 杜松子酒 | Sixty-three | 伊利诺伊 | [gin@example.com](mailto:gin@example.com) |
| Ten | 爱丽丝 | Fifty | 内华达州 | [alice@example.com](mailto:alice@example.com) |
| Twelve | 德尔马 | fifty-six | 爱达荷 | [delmar@example.com](mailto:delmar@example.com) |
| Thirteen | 丹尼斯 | Ninety-six | 俄亥俄州 | [dennie@example.com](mailto:dennie@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |
| Eighteen | 中提琴 | Sixty-six | 缅因州 | [viola@example.com](mailto:viola@example.com) |
| Twenty | 伊莎贝拉！伊莎贝拉 | Sixty-one | 亚利桑那州 | [izabela@example.com](mailto:izabela@example.com) |

## 带有`DELETE`语句的 SQL `WHERE`子句示例

让我们假设科迪已经决定把自己从这个列表中删除。您可以使用`DELETE`语句和`WHERE`来更新该表，以确保只删除 Cody 的记录。

```
DELETE FROM users
WHERE name IS "Cody";
```

`users`表格现在看起来如下，没有第 6 行(Cody 的信息所在的位置):

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 布赖恩 | Fifteen | 密歇根 | [brian@example.com](mailto:brian@example.com) |
| Two | 伦纳德 | Fifty-five | 密西西比河 | [leonard@example.com](mailto:leonard@example.com) |
| three | 砧骨 | Thirty-one | 南达科塔州 | [anvil@example.com](mailto:anvil@example.com) |
| four | 乔 | forty-four | 缅因州 | [jo@example.com](mailto:jo@example.com) |
| five | 梅瑞狄斯 | Forty-three | 特拉华河 | [meredith@example.com](mailto:meredith@example.com) |
| seven | 迪拉拉 | Fifty | 俄亥俄州 | [dilara@example.com](mailto:dilara@example.com) |
| eight | 科尔宾 | Forty-seven | 威斯康星州 | [corbin@example.com](mailto:corbin@example.com) |
| nine | 杜松子酒 | Sixty-three | 伊利诺伊 | [gin@example.com](mailto:gin@example.com) |
| Ten | 爱丽丝 | Fifty | 内华达州 | [alice@example.com](mailto:alice@example.com) |
| Eleven | Saint） | Twenty-one | 马萨诸塞州 | [zachery@example.com](mailto:zachery@example.com) |
| Twelve | 德尔马 | fifty-six | 爱达荷 | [delmar@example.com](mailto:delmar@example.com) |
| Thirteen | 丹尼斯 | Ninety-six | 俄亥俄州 | [dennie@example.com](mailto:dennie@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |
| Fifteen | 巴士拉 | Eighteen | 南达科塔州 | [busrah@example.com](mailto:busrah@example.com) |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |
| Eighteen | 中提琴 | Sixty-six | 缅因州 | [viola@example.com](mailto:viola@example.com) |
| Nineteen | Nadya | Twenty-two | 佛罗里达州 | [nadya@example.com](mailto:nadya@example.com) |
| Twenty | 伊莎贝拉！伊莎贝拉 | Sixty-one | 亚利桑那州 | [izabela@example.com](mailto:izabela@example.com) |

## 带有`UPDATE`语句的 SQL `WHERE`子句示例

也许你已经收到通知，Anvil 已经变老了，现在已经 32 岁了。您可以使用`UPDATE`语句更改 Anvil 的记录，并且可以使用`WHERE`确保只有 Anvil 的记录得到更新。

```
UPDATE users
SET age = 32
WHERE name IS "Anvil";
```

现在，该表将如下所示:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 布赖恩 | Fifteen | 密歇根 | [brian@example.com](mailto:brian@example.com) |
| Two | 伦纳德 | Fifty-five | 密西西比河 | [leonard@example.com](mailto:leonard@example.com) |
| three | 砧骨 | Thirty-two | 南达科塔州 | [anvil@example.com](mailto:anvil@example.com) |
| four | 乔 | forty-four | 缅因州 | [jo@example.com](mailto:jo@example.com) |
| five | 梅瑞狄斯 | Forty-three | 特拉华河 | [meredith@example.com](mailto:meredith@example.com) |
| seven | 迪拉拉 | Fifty | 俄亥俄州 | [dilara@example.com](mailto:dilara@example.com) |
| eight | 科尔宾 | Forty-seven | 威斯康星州 | [corbin@example.com](mailto:corbin@example.com) |
| nine | 杜松子酒 | Sixty-three | 伊利诺伊 | [gin@example.com](mailto:gin@example.com) |
| Ten | 爱丽丝 | Fifty | 内华达州 | [alice@example.com](mailto:alice@example.com) |
| Eleven | Saint） | Twenty-one | 马萨诸塞州 | [zachery@example.com](mailto:zachery@example.com) |
| Twelve | 德尔马 | fifty-six | 爱达荷 | [delmar@example.com](mailto:delmar@example.com) |
| Thirteen | 丹尼斯 | Ninety-six | 俄亥俄州 | [dennie@example.com](mailto:dennie@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |
| Fifteen | 巴士拉 | Eighteen | 南达科塔州 | [busrah@example.com](mailto:busrah@example.com) |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |
| Eighteen | 中提琴 | Sixty-six | 缅因州 | [viola@example.com](mailto:viola@example.com) |
| Nineteen | Nadya | Twenty-two | 佛罗里达州 | [nadya@example.com](mailto:nadya@example.com) |
| Twenty | 伊莎贝拉！伊莎贝拉 | Sixty-one | 亚利桑那州 | [izabela@example.com](mailto:izabela@example.com) |

# 可以与`WHERE`子句一起使用来选择记录的运算符

您可以使用类似于`=`、`>`、`<`、`>=`、`<=`、`<>`(或`!=`，这取决于您的 SQL 版本)、`BETWEEN`、`LIKE`、`IN`的操作符。

在上面的例子中，我们已经看到了`>=`，“大于或等于”。

`=`是“等于”，`>`是“大于”，`<`是“小于”，`<=`是“小于等于”，`<>`(或`!=`)是“不等于”。

四个运算符，*大于*、*小于*、*大于等于*、*小于等于*在处理数字时最有用。

两个运算符，*等于*和*不等于*，对于数字和其他数据类型都很有用。

## 如何在 SQL 中使用`BETWEEN`运算符

`BETWEEN`允许您指定一个数字范围。例如`WHERE age BETWEEN 24 and 51`将选择该年龄范围内的所有记录。

```
SELECT * FROM users
WHERE age BETWEEN 24 AND 51;
```

有 7 个用户的年龄在此范围内:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| three | 砧骨 | Thirty-two | 南达科塔州 | [anvil@example.com](mailto:anvil@example.com) |
| four | 乔 | forty-four | 缅因州 | [jo@example.com](mailto:jo@example.com) |
| five | 梅瑞狄斯 | Forty-three | 特拉华河 | [meredith@example.com](mailto:meredith@example.com) |
| seven | 迪拉拉 | Fifty | 俄亥俄州 | [dilara@example.com](mailto:dilara@example.com) |
| eight | 科尔宾 | Forty-seven | 威斯康星州 | [corbin@example.com](mailto:corbin@example.com) |
| Ten | 爱丽丝 | Fifty | 内华达州 | [alice@example.com](mailto:alice@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |

## 如何在 SQL 中使用`LIKE`运算符

`LIKE`允许您指定一个模式。例如`WHERE name LIKE "A%"`将选择名称以 a 开头的所有记录。

```
SELECT * FROM users
WHERE name LIKE "A%";
```

我们的列表中有 5 个用户名以 A 开头的用户:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| three | 砧骨 | Thirty-two | 南达科塔州 | [anvil@example.com](mailto:anvil@example.com) |
| Ten | 爱丽丝 | Fifty | 内华达州 | [alice@example.com](mailto:alice@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |

### 如何使用`LIKE`制作图案

您可以使用字符`%`和`_`制作图案。字符`%`代表任意数量的字符(零个、一个或多个)。字符`_`恰好代表一个字符。

例如`"_ook"`可以是“书”、“看”、“nook”。但是`"%ook"`也可以是“ook”或“phonebook”。

## 如何在 SQL 中使用`IN`运算符

让您在一系列可能性中进行选择。例如，让我们看看哪些用户在东海岸。

```
SELECT * FROM users
WHERE state IN ("Maine", "New Hampshire", "Massachusetts", "Rhode Island", "Connecticut", "New York", "New Jersey", "Delaware", "Maryland", "Virginia", "North Carolina", "South Carolina", "Georgia", "Florida");
```

`IN`操作员正在检查`state`列中的值是否等于东海岸州列表中的某个值。

只有六个用户住在东海岸:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| four | 乔 | forty-four | 缅因州 | [jo@example.com](mailto:jo@example.com) |
| five | 梅瑞狄斯 | Forty-three | 特拉华河 | [meredith@example.com](mailto:meredith@example.com) |
| Eleven | 圣扎迦利 | Twenty-one | 马萨诸塞州 | [zachery@example.com](mailto:zachery@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |
| Eighteen | 中提琴 | Sixty-six | 缅因州 | [viola@example.com](mailto:viola@example.com) |
| Nineteen | Nadya | Twenty-two | 佛罗里达州 | [nadya@example.com](mailto:nadya@example.com) |

## 让我们不要忘记`IS`、`NOT`、`AND`、`OR`操作符

我们已经在上面的例子中使用了`IS`操作符。像`WHERE name IS "Cody"`一样，它检查一个列是否有确切的值。

你可以在一个条件前使用`NOT`使其相反。例如`WHERE age NOT BETWEEN 24 AND 51`将只选择小于 24 岁和大于 51 岁的用户。使用该标准，选择了 12 个用户:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 布赖恩 | Fifteen | 密歇根 | [brian@example.com](mailto:brian@example.com) |
| Two | 伦纳德 | Fifty-five | 密西西比河 | [leonard@example.com](mailto:leonard@example.com) |
| nine | 杜松子酒 | Sixty-three | 伊利诺伊 | [gin@example.com](mailto:gin@example.com) |
| Eleven | Saint） | Twenty-one | 马萨诸塞州 | [zachery@example.com](mailto:zachery@example.com) |
| Twelve | 德尔马 | fifty-six | 爱达荷 | [delmar@example.com](mailto:delmar@example.com) |
| Thirteen | 丹尼斯 | Ninety-six | 俄亥俄州 | [dennie@example.com](mailto:dennie@example.com) |
| Fifteen | 巴士拉 | Eighteen | 南达科塔州 | [busrah@example.com](mailto:busrah@example.com) |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |
| Eighteen | 中提琴 | Sixty-six | 缅因州 | [viola@example.com](mailto:viola@example.com) |
| Nineteen | Nadya | Twenty-two | 佛罗里达州 | [nadya@example.com](mailto:nadya@example.com) |
| Twenty | 伊莎贝拉！伊莎贝拉 | Sixty-one | 亚利桑那州 | [izabela@example.com](mailto:izabela@example.com) |

您使用`AND`来组合条件，使得两个条件都必须为真，例如`WHERE name LIKE "A%" AND age > 70`将选择姓名以大于 70 岁的**和**开头的用户。只有 2 名用户符合该标准:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |

您可以使用`OR`来组合条件，这样两个条件中只有一个需要为真。例如，`WHERE name LIKE "A%" OR age > 70`将选择姓名以**或**开头的 70 岁以上的用户(这两部分中只有一部分必须为真，但也可以都为真)。

有 6 个用户的姓名以 A 开头，或者年龄超过 70 岁(或者两者都有)。

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| three | 砧骨 | Thirty-two | 南达科塔州 | [anvil@example.com](mailto:anvil@example.com) |
| Ten | 爱丽丝 | Fifty | 内华达州 | [alice@example.com](mailto:alice@example.com) |
| Thirteen | 丹尼斯 | Ninety-six | 俄亥俄州 | [dennie@example.com](mailto:dennie@example.com) |
| Fourteen | ２８：１）　（圣经）亚伦（摩西之兄 | Fifty | 佛罗里达州 | [aaron@example.com](mailto:aaron@example.com) |
| Sixteen | 阿维尼翁 | Eighty-eight | 内华达州 | [aveline@example.com](mailto:aveline@example.com) |
| Seventeen | 谢林 | seventy-two | 阿肯色州 | [aherin@example.com](mailto:aherin@example.com) |

# 结论

指定要对表中的哪些记录进行操作非常重要。

通过这篇文章，您已经学会了如何使用`WHERE`子句来实现这一点。

感谢您的阅读！