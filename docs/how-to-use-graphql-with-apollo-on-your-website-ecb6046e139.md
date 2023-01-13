# 如何在你的网站上使用 GraphQL 和 Apollo

> 原文：<https://www.freecodecamp.org/news/how-to-use-graphql-with-apollo-on-your-website-ecb6046e139/>

在我之前的文章中，我解释了为什么将网站的前端部分从后端服务中分离出来是有意义的。我介绍了 GraphQL、Apollo 和其他工具，它们支持这种抽象，并使产品网站的维护成为一种美好的体验。

在本文中，我将向您展示一个样板文件，它已经设置好了所有这些工具，并在开始开发时为您节省了大量时间。

[查看样板文件的现场演示](http://bit.ly/2GGHIB5)

# 加快开始的样板文件

让我们从我使用的工具开始:

*   Node.js —运行时
*   Express — web 应用程序框架
*   Apollo server——支持 GraphQL 的中间件服务
*   Apollo 客户端— GraphQL 客户端
*   Kentico 云工具—无头 CMS
*   Pug —模板引擎

# 模式和解析器代码

构建站点的第一步是创建或生成一个模式。我在上一篇文章中已经提到，我正在使用[内容即服务平台 Kentico Cloud](http://bit.ly/2QzUALM) 进行内容存储。存储在那里的内容已经在定义的模型结构中进行了结构化。因此，我可以使用[模式生成器](http://bit.ly/2SqEAju)快速生成模式:

kc-generate-gql-schema —项目 id {项目 ID} — createModule

但是也可以用下面的语法手动定义所有的模型。

const TYPE _ DEFINITION = `

TYPE system info {
id:String！
名称:字符串！
代号:弦！
语言:字符串！
类型:字符串！
lastModified: String！
}

界面 content item {
system:system info！
}
...
type FactAboutUsContentType 实现 ContentItem {
system:system info！
描述:RichTextElement
标题:TextElement
图片:AssetElement
}
...` module . exports = {
TYPE _ DEFINITION
}

**(参见全文件**[**【GitHub】**](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/master/graphQL/types.js)**)。)**

模型生成器列出了所有的系统类型，包括链接、文本、日期时间字段、图像和其他(上面的`SystemInfo`)，后面是每个定制内容模型的数据模型(`FactAboutUsContentType`)。我们将需要使用类型定义作为模块，因此最后一个参数`createModule`。

下一步是创建 GraphQL 查询和解析器。因为内容 API 是只读的，所以查询非常简单，仅限于获取所有项目或按类型分组的项目:

const queryTypes = `
类型查询{
项:[内容项]，
项 ByType(类型:字符串！，limit: Int，depth: Int，order:String):【content item】
}
`；

**(参见全文件**[**【GitHub】**](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/master/graphQL/queries.js)**)。)**

在定义之后，我们可以为 headless CMS API 创建一个解析器:

const delivery client = new delivery client(delivery config)；
常量解析器= {
...
查询:{
items:async()=>{
const response = await delivery client . items()
。get promise()；
返回 response.items
}，
itemsByType: async (_，{ type，limit，depth，order })=>{
const query = delivery client . items()
。类型(type)；
限制& & query.limitParameter(限制)；
深度&query . depth parameter(深度)；
订单& & query.orderParameter(订单)；
const response = await query . get promise()；
返回 response.items
}
}，
}；

**(参见全文件**[**【GitHub】**](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/master/graphQL/queries.js)**)。)**

您是否注意到查询总是返回通用类型`ContentItem`，即使有更多的特定类型，比如继承了定义的`ContentItem`的`FactAboutUsContentType`？如果你做到了，干得好！为每一种类型定义一个特定的查询是低效的(会有很多类型)。因此，我们的两个查询都返回`ContentItem`数据。但是我们如何确保在运行时返回正确的模型呢？

来自 headless CMS 的每个内容项都包含有关其类型的信息。你可以在上面的`SystemInfo`数据模型的定义中看到字符串属性`Type`。

{
"系统":{
"类型":"事实 _ 关于 _ 我们"
...
}
...
}

现在我们知道内容项属于类型`fact_about_us`，它对应于生成的数据模型`FactAboutUsContentType`。因此，我们需要将类型名转换成 pascal 大小写，并确保 GraphQL 使用正确的数据模型。我们可以使用通用数据模型的特殊解析器来确保这一点:

