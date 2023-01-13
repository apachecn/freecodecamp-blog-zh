# 主键 SQL 教程–如何在数据库中定义主键

> 原文：<https://www.freecodecamp.org/news/primary-key-sql-tutorial-how-to-define-a-primary-key-in-a-database/>

每个伟大的故事都始于身份危机。伟大的绝地大师卢克开始不确定- *“我是谁？”我怎么可能是重要人物？这需要尤达，那个拥有原力的人，来教他如何驾驭他的力量。*

今天，让我成为你的尤达。

我们将从如何选择主键开始，应对身份危机，然后以在数据库中创建主键的代码示例结束。

## 如何选择主键

你可能认为卢克是唯一一个有身份危机的人，但事实并非如此。创建数据库时，一切都处于身份危机中。这正是我们需要主键的原因:它们可以解决危机。他们告诉我们如何找到每个人。

假设你是政府，你想数字化地识别你的每一个公民。因此，您创建了这个数据库，其中包含了他们的所有信息:

```
First Name
Last Name
Passport Number
```

您选择护照号码作为主键——每个人的身份。你认为这就是你所需要的，因为护照上有地址和其他一切。你知道护照号码是唯一的，所以你觉得很好，并实施这个系统。

然后，几年后，你发现了一个丑陋的事实:整个国家都面临着身份危机。

每当某人的护照到期，他们就会得到一本新的。他们的身份改变了。其他系统继续使用旧的护照号码，所以他们现在指向幽灵人。

> 独一无二还不够。该值在该行的整个生命周期内不得更改。

然后，你会发现有些人甚至没有护照。你不能把它们输入你的系统，因为主键不能是`NULL`。你如何识别一个有`NULL`钥匙的人？

> 每一行必须有一个标识符。不允许空值。

下一次迭代意味着找到一个不随时间变化的标识符，一个每个人都有的标识符。在印度，这就是 Adhaar 卡。在美国，社会安全号码。

如果您正在创建一个数据库，请将它们作为您的主键。

有时候，你没有这样的钥匙。考虑一个还没有社会安全号码的国家，他们想要建立一个每个公民的数字记录。他们可以创建一个新的 SSN，或者他们可以利用数据库的力量，使用一个代理键。

代理键在现实世界中没有对等物。它只是数据库中的一个数字。所以，你在新的国家有这张桌子:

```
userID
First Name
Last Name
Passport Number
```

护照号码是唯一的。无论何时您想要获取用户的标识符，您都可以通过 Passport Number 来获取。

用户标识永远不会改变。护照号码可以改变，但它总是唯一的，所以你总是得到正确的用户。userID 是这个国家不存在的社会安全号码的替代者。

> 有趣的事实:这里的护照号码也是一个候选人的关键。它可能是主键，如果它从未改变的话。这是一个商业逻辑的区别。

主要要点是:**无论何时选择主键，都要考虑身份危机**。将来有人可能会更改他们的标识符吗？我们能进入一种多人拥有相同标识符的状态吗？

我以人作为例子，因为它使身份更加清晰——我们知道每个人都应该有自己的身份。将这种想法转移到你的数据库中。一切都有一个身份，这就是为什么你需要主键。

> 注意:有时，将多个列一起用作主键是可能的，也是可取的。这是一个组合键。

现在让我们用真实的代码例子来尝试定义主键。这里有两件事要做:首先，您将识别主键。然后，您将学习在数据库中定义它的语法。

## 真实世界的例子

假设您经营一家航运初创公司，非常像 Flexport。你有需要从一个地方运送到另一个地方的包裹，还有运送它们的船只。此外，您还有订购这些产品包的客户。

您认为您需要一个用于客户的表，一个用于包裹的表，一个用于运输的表，显示哪个包裹现在在哪里。

仔细考虑您将需要哪些列，以及什么应该是主键。如果你是 Flexport 的工程师，这是一个你必须解决的实际问题。没有什么是给定的，一切都是在现实世界中发现的。

给定这些信息，我会这样设计这些表:

```
Customers: first_name, last_name, email, address (for deliveries to their location)
Packages: weight, content
Transportation: <package_primary_key>, Port, time
```

