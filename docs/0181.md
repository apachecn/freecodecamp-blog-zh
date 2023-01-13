# 什么是 ORM——对象关系映射数据库工具的含义

> 原文：<https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/>

对象关系映射(ORM)是一种用于在面向对象的程序和大多数情况下的关系数据库之间建立“桥梁”的技术。

换句话说，你可以把 ORM 看作是连接面向对象编程和关系数据库的层。

当使用 OOP 语言与数据库交互时，您将不得不执行不同的操作，比如从数据库中创建、读取、更新和删除(CRUD)数据。按照设计，您使用 SQL 在关系数据库中执行这些操作。

尽管为此目的使用 SQL 并不一定是个坏主意，但是 ORM 和 ORM 工具有助于简化关系数据库和不同 OOP 语言之间的交互。

## 什么是 ORM 工具？

ORM 工具是设计用来帮助 OOP 开发者与关系数据库交互的软件。因此，您可以利用这些工具，而不是从头开始创建自己的 ORM 软件。

下面是一个从数据库中检索特定用户信息的 SQL 代码示例:

```
"SELECT id, name, email, country, phone_number FROM users WHERE id = 20"
```

上面的代码从名为`users`的表中返回关于用户的信息— `name`、`email`、`country`和`phone_number`。使用`WHERE`子句，我们指定信息应该来自一个`id`为 20 的用户。

另一方面，ORM 工具可以用更简单的方法完成与上面相同的查询。那就是:

```
users.GetById(20)
```

所以上面的代码做的和 SQL 查询一样。请注意，每个 ORM 工具都是以不同的方式构建的，因此方法也不会相同，但一般目的是相似的。

ORM 工具可以生成类似上一个例子中的方法。

大多数 OOP 语言都有多种 ORM 工具可供选择。下面是一些最流行的 Java、Python、PHP 和。净发展:

### 面向 Java 的流行 ORM 工具

#### 1.冬眠

Hibernate 使开发人员能够按照 OOP 的概念编写数据持久类，比如继承、多态、关联、组合。Hibernate 是高性能的，也是可伸缩的。

#### 2.Apache OpenJPA

[Apache OpenJPA](https://openjpa.apache.org/) 也是一个 Java 持久化工具。它可以作为一个独立的 **POJO** (普通旧 Java 对象)持久层。

#### 3.EclipseLink

EclipseLink 是一个针对关系、XML 和数据库 web 服务的开源 Java 持久性解决方案。

#### 4.jOOQ

jOOQ 从存储在数据库中的数据生成 Java 代码。您还可以使用该工具来构建类型安全的 SQL 查询。

#### 5.甲骨文 tplink

您可以使用 [Oracle TopLink](https://docs.oracle.com/cd/E17904_01/web.1111/b32441/undtl.htm#JITDG91126) 构建存储持久数据的高性能应用程序。数据可以转换成关系数据或 XML 元素。

### Python 的流行 ORM 工具

#### 1.姜戈

Django 是一个快速构建 web 应用程序的伟大工具。

#### 2.web2py

web2py 是一个开源的全栈 Python 框架，用于构建快速、可伸缩、安全和数据驱动的 web 应用。

#### 3.SQLObject

SQLObject 是一个对象关系管理器，它为你的数据库提供了一个对象接口。

#### 4.sqllcemy(SQL 语法)

[SQLAlchemy](https://www.sqlalchemy.org/) 提供了为高效和高性能的数据库访问而设计的持久性模式。

### PHP 的流行 ORM 工具

#### 1.拉勒韦尔

Laravel 带有一个名为雄辩的对象关系管理器，使得与数据库的交互更加容易。

#### 2.CakePHP

CakePHP 提供了两种对象类型:存储库和实体，前者让你可以访问数据集合，后者代表数据的单个记录。

#### 3.克哥多

[Qcodo](https://github.com/qcodo/qcodo) 提供不同的命令，可以在终端运行，与数据库交互。

#### 4.RedBeanPHP

RedBeanPHP 是一个零配置的对象关系映射器。

### 流行的 ORM 工具。网

#### 1.实体框架

[实体框架](https://learn.microsoft.com/en-us/ef/)是一个多数据库对象-数据库映射器。它支持 SQL、SQLite、MySQL、PostgreSQL 和 Azure Cosmos DB。

#### 2.nhiberinated(无国籍人士)

NHibernate 是一个开源的对象关系映射器，拥有大量的插件和工具，使开发变得更加容易和快速。

#### 3.衣冠楚楚的

[Dapper](https://www.learndapper.com/) 是一种微 ORM。它主要用于将查询映射到对象。这个工具不做 ORM 工具会做的大部分事情，比如 SQL 生成、缓存结果、延迟加载等等。

#### 4.Base One 基础组件库(BFC)

BFC 是一个使用微软、甲骨文、IBM、Sybase 和 MySQL 的 Visual Studio 和 DBMS 软件创建网络数据库应用程序的框架

你可以在这里看到更多的 ORM 工具[。](https://en.wikipedia.org/wiki/List_of_object%E2%80%93relational_mapping_software)

现在让我们讨论一下使用 ORM 工具的一些优点和缺点。

## 使用 ORM 工具的优势

以下是使用 ORM 工具的一些优势:

*   它加快了团队的开发时间。
*   降低开发成本。
*   处理与数据库交互所需的逻辑。
*   提高安全性。ORM 工具旨在消除 SQL 注入攻击的可能性。
*   使用 ORM 工具比使用 SQL 编写的代码更少。

## 使用 ORM 工具的缺点

*   学习如何使用 ORM 工具可能很耗时。
*   当涉及非常复杂的查询时，它们可能不会表现得更好。
*   ORM 通常比使用 SQL 要慢。

## 摘要

在本文中，我们讨论了对象关系映射。这是一种用于将面向对象的程序连接到关系数据库的技术。

我们为不同的编程语言列出了一些流行的 ORM 工具。

我们总结了使用 ORM 工具的一些优点和缺点。语言。

编码快乐！