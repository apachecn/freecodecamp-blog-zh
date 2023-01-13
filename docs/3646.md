# REST APIs

> 原文：<https://www.freecodecamp.org/news/rest-apis/>

## 历史

REST 代表****Re**presental**T5**S**tate**T**传输协议。罗伊·菲尔丁在 2000 年的博士论文中定义了休息。

## 什么是 REST API？

REST 的开发是为了给

*   识别资源
*   操纵资源
*   自我描述消息
*   使用超媒体作为应用状态的引擎(HATEOS)

## 最佳实践

### 基础

**法**|【http://api.co/v2/cars】|[|](http://api.co/v2/cars)|【http://api.co/v2/cars/1234】|

*   获取||列出所有汽车||检索单辆汽车
*   POST ||创建新汽车||错误
*   放置||更换汽车系列||将 id 为 1234
    的汽车更换为新车
*   删除||删除所有汽车||删除 id 为 1234 的汽车

*注意，在上传操作时，客户端或服务器都可以生成 id*

### 名词是好的动词是坏的

*   用名词指代`cars`、`fruits`等资源。
*   动作声明使用动词`convertMilesToKms`，`getNutritionalValues`

### 单数还是复数？

使用正确的语法进行声明

****忌**** 忌`/person/145`

****更喜欢**** `/people/154`假定从人员列表中返回第 154 人

### 用例

使用以下任一模式，并保持 ****一致！****

case style 示例 ****大写**** `http://api.fintech.cp/DailyTransactions/Today` ****小写****`http://api.fintech.cp/dailyTransactions/today`****snake _ case****`http://api.fintech.cp/daily_transactions/today`

### 关系和资源

*   资源可以有`one-to-many`、`many-to-many`、`many-to-one`等关系。正确映射它们至关重要。

****一对多**** 映射

例如，`Tickets/145/messages/4`表示`tickets`和`messages`之间的一对多关系。含义`1`票上有`N`的消息。消息不是独立的资源。不能有`/messages/4`。

****多对多**** 映射

例如，`/usergroups/345/users/56`建议选择第 345 个用户组，获取 id 为 56 的用户。然而，一个用户可能在多个`usergroups`中，即`/usergroups/209/users/56`也有效。在这种情况下，将从属资源`users`分离成一个独立的端点，如`/users/56`，并在`/usergroups/209/users/56`中提供资源链接

### API 参数

*   ****路径**** : *需要*才能访问资源，例如`/cars`，`/fruits`
*   ****查询参数**** : *可选*过滤列表例如`/cars?type=SUV&year=2010`
*   ****正文**** :资源特定逻辑。高级搜索查询。有时它可能既有查询又有正文。
*   ****表头**** :应包含全局或全平台数据。例如 API 密钥参数、用于 auth 的加密密钥、设备类型信息(例如移动或桌面或端点)、设备数据类型(例如 xml 或 json)。使用报头来传达这些参数

### HTTP 状态代码

使用正确的状态代码

CodesMeaning1xxRequest 已收到并已理解。2x 客户端请求的操作已收到、已理解并已请求。3xx 客户端必须采取其他操作来完成请求。这些状态代码大多用于 URL 重定向。4xx 适用于错误似乎是由客户端引起的情况。5xx 服务器无法满足请求。

小更上**2xx**T3！

*   ****201 资源创建完毕。**** POST `/cars`应该返回用`location`头创建的 HTTP 201，说明如何访问资源，例如头中的`location` : `api.com/cars/124`

****202 -已接受****

如果任务很大，请使用此选项。告诉客户端，它已经接受了请求，并将/正在处理/处理没有有效负载返回

****204 -无内容****

删除时使用`DELETE cars/124`不返回内容。但是如果 API 打算发送删除的资源进行进一步处理，也可以返回`200 OK`。

危险的 ****5xx**** 资源！

*   ****500**** 内部服务器出错
*   ****504**** 网关超时。服务器没有收到及时的响应

不太为人所知的 ****4xx**** 表明你在传递错误的参数。也会传递错误的信息。例如

`DELETE /cars/MH09234`

返回`4xx`或消息`Expecting int car id /car/id got string car/MH09234`