我们缺少主键。在进一步阅读之前，先想想它们。

对于这个包，我将选择一个*代理* PackageID。我可以试着列出包裹的所有属性:重量、体积、密度、年龄。它们将唯一地标识该包，但是这在实践中很难做到。人们不关心这个，他们只关心包裹从一个地方到另一个地方。

因此，创建一个随机数并将其用作 ID 是有意义的。这正是为什么你会看到联邦快递，UPS，和每一个送货服务使用条形码和身份证。这些是为跟踪包而生成的代理键。

对于客户，我将选择一个*代理* CustomerID。在这里，我可以选择，比如说，我的客户的社会保险号。但是，客户不想与我分享这些信息，这样我就可以给他们发货了。因此，我们在内部生成一个密钥，不要告诉我们的客户这个密钥，继续称他们为 CustomerNo。345681.

> 有趣的故事:我知道一些公司，他们暴露了这个客户没有，客户坚持他们得到第一。这太搞笑了——工程师们实际上不得不把他们的前端代码改成:`if (cust == 345681) print(1);`

对于运输，我将选择一个*复合* PackageID+Port+time。这个更有意思一点。我也可以在这里创建一个*代理*，它也会工作得一样好。

但是，索引的魔力就在这里。主键自动获得索引，这意味着搜索比主键更有效。

当您在这个数据库中搜索时，大多数查询都是“这个包在哪里？”。换句话说，给定这个 PackageID，告诉我它现在的端口和时间。如果我没有将 PackageID 作为主键的一部分，我将需要一个额外的索引。

这听起来不错吧？最后一步，让我们在 SQL 中定义这三个表。语法会因您使用的数据库而略有不同。

## 在 MySQL 中定义主键

```
CREATE TABLE customers
( customerID  INT(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
  last_name   VARCHAR(30) NOT NULL,
  first_name  VARCHAR(25) NOT NULL,
  email		  VARCHAR(50) NOT NULL,
  address     VARCHAR(300)
);
```

```
CREATE TABLE packages
( packageID  INT(15) NOT NULL AUTO_INCREMENT,
  weight     DECIMAL (10, 2) NOT NULL,
  content    VARCHAR(50),
  CONSTRAINT packages_pk PRIMARY KEY (packageID) # An alternative way to above,
  # when you want to name the constraint as well.
);
```

```
CREATE TABLE transportation
( package 	INT(15) NOT NULL,
  port  	INT(15) NOT NULL,
  time	 	DATE NOT NULL,

  PRIMARY KEY (package, port, time),
  FOREIGN KEY package
  	REFERENCES packages(packageID)
	ON DELETE RESTRICT    # It's good practice to define what should happen on deletion. In this case, I don't want things to get deleted.

);
```

## 在 PostgreSQL 中定义主键

```
CREATE TABLE customers
( customerID  SERIAL NOT NULL PRIMARY KEY, # In PostgreSQL SERIAL is same as AUTO_INCREMENT - it adds 1 to every new row.
  last_name   VARCHAR(30) NOT NULL,
  first_name  VARCHAR(25) NOT NULL,
  address     TEXT,
  email		  VARCHAR(50) NOT NULL
);
```

```
CREATE TABLE packages
( packageID  SERIAL NOT NULL,
  weight     NUMERIC NOT NULL,
  content    TEXT,
  CONSTRAINT packages_pk PRIMARY KEY (packageID) # In PostgreSQL, this alternative way works too.
);
```

```
CREATE TABLE transportation
( package 	INTEGER NOT NULL,
  port  	INT(15) NOT NULL,
  time	 	DATE NOT NULL,

  PRIMARY KEY (package, port, time),

  FOREIGN KEY package
  	REFERENCES packages(packageID)
	ON DELETE RESTRICT    # It's good practice to define what should happen on deletion. In this case, I don't want things to get deleted.

);
```

没什么不同吧？一旦掌握了基础知识，只需快速浏览一下文档，就可以将其应用于几乎任何数据库。关键是要知道要找什么！

祝你好运，年轻的学徒。

喜欢这个吗？你可能也会喜欢我从一位高级软件工程师那里学到的东西