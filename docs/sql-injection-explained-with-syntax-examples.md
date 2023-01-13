# 用语法示例解释 SQL 注入

> 原文：<https://www.freecodecamp.org/news/sql-injection-explained-with-syntax-examples/>

## SQL 注入如何工作

SQL 注入是一种恶意技术，旨在损害或破坏数据库。这是最常见的网络黑客技术之一。

通过输入在 SQL 语句中放置恶意代码来执行 SQL 注入。

您可能以前听说过 SQL 注入。在这部著名的 XKCD 漫画中，这一点永垂不朽:

下面的示例是一个代码片段，它将基于`AccountId`从数据库中检索用户。

```
passedInAccountId = getRequestString("AccountId");
sql = "select * from Accounts where AccountId = " + passedInAccountId; 
```

通过为`AccountId`注入一个`1=1;`语句，SQL 注入可以用来破解这段代码。

`https://www.foo.com/get-user?AccountId="105 OR 1=1;"`

`1=1`将始终评估为`TRUE`。这将导致执行的代码输出所有的帐户表。

漫画:[https://imgs.xkcd.com/comics/exploits_of_a_mom.png](https://imgs.xkcd.com/comics/exploits_of_a_mom.png)