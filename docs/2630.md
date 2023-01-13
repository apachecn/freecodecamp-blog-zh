# 如何在微服务中处理日志

> 原文：<https://www.freecodecamp.org/news/how-to-handle-logs-in-microservices/>

日志是软件系统最重要的部分之一。无论您刚刚开始开发一个新的软件，还是您的系统正在大规模的生产环境中运行，您总是会发现自己在从日志文件中寻求帮助。

正因为如此，当出现问题或者事情没有按预期运行时，日志是开发人员首先要找的东西。

以正确的方式记录正确的信息使开发人员的工作变得更加容易。为了更好地记录日志，你需要知道记录什么以及如何记录。

在本文中，我们将了解一些基本的日志礼仪，它们可以帮助您最大限度地利用日志。

# 记录什么以及记录如何工作

让我们从一个电子商务系统的例子开始，看看登录它的订单管理服务(OMS)。

假设客户订单由于库存管理服务(IMS)的错误而失败，IMS 是 OMS 用来验证可用库存的下游服务。

让我们假设 OMS 已经接受了订单。但是在最后的验证步骤中，IMS 返回以下错误，因为库存中不再有该产品。

`404 Product Not Available`

## 记录什么

通常，您会以这种方式记录错误:

```
log.error(“Exception in fetching product information - {}”, ex.getResponseBodyAsString())
```

这将输出以下格式的日志:

```
[2020-09-27T18:54:41,500+0530]-[ERROR]-[InventoryValidator]-[13] Exception in fetching product information - Product Not Available
```

这个日志语句中没有太多可用的信息，不是吗？像这样的日志没有多大用处，因为它缺少关于错误的任何上下文信息。

我们是否可以在这个日志中添加更多的信息，使其与调试更加相关？添加订单 Id 和产品 Id 怎么样？

```
log.error("Exception in processing Order #{} for Product #{} due to exception - {}", orderId, productId, ex.getResponseBodyAsString())
```

这将输出以下格式的日志:

```
[2020-09-27T18:54:41,500+0530]-[ERROR]-[InventoryValidator]-[13] Exception in processing Order #182726 for Product #21 due to exception - Product Not Available
```

这很有道理！查看日志，我们可以了解到在处理订单#182726 时发生了一个错误，因为产品#21 不可用。

## 如何记录

虽然上面的日志对我们人类来说非常有意义，但它可能不是机器的最佳格式。让我们看一个例子来理解为什么。

假设某个产品(比如产品#21)的可用性出现问题，导致包含该产品的所有订单失败。您被分配了查找该产品所有失败订单的任务。

你高兴地在日志中为产品#21 做了一个`grep`,并兴奋地等待结果。当搜索完成时，您会得到类似这样的结果

```
[2020-09-27T18:54:41,500+0530]-[ERROR]-[InventoryValidator]-[13] Exception in processing Order #182726 for Product #21 due to exception - Product Not Available

[2020-09-27T18:53:29,500+0530]-[ERROR]-[InventoryValidator]-[13] Exception in processing Order #972526 for Product #217 due to exception - Product Not Available

[2020-09-27T18:52:34,500+0530]-[ERROR]-[InventoryValidator]-[13] Exception in processing Order #46675754 for Product #21 due to exception - Product Not Available

[2020-09-27T18:52:13,500+0530]-[ERROR]-[InventoryValidator]-[13] Exception in processing Order #332254 for Product #2109 due to exception - Product Not Available
```

和你想象的不太一样，对吗？那么如何改善这一点呢？结构化日志记录拯救世界。

# 什么是结构化日志记录？

结构化日志记录解决了这些常见问题，并允许日志分析工具提供额外的功能。以结构化格式编写的日志对机器更友好，这意味着它们可以很容易地被机器解析。

这在以下情况下会有所帮助:

*   开发人员可以搜索日志并关联事件，这对于开发过程以及解决生产问题都是非常宝贵的。
*   业务团队可以解析这些日志，并通过提取和汇总这些字段来对某些字段(例如，每天的唯一产品计数)进行分析。
*   您可以通过解析日志并在相关字段上执行聚合来构建仪表板(业务和技术)。

让我们使用前面的日志语句，做一点小小的修改，使它结构化。

```
log.error("Exception in processing OrderId={} for ProductId={} due to Error={}", orderId, productId, ex.getResponseBodyAsString())
```

这将输出以下格式的日志:

```
[2020-09-27T18:54:41,500+0530]-[ERROR]-[InventoryValidator]-[13] Exception in processing OrderId=182726 for ProductId=21 due to Error=Product Not Available
```

现在，通过使用“=”作为分隔符来提取`OrderId`、`ProductId`和`Error`字段，机器可以很容易地解析这个日志消息。我们现在可以对`ProductId=21`进行精确搜索来完成我们的任务。

这还允许我们对日志执行更高级的分析，例如准备一份报告，报告所有因这些错误而失败的订单。

如果您使用像 Splunk 这样的日志管理系统，查询`Error=”Product Not Available” | stats count by ProductId`现在可以产生以下结果:

```
+-----------+-------+
| ProductId | count |
+-----------+-------+
| 21        | 5     |
| 27        | 12    |
+-----------+-------+
```

我们还可以使用 JSON 布局以 JSON 格式打印日志:

```
{  
    "timestamp":"2020-09-27T18:54:41,500+0530"  
    "level":"ERROR"  
    "class":"InventoryValidator"  
    "line":"13"  
    "OrderId":"182726"  
    "ProductId":"21"  
    "Error":"Product Not Available"
}
```

理解结构化日志背后的方法很重要。没有固定的标准，可以通过许多不同的方式来实现。

# 结论

在本文中，我们看到了非结构化日志记录的缺陷以及结构化日志记录提供的好处和优势。

Splunk 等日志管理系统从结构良好的日志消息中获益匪浅，并且可以提供对日志事件的轻松搜索和分析。

然而，当涉及到结构化日志记录时，最大的挑战是为您的软件建立一个标准的字段集。这可以通过遵循定制日志模型或集中日志来实现，这可以确保所有开发人员在他们的日志消息中使用相同的字段。

谢谢你一直陪着我。希望你喜欢这篇文章。你可以在 LinkedIn 上和我联系，我经常在那里讨论技术和生活。也看看一些[我的其他文章](https://www.freecodecamp.org/news/author/theawesomenayak/)。快乐阅读。🙂