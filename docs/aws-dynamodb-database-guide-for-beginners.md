# AWS dynamo db–NoSQL 数据库初学者指南

> 原文：<https://www.freecodecamp.org/news/aws-dynamodb-database-guide-for-beginners/>

## 什么是 DynamoDB？

DynamoDB 是 AWS 的一个完全托管的 [NoSQL 数据库](https://www.mongodb.com/nosql-explained)。DynamoDB 类似于 MongoDB 等其他 NoSQL 数据库，除了您不需要做任何维护或扩展。

> DynamoDB 每天可以处理超过 10 万亿个请求，每秒可以支持超过 2000 万个请求的峰值——通过 AWS 文档。

DynamoDB 提供内置的安全性、按需和时间点备份、跨区域复制、内存缓存和许多其他支持业务关键型工作负载的功能。

最重要的是，DynamoDB 可以与 S3 和 Lambda 等其他 AWS 应用程序无缝协作。

但是在我们进入本文之前，理解 NoSQL 数据库的概念是很重要的。

## 什么是 NoSQL 数据库？

NoSQL 代表“**不仅仅是 SQL** ”。简单地说，NoSQL 数据库以类似于 JSON 的格式存储文档，而关系数据库以表格的形式存储数据。

NoSQL 在数据建模方面提供了更大的灵活性，并且不强迫您使用模式来存储文档。

一些类型的 NoSQL 数据库包括纯文档数据库(如 MongoDB)、键值存储(如 DynamoDB)、宽列数据库(如 Cassandra)和图形数据库(如 Neo4j)。[点击此处了解更多关于 NoSQL 数据库的信息](https://www.couchbase.com/resources/why-nosql)。

太好了。现在让我们看看 DynamoDB 的一些特性。

## DynamoDB 的核心特性

### 自动缩放

DynamoDB 最重要的特性可能是，它可以根据应用程序的性能或使用情况自动调整吞吐量和存储。

在典型的数据库服务器中，当应用程序遇到比平常高的流量时，系统管理员负责伸缩。

使用 DynamoDB，您可以创建可以存储和检索任意数量数据的数据库表，并且缩放由 AWS 自动管理。这包括针对较高流量的扩展和针对较低流量的缩减，因此您只需为您使用的流量付费。

### 数据模型

DynamoDB 支持键值和文档数据模型。这使您能够拥有一个灵活的模式，因此每一行在任何时间点都可以有任意数量的列。这对于需求不断变化的成长型企业至关重要。

在不断增长的应用程序中，重新定义数据库模式是许多开发人员/数据库管理员经历的一场噩梦。这种数据模型的灵活性为小型和大型企业提供了强大的数据库解决方案。

### 分身术

AWS 根据您选择的 AWS 区域自动处理 DynamoDB 表复制(跨区域复制)。使用 DynamoDB，即使是分布式应用程序也可以拥有个位数的毫秒级读写性能。

有了复制，您就不必担心数据可用性。如果主源发生故障，您可以轻松地从辅助备份中访问数据，从而降低应用程序停机的可能性。

### 备份和恢复

DynamoDB 为您的表提供了按需备份，您可以在 AWS 控制台中启用这些备份。您还可以将您的数据自动备份和归档到其他 AWS 解决方案，如 S3。

DynamoDB 还提供时间点恢复。这可以保护您的数据免受意外的写入/删除操作。

使用时间点恢复，您可以将数据库还原到最近 35 天内的任意时间点。时间点恢复是通过存储数据库的增量备份实现的，并由 AWS 自动管理。

### 安全性

默认情况下，DynamoDB 使用存储在 AWS 密钥管理服务中的密钥(或客户提供的密钥)对静态数据和传输中的数据进行加密。

加密到位后，您可以构建满足合规性和法规要求的安全敏感型应用程序。DynamoDB 还通过 [AWS IAM 角色提供访问控制。](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)

### 监视

监控对于任何业务关键型应用程序都至关重要。它有助于保持可靠性，并在发生事件或故障时通知相关人员。

AWS 提供了详细的监控工具，如 CloudWatch 日志、CloudWatch 事件和 CloudTrail 日志，这些工具将帮助您观察、通知和调试 DynamoDB 中的所有类型的事件。您还可以根据系统错误、容量使用等指标设置自定义触发器。

现在让我们将 DynamoDB 与两种流行的数据库替代方案——MySQL 和 MongoDB 进行比较。

## DynamoDB vs MySQL

MySQL 和 MongoDB 之间有一个主要的区别，因为 MySQL 是一个关系数据库。就好处而言，我认为 MySQL 是有限的，因为在开始推送数据之前需要有一个模式。

但是 MySQL 对于许多用例来说也很棒。它通常被称为“世界上最受欢迎的开源数据库”，它提供了一个快速、多线程、多用户和健壮的 SQL(结构化查询语言)数据库服务器。

但是作为一个 NoSQL 数据库，DynamoDB 在数据建模方面具有更大的灵活性。

尽管 AWS 为 MySQL 和其他关系数据库提供托管服务，但 DynamoDB 是由 AWS 设计的数据库，而不仅仅是托管数据库解决方案。因此，这提供了 MySQL 和其他关系数据库无法提供的更多改进和功能。

## DynamoDB vs MongoDB

DynamoDB 和 MongoDB 彼此密切相关，因为它们都是 NoSQL 数据库。但是由于 DynamoDB 是由 AWS 构建和维护的，与 MongoDB 相比，它提供了更多的功能和集成，特别是与 S3 等其他亚马逊服务的集成。

如果我在经营一家成长中的公司，我会更喜欢使用 DynamoDB，因为它具有可伸缩性和跨区域复制特性。AWS 不提供托管的 MongoDB 服务，但是如果你正在寻找这样的服务， [MongoDB Atlas](https://www.mongodb.com/atlas/database) 将是一个很好的选择。

DynamoDB 相对于 MongoDB 的另一个重要特性是，MongoDB 在默认情况下是不安全的，您必须自己配置安全性。DynamoDB 在默认情况下是安全的，所以如果安全性对您来说是一个障碍，那么它可能是一个更好的选择。

## 包扎

AWS DynamoDB 是一个完全托管的 NoSQL 数据库，可以根据需要进行扩展。AWS 负责典型的功能，包括软件修补、复制和维护。

DynamoDB 还提供静态加密、时间点快照和强大的监控功能。简而言之，当您构建一个需要高性能可伸缩 NoSQL 数据库的应用程序时，这是一个很好的选择。

****喜欢这篇文章吗？****[************加入我的简讯************](http://tinyletter.com/manishmshiva)*****每周一上午****。** 你也可以在这里* [**访问我的博客。**](https://www.hardcoder.io/)**