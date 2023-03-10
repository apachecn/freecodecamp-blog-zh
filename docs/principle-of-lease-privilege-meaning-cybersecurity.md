# 最小特权原则——网络安全的定义和含义

> 原文：<https://www.freecodecamp.org/news/principle-of-lease-privilege-meaning-cybersecurity/>

在过去的三十年里，信息技术对我们的生活产生了深远的影响。它帮助我们创建全球企业，改造行业，建立强大的联系。

但这也增加了安全和隐私方面的风险。个人和企业比以往任何时候都更容易受到网络攻击。最近各种企业的数据泄露(以及最终的破产)向我们展示了拥有强大网络防御机制的重要性。

考虑到拥有内部网络安全团队的成本，大多数小型企业都面临数据泄露的风险。正如前联邦调查局局长罗伯特·穆勒(Robert S. Mueller)所说，“只有两种类型的公司:已经被黑客攻击的公司和将要被黑客攻击的公司”。

那么，什么是企业可以开始实施的可扩展且经济高效的解决方案呢？我们可以从一个开始:最小特权原则。

## 最小特权原则是什么？

最小特权原则(PoLP)是限制组织成员访问资源的实践。简而言之，如果某人不需要访问资源，他们就不应该拥有它。

尽管有这样的逻辑陈述，PoLP 很少被实现。组织中的每个人都应该只拥有足够的权限来完成他们特定的工作职能。不多不少。

一个成员有多熟练或多值得信任并不重要。最小特权原则是公司安全的重要组成部分。由于政府坚持将网络违规行为公之于众，正确的访问控制是企业保护自己免受金钱和名誉损失的唯一途径。

## 如何实现最小特权原则

那么一个组织如何实现 PoLP 呢？这里有五种方法可以开始。

### 基于角色的访问管理

管理个人用户的访问本身就是一项挑战。增加安全性会使它变得更加困难。这就是基于角色的访问可以帮助实现这两个目标的地方。

组织成员可以根据他们的工作职能分组，例如，开发人员、系统管理员和人力资源专业人员。每个组都可以拥有自己的组织资源权限集。

这使得访问控制的实现更具可伸缩性。添加/删除用户就是将他们添加到各自的组中。基于角色的访问还消除了在员工转换期间撤销个人对服务的访问的需要。

### 多因素身份认证(MFA)

MFA 是实现对组织服务的安全访问的另一种方式。使用 MFA 使得使用员工凭证来访问关键业务资产变得更加困难。

有三种类型的 MFA 方法:

*   您知道什么(密码、个人识别码)
*   您拥有什么(徽章、智能手机身份验证)
*   你是什么(指纹和其他生物识别)

### 即时访问管理

在处理大量人员时，雇主经常会在打开和关闭访问权限方面遇到困难。如果长时间不关闭对外部人员的访问，这可能会成为一个严重的漏洞。

即时访问管理允许管理员授予对资源的临时访问权限。授予有截止日期的访问权限是保护资源的最佳方式，因为它消除了在工作功能完成后删除访问权限的需要。

### 审查跟踪

审计跟踪记录了组织中每个员工执行的每个操作。在部署基于人员的安全措施时，使用审计跟踪有很多好处。

拥有审计线索有助于防止攻击，并追踪攻击的源头。在社会工程攻击期间，处于较低级别的员工更容易受到攻击。如果员工账户出现违规情况，公司可以通过使用明确定义的审计线索来避免进一步升级。

### 安全策略

拥有一套安全策略对于防止网络攻击至关重要。这些策略包括从密码策略到资源共享策略。记录一套安全策略也有助于其他成员做出明智的决策。

从开始，这里有一个伟大的网络安全政策列表。

## 实施最小特权原则的挑战

寻找安全性和可用性之间的完美平衡一直是企业面临的挑战。下面是在尝试实现 PoLP 时会遇到的一些挑战。

### 传统应用程序

遗留应用程序对于任何安全从业者来说都是一个挑战。如果应用程序属于第三方，它会增加确保公司安全所需的工作量。如果很难从遗留服务过渡，企业应该采取措施限制非管理员用户的管理员访问权限。

