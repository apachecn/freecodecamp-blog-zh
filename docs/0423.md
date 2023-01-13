# 如何在几乎零停机的情况下升级 Postgres 主要版本——实用指南

> 原文：<https://www.freecodecamp.org/news/how-to-upgrade-postgres-major-versions-with-near-zero-downtime/>

跨主要版本创建零停机 Postgres 升级是不可能的——对吗？如果我说错了，请纠正我:)

但至少我们找到了接近零停机时间的方法。

*   PostgreSQL 升级很难！
*   [Retool 如何升级我们的 4 TB 主应用 PostgreSQL 数据库](https://retool.com/blog/how-we-upgraded-postgresql-database/)

在 [Listen Notes](https://www.listennotes.com/) ，自 2017 年 Listen Notes 成立以来，我们已经进行了两次 Postgres 主要版本升级。在这些升级过程中，我们经历了“读取”操作的零停机时间，以及不到 1 分钟的“写入”操作停机时间。

让我们回顾一下我们在 Listen Notes 上升级 Postgres 的过程。

### **TL；博士**

1.  使用旧版本的 Postgres 调配新的副本数据库(DB_A)。
2.  在联机服务器的“/etc/hosts”中更改数据库主机的 IP 地址，以使用只读副本数据库(不是 DB_A)。此时，所有写操作都将失败。
3.  在 DB_A 上运行 [pg_upgrade](https://www.postgresql.org/docs/current/pgupgrade.html) (带“- link”)升级到 Postgres 的新版本，将 DB_A 提升为主。
4.  将在线服务器的“/etc/hosts”中的所有数据库主机的 IP 地址替换为使用 DB_A。此时，写操作将恢复。
5.  使用新版本的 Postgres 重新调配新的复制副本节点。

## 我们如何在听力笔记中使用 Postgres

Listen Notes 是一个流行的播客搜索引擎。我们提供了一个月浏览量数百万的网站([ListenNotes.com](https://www.listennotes.com/))，以及一个被数千个第三方应用和网站使用的可靠的[播客 API](https://www.listennotes.com/api/) 。

我们使用 Postgres 作为我们的主数据库，它存储所有的播客、剧集元数据和用户数据。

我们在 AWS EC2 上运行一个自托管 Postgres 集群，由一个用于读写操作的主服务器(db1)和两个用于只读操作的副本服务器(db2 和 db3)组成。数据库大小略小于 1TB。

### Postgres 进度

2017 年开始听笔记的时候，我们运行的是 Postgres 9.6。

[然后我们在 2019 年升级到 Postgres 11.0](https://www.listennotes.fm/p/monthly-update-for-may-2019-19-05-31)。

到 2021 年，我们运行 Postgres 13.0 。

一般来说，我们不习惯使用任何基础设施软件的最新版本，无论是 Postgres、Django、Redis 还是其他软件。

我们相信 Postgres 的质量，但最新版本的文档和在线讨论可能会更少，这可能会在出现特定版本问题时给故障排除带来困难。

Listen Notes 与 Postgres DB 对话的服务器使用的主机名有 db1.internal.ln、db2.internal.ln、db3.internal.ln 等等。

这些主机名位于“/etc/hosts”中，因此很容易在保持主机名不变的同时更改实际的 IP 地址。

我们的 Postgres 集群有在线和离线工作负载。在线工作负载是服务于我们的网站(ListenNotes.com)和 API 端点(PodcastAPI.com)，不能有长时间的停机时间(例如，超过 5 分钟)。

离线工作负载运行 Celery 任务和其他可以停止相对较长时间(例如，2 小时)的脚本。你可以阅读我们过去的博客文章来了解 Listen Notes 架构的细节:

*   [一个人互联网公司背后的无聊技术](https://www.listennotes.com/blog/the-boring-technology-behind-a-one-person-23/)
*   [足够好的工程技术，可以创办一家互联网公司](https://www.listennotes.com/blog/good-enough-engineering-to-start-an-internet-27/)
*   [我是如何意外构建了一个播客 API 业务](https://www.listennotes.com/blog/how-i-accidentally-built-a-podcast-api-business-46/)

对于跨主要版本升级 Postgres，我们的目标是实现读操作的零停机时间和写操作的最小停机时间(少于 5 分钟)。这将确保我们的大多数用户在升级期间不会受到影响。

## **如何准备**进行 Postgres 升级****

实际升级可能只需要 30 分钟，但我们通常会花几个工作日来准备，这增加了升级成功的几率。

### 准备步骤 1:

我们必须确保 Postgres 的新主版本能很好地与我们的代码库兼容。因此，我们在开发和登台测试新版本的 Postgres。

除了自动单元测试之外，我们还必须手工测试所有主要的产品特性。

### 准备步骤 2:

在线服务([ListenNotes.com](https://www.listennotes.com/)和[PodcastAPI.com](https://podcastapi.com/))应该对大多数用户有效，即使 Postgres 写操作被禁用。

对于 ListenNotes.com 来说，大多数用户都在执行“只读”任务，比如搜索播客、浏览播客细节以及类似的无害操作。这意味着“写”失败只会影响一小部分用户。

对于 PodcastAPI.com，所有 API 端点都是只读的，或者将写操作卸载到异步离线任务，因此可以暂时禁用写操作。

我们会花一些时间对暂存进行测试，以确保当数据库写入被禁用时，在线服务仍然可以正常工作。

### 准备步骤 3:

我们花最多的时间*排练*升级 Postgres 的过程。基本上，我们提供整个系列的听力笔记，并练习升级 Postgres 的所有必要步骤。

我们尝试在 Ansible 或 Bash 脚本中编写一些步骤来实现自动化。我们记录并记录每一步。当我们在生产环境中执行它时，我们知道每一步要花多少分钟(甚至几秒钟)。

### 准备步骤 4:

我们练习如何快速回滚到旧版本的 Postgres，以防升级失败，我们被迫尽快恢复稳定的环境。

## **如何升级**Postgres****

我们通常在周五晚上进行实际升级，此时网站和 API 流量较低。此外，我们必须在白天好好休息，以保存足够的能量在当天晚上晚些时候执行生产中如此危险和紧张的操作:)

由于我们在前几天的准备过程中已经在 idea 中创建了一个详细的待办事项列表，我们仔细地按照待办事项列表来升级 Postgres:

### 升级步骤 1:

我们使用旧版本的 Postgres 提供一个新的只读副本数据库。让我们称它为 DB_A。它将实时同步主数据库中的数据，并将首先升级到 Postgres 的新版本，然后提升为主数据库。

如果 DB_A 上的升级稍后失败，我们仍然可以选择快速回滚并使用旧的主数据库。

### 升级步骤 2:

我们停止了所有离线任务，除了一个 Celery worker 处理一些时间敏感的异步任务，比如发送登录电子邮件。我们将在第 4 步之前阻止这个芹菜工人。

我们还将大多数 web/API 服务器从负载均衡器中移除，只留下少量在线服务器。

### 升级步骤 3:

我们在最小的在线服务器(例如 web、API……)上的“/etc/hosts”中更改所有数据库主机的 IP 地址，以使用旧的只读数据库。姑且称之为 DB_B 吧。

从这一点开始，所有写操作都应该失败。这一步是为了确保未来新的主数据库不会有过时的数据。

### 升级步骤 4:

我们在 DB_A 上运行 pg_upgrade(带有“- link”)，以升级到 Postgres 的新版本，并将其提升为主 DB。从现在开始，DB_A 是主要的，运行 Postgres 的新版本。

### 升级步骤 5:

我们将最小在线服务器(例如 web、API……)的“/etc/hosts”中出现的所有 DB_B 的 IP 替换为 DB_A。此时，DB_A 同时用作主服务器和副本服务器。写操作现在应该不错了。

### 升级步骤 6:

我们将“/etc/hosts”更改为对所有其他服务器上的所有数据库主机(主服务器+副本服务器)使用 DB_A，并恢复离线任务。

从用户的角度来看，所有的 Listen Notes 服务现在都应该是正常的。事实上，在整个升级过程中，所有 API 用户都不会遇到任何中断，而一小部分网站用户可能会在执行“写操作”时遇到错误，例如创建播客播放列表或剪辑。

### ****升级到新版 Postgres 最重要的一步****

其中，第四步最为关键。如果它失败或运行时间过长(例如，超过 10 分钟)，那么我们必须通过在那些在线服务器上更改“/etc/hosts”来回滚。

根据我们的经验，运行第 4 步用时不到 1 分钟。如果你有一个更大(或更小)的数据库，你的里程可能会有所不同。

在我们确保第 6 步后一切恢复正常后，我们可以使用新版本的 Postgres 重新配置副本数据库实例。最终，我们会终止旧的数据库实例。

## 最后的想法

听起来很复杂？是啊，有点…

生产中的数据库操作本质上是复杂而危险的。不能催流程:)

![747a0d713bc541d8bc063df623d35ee0](img/c9abbc86fd7cd66aef698bbd1cdf2b4d.png)

## ****常见问题****

### 你为什么不用托管的 Postgres，比如亚马逊 RDS？

我们希望完全控制关键的基础设施软件(例如，Postgres、Elasticsearch…)，因为…

1.  我们不希望平台锁定
2.  我们希望了解服务器内部的情况，避免无助地等待第三方客户支持团队(例如 AWS)帮助解决黑盒内部的紧急生产问题
3.  对我们来说，自己运行 Postgres 实例更具成本效益——如果资金不是问题(例如，筹集大笔风险投资资金)或者如果我们没有多少 Postgres 运营经验，那么 Amazon RDS 可能是一个不错的选择。就像生活中的很多事情一样，我们需要做有约束的事情(比如金钱、时间、专业知识……)。

据我所知，使用一个托管的 Postgres(像亚马逊 RDS)并不会消除跨主要版本升级的痛苦:Google“[亚马逊 RDS 零宕机升级 Postgres 版本](https://www.google.com/search?q=amazon+rds+upgrade+postgres+versions+zero+downtime)”。

### 你为什么不使用第三方工具来自动化这个过程呢(比如一键操作来自动化整个过程)？

我们不知道是否有任何可靠的第三方工具易于使用、易于理解、使用安全……但我们愿意接受建议—[wenbin@listennotes.com](mailto:wenbin@listennotes.com)。

我们经常需要评估在生产中学习和使用新的黑盒工具是否值得花费时间和风险，特别是对于严肃的 DevOps 任务。

### 为什么不用 MySQL，MongoDB，或者其他非 Postgres 的数据库？

当我开始使用 Listen Notes 时，我对 Postgres 的了解远远超过 MySQL 和其他数据库，因为我以前的雇主 Nextdoor.com 也使用 Postgres。我知道 Instagram 和其他大型在线服务也使用 Postgres 作为他们的主要数据存储(至少在最初几年)。

如果 Postgres 适用于大型在线服务，那么它也应该适用于 Listen Notes:)有时我们花时间学习新技术来启动项目，但更多时候我们只是使用我们已经知道的技术，以便更快、更有效地启动项目。

同样，跨主要版本升级非 Postgres 数据库也不容易……但希望以上所有步骤有助于成功升级 Postgres 数据库！

*这篇博文最初发表于[ListenNotes.com](https://www.listennotes.com/blog/a-practical-way-to-upgrade-postgres-major-49/)。*