...
const resolvers = {
content item:{
_ _ resolve type(item，_context，_ info){
//fact _ about _ us->fact about us
const type = convertsnakecaspascalcase(item)；
//FactAboutUs->FactAboutUsContentType
返回类型+' content type '；
}
}，
...

**(参见全文件**[**【GitHub】**](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/master/graphQL/queries.js)**)。)**

并添加一个简单的函数来将类型名转换为数据模型名:

...
//fact _ about _ us->fact about us
const convertSnakeCaseToPascalCase =(item)=>{
return item . system . type
。
(_’)分裂。map((str) = > str.slice(0，1)。toUpperCase() + str.slice(1，str.length))
。联接(“”)；
}
...

**(参见全文件**[**【GitHub】**](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/master/graphQL/queries.js)**)。)**

您会看到，对于解析器的实现，您需要了解目标服务 API，或者在这种情况下了解 SDK 的细节。无论您使用什么服务，从事前端工作的开发人员只需要知道 GraphQL 模式。

# 把所有的放在一起

为了实现我们的数据模型、查询和解析器，我们需要在主`app.js`文件中创建 Apollo 服务器实例，并将其与 Express 和我们的 GraphQL 模式定义连接起来:

const { TYPE_DEFINITION } = require('。/graph QL/types ')；
const { queryTypes，resolvers } = require('。/graph QL/queries ')；
const app = express()；
const Apollo server = new Apollo server({
自省:true，
游乐场:true，
typedef s:[
TYPE _ DEFINITION，
queryTypes
]，
解析器
})；
Apollo server . applymiddleware({
app，
path:graphql path
})；

**(参见全文件**[**【GitHub】**](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/master/app.js)**)。)**

在这段代码中，我们告诉 Apollo 使用哪个模式。定义在`typeDefs`数组中提供，对应于之前创建的查询和解析器。

`app.js`中的其余代码(此处省略，不过你可以看看 GitHub 上的[整个文件)都与 Pug 模板和路由引擎有关。Pug 支持在 MVC 结构中构建页面和路径，因此简单明了。看一下`routes/index.js`文件(GitHub](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/master/app.js) 上的[文件),它包含样板文件项目中唯一的路由的定义:](https://github.com/Kentico/cloud-boilerplate-express-apollo/blob/routes/index.js)

...
router.get('/')，async function (_req，res，_ next){
const result = await Apollo client . query({
query:gql `
{
items bytype(type:" article "，limit: 3，depth: 0，order:" elements . post _ date "){
...on article content type {
title {
value
}
summary {
value
}
teaser _ image {
assets {
name
}
}
}
}
} `
})；
res.render('index '，{
articles:result . data . items bytype，
...
})；
})；module.exports =路由器；

是啊！最后，一个 GraphQL 查询。您会看到它请求所有按`post_date`排序的文章，并指定哪些数据字段应该在响应中提供(`title`、`summary`、`teaser_image`)。

注意，在查询中，我们需要指定我们期望的数据模型，因为不是所有的`ContentItem`的子节点都必须包含请求的字段(例如`summary`或`teaser_image`)。到了`… on ArticleContentType`，我们基本上创建了一个`switch`案例，如果返回的内容项是类型`ArticleContentType`的，它将返回定义的字段(`title`、`summary`和`teaser_image`)。

Apollo 客户机将这个请求发送给 Apollo 服务器，服务器将它转发给 Kentico Cloud resolver。解析器将 GraphQL 查询翻译成 REST API。内容以同样的方式返回到 Pug，它根据`views/index.pug`中的模板呈现页面。

它们是如何协同工作的？看一下[现场演示](http://bit.ly/2GGHIB5)。

# 抽点时间喝杯啤酒

我使用过并展示给你的所有工具都很容易组装起来，但是为什么要重新发明轮子呢？当您想开始使用 Apollo 和 React 或任何其他 JavaScript 框架实现一个网站时，请记住这个样板文件以节省您的时间和精力。如果您发现缺少什么或希望改进它，请随时[提出问题](http://bit.ly/2ByzwiJ)或[将它直接](http://bit.ly/2TGTmPW)添加到代码库。

你有使用 Apollo 和 GraphQL 分离问题的经验吗？你会推荐给别人吗？请在评论中告诉我。