您还应该将应用程序更新到最新版本。大多数软件更新都是安全补丁。如果不是来自可信的供应商，即使是现代应用程序也可能存在与访问相关的漏洞。在将每个应用程序部署到公司之前，对其进行验证是非常重要的。

### 员工挫折感

实施强有力的安全策略通常会导致员工感到沮丧。这是因为安全协议越宽松，员工的工作就越容易。不幸的是，这迟早会导致该组织的垮台。

定期的网络安全培训可以帮助员工理解安全实践的重要性。此培训还可以帮助提高保护手机和笔记本电脑等个人资源的意识。

### 缺乏意识

员工可以通过培训了解网络风险，但这不适用于供应商和销售商。即使一家公司的系统是安全的，第三方供应商通常也承担着巨大的风险。

企业可以邀请他们的供应商和销售商参加网络安全意识项目。但是这并不能保证安全的实践，也不具有可伸缩性。强大的第三方安全策略是减轻这些外部风险的更好方法。

## 最小特权原则的好处

如果实施得当，PoLP 可以为任何企业提供强大的安全屏障。以下是一些好处。

### 数据安全

PoLP 的核心目的是消除特权升级。大多数违规从较低级别开始，然后由恶意行为者升级。使用 PoLP 练习可以减缓攻击者的速度，并为防御者提供战斗机会。

### 安全可扩展性

如果有强有力的 PoLP 实践，企业可以放心地进行扩展。幸运的是，随着组织扩展到更多的资源，PoLP 成倍增加了组织的安全性。与大型企业相比，在小规模架构中实施 PoLP 也更容易。

### 法规遵循

根据业务的性质，法规遵从性在大多数国家都是强制性的。PoLP 是大多数合规性要求中反复出现的话题。这包括医疗保健提供商的 [HIPPA 合规性](https://compliancy-group.com/what-is-hipaa-compliance/)、支付处理商的 [PCI-DSS 合规性](https://www.imperva.com/learn/data-security/pci-dss-certification/)以及许多其他合规性。

### 降低第三方风险

第三方提供商总是攻击的媒介。考虑到企业对供应商安全实践的影响有限，准备是关键。确保始终遵循 PoLP 实践有助于减轻组织的众多外部风险。

### 改进的事件响应

在大多数国家，首席执行官对安全漏洞负有责任。在网络事故的不幸事件中，像审计跟踪这样的 PoLP 工具将决定一个公司的成败。识别和遏制事故对于保护企业免受严重损害甚至破产至关重要。

## 真实世界的事件

*   [2013 年塔吉特安全漏洞](https://www.usatoday.com/story/money/2017/05/23/target-pay-185m-2013-data-breach-affected-consumers/102063932/) —塔吉特在安全漏洞后不得不支付 1850 万美元的赔偿金。黑客通过能够访问目标网络的第三方供应商获得了对目标系统的访问权。
*   [Solarwinds 违规](https://www.techtarget.com/whatis/feature/SolarWinds-hack-explained-Everything-you-need-to-know) —攻击者获得了 Solarwinds 众多全球(完全特权)账户中的一个。这导致了 21 世纪最大的违规事件之一，甚至影响到了美国政府。
*   美国国家安全局斯诺登泄密事件——可以说是有史以来最著名的安全漏洞，爱德华·斯诺登拥有管理员级别的资源访问权限，帮助他复制了 170 万份美国国家安全局文件。国家安全局消除了一些系统管理员，以改善 PoLP 的姿态。

## 摘要

缺乏网络安全知识的企业往往会忽视网络攻击的危险。恶意行为者可以让企业陷入停滞，甚至让整个公司破产。

最小特权原则是保护组织的第一步，也是最重要的一步。实施 PoLP 并不能保护一个组织免受所有的网络攻击，但是缺乏 PoLP 会使它变得非常容易。

喜欢这篇文章吗？加入**[Stealth Security Weekly Newsletter](https://stealthsecurity.io/)**，让文章每周五送达你的收件箱。你也可以[在 LinkedIn 上和我](https://www.linkedin.com/in/manishmshiva/)联系。