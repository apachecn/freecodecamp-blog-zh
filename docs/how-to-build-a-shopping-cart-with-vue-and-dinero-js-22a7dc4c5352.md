# 如何用 Vue 和 Dinero.js 搭建购物车

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-shopping-cart-with-vue-and-dinero-js-22a7dc4c5352/>

莎拉·达扬

# 如何用 Vue 和 Dinero.js 搭建购物车

![aZOvofVNEwXYaZxNh9vZn0LwlbpyJGYz-MHT](img/34165b59e5a2abf4f27cb92d8309a981.png)

Photo by [Allef Vinicius](https://unsplash.com/photos/fJTqyZMOh18?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/checkout-money?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我的朋友科里和我几乎每天都聊天，所以你可以打赌他知道我生活中发生的一切。但是前几天我们聊天时，我意识到**他根本不知道我的最新项目实际上是如何运作的**。比如，你能用它做什么。

我停顿了一下，意识到这可能并不那么明显。无论你的技能水平如何，理解一个平滑滚动插件比理解一个钱库能提供什么要容易得多。

> “你在 JavaScript 中看到了如何使用日期构造函数来存储日期并在以后格式化它吗？或者使用 Moment.js 创建 Moment 对象，这比将日期存储为字符串或任何其他类型要好吗？

> 嗯， **Dinero.js 就像 Moment，但是为了钱**。没有处理金钱的固有方式，如果你试图用`Number`类型来处理，你将会遇到问题。这就是 Dinero.js 帮你避免的。它保护你在物品中的货币价值，并允许你做任何你需要的事情。"

我对自己的解释很满意，因为科里开始“啊哈”了。但我意识到从一开始就缺少了一样东西。一些有说服力的东西，可以帮助任何人理解 Dinero.js 的好处:一个**真实世界的例子**。

在本教程中，我们将构建一个购物车。我们将使用 Vue.js 来构建组件，然后集成 Dinero.js 来处理所有的钱的事情。

***TL；博士:*** *这篇文章深入探讨了如何和为什么。它旨在帮助你掌握 Dinero.js 的核心概念。如果你想了解整个思维过程，请继续阅读。否则可以在 [CodeSandbox](https://codesandbox.io/s/ojvmp7ryk5) 上看最终代码。*

这篇文章假设你对 Vue.js 有基本的了解。如果没有，先看看我的教程[“构建你的第一个 Vue.js 组件](https://frontstuff.io/build-your-first-vue-js-component)”它将为你提供进一步发展所需的一切。

### 入门指南

对于这个项目，我们将使用 [vue-cli](https://github.com/vuejs/vue-cli) 和 [webpack-simple](https://github.com/vuejs-templates/webpack-simple) Vue.js 模板。如果您的计算机上没有全局安装 vue-cli，请启动您的终端并键入以下内容:

```
npm install -g vue-cli
```

然后:

```
vue init webpack-simple path/to/my-project
```

您可以保留所有问题的默认选项。完成后，导航到新目录，安装依赖项，并运行项目:

```
cd path/to/my-project npm install npm run dev
```

Webpack 将开始在端口`8080`(如果可用)上为您的项目提供服务，并在您的浏览器中打开它。

### 设置 HTML/CSS

**我不会在本教程**中涉及页面结构和样式，所以我邀请你复制/粘贴代码。打开`App.vue`文件，粘贴以下代码片段。

这位于`<templa` te >标签之间:

```
<div id="app">  <div class="cart">    <h1 class="title">Order</h1>    <ul class="items">      <li class="item">        <div class="item-preview">          <img src="" alt="" class="item-thumbnail">          <div>            <h2 class="item-title"></h2>            <p class="item-description"></p>          </div>        </div>        <div>          <input type="text" class="item-quantity">          <span class="item-price"></span>        </div>      </li>    </ul>    <h3 class="cart-line">      Subtotal <span class="cart-price"></span>    </h3>    <h3 class="cart-line">      Shipping <span class="cart-price"></span>    </h3>    <h3 class="cart-line">      Total <span class="cart-price cart-total"></span>    </h3>  </div></div>
```

在`<sty` le >标签之间添加以下内容:

```
body {  margin: 0;  background: #fdca40;  padding: 30px;}
```

```
.title {  display: flex;  justify-content: space-between;  align-items: center;  margin: 0;  text-transform: uppercase;  font-size: 110%;  font-weight: normal;}
```

```
.items {  margin: 0;  padding: 0;  list-style: none;}
```

```
.cart {  background: #fff;  font-family: 'Helvetica Neue', Arial, sans-serif;  font-size: 16px;  color: #333a45;  border-radius: 3px;  padding: 30px;}.cart-line {  display: flex;  justify-content: space-between;  align-items: center;  margin: 20px 0 0 0;  font-size: inherit;  font-weight: normal;  color: rgba(51, 58, 69, 0.8);}.cart-price {  color: #333a45;}.cart-total {  font-size: 130%;}
```

```
.item {  display: flex;  justify-content: space-between;  align-items: center;  padding: 15px 0;  border-bottom: 2px solid rgba(51, 58, 69, 0.1);}.item-preview {  display: flex;  align-items: center;}.item-thumbnail {  margin-right: 20px;  border-radius: 3px;}.item-title {  margin: 0 0 10px 0;  font-size: inherit;}.item-description {  margin: 0;  color: rgba(51, 58, 69, 0.6);}.item-quantity {  max-width: 30px;  padding: 8px 12px;  font-size: inherit;  color: rgba(51, 58, 69, 0.8);  border: 2px solid rgba(51, 58, 69, 0.1);  border-radius: 3px;  text-align: center;}.item-price {  margin-left: 20px;}
```

### 添加数据

当您处理产品时，您通常从数据库或 API 中检索原始数据。我们可以通过在单独的 JSON 文件中表示它，然后异步导入它，就像我们在查询 API 一样。

让我们在`assets/`中创建一个`products.json`文件，并添加以下内容:

```
{  "items": [    {      "title": "Item 1",      "description": "A wonderful product",      "thumbnail": "https://fakeimg.pl/80x80",      "quantity": 1,      "price": 20    },    {      "title": "Item 2",      "description": "A wonderful product",      "thumbnail": "https://fakeimg.pl/80x80",      "quantity": 1,      "price": 15    },    {      "title": "Item 3",      "description": "A wonderful product",      "thumbnail": "https://fakeimg.pl/80x80",      "quantity": 2,      "price": 10    }  ],  "shippingPrice": 20}
```

**这与我们从真实的 API 中得到的非常相似:**数据是一个集合，标题和文本是字符串，数量和价格是数字。

我们可以回到`App.vue`，在`data`中设置空值。这将允许在获取实际数据时初始化模板。

```
data() {  return {    data: {      items: [],      shippingPrice: 0    }  }}
```

最后，我们可以通过异步请求从`products.json`获取数据，并在准备就绪时更新`data`属性:

```
export default {  ...  created() {    fetch('./src/assets/products.json')      .then(response => response.json())      .then(json => (this.data = json))  }}
```

现在让我们用这些数据填充模板:

```
<ul class="items">  <li :key="item.id" v-for="item in data.items" class="item">    <div class="item-preview">      <img :src="item.thumbnail" :alt="item.title" class="item-thumbnail">      <div>        <h2 class="item-title">{{ item.title }}</h2>        <p class="item-description">{{ item.description }}</p>      </div>    </div>    <div>      <input type="text" class="item-quantity" v-model="item.quantity">      <span class="item-price">{{ item.price }}</span>    </div>  </li></ul>...<h3 class="cart-line">  Shipping  <span class="cart-price">{{ data.shippingPrice }}</span></h3>...
```

您应该会看到购物车中的所有商品。现在让我们添加一些计算属性来计算小计和总计:

```
export default {  ...  computed: {    getSubtotal() {      return this.data.items.reduce(        (a, b) => a + b.price * b.quantity,        0      )    },    getTotal() {      return (        this.getSubtotal + this.data.shippingPrice      )    }  }}
```

并将它们添加到我们的模板中:

```
<h3 class="cart-line">  Subtotal  <span class="cart-price">{{ getSubtotal }}</span></h3>...<h3 class="cart-line">  Total  <span class="cart-price cart-total">{{ getTotal }}</span></h3>
```

我们走吧！尝试改变数量—您应该会看到小计和总额相应地改变。

现在我们有几个问题。首先，我们只显示金额，不显示货币。当然，我们可以将它们硬编码在模板中，紧挨着反应量。但是如果我们想做一个多语言的网站呢？并非所有语言都以相同的方式格式化货币。

为了更好地对齐，如果我们想用两位小数显示所有金额，该怎么办？您可以通过使用`toFixed`方法尝试将所有的初始金额保持为浮点数，但是这样一来，您将使用`String`类型，这种类型在进行数学运算时要困难得多，性能也差得多。此外，这将意味着纯粹为了表示的目的而改变数据，这从来都不是一个好主意。如果您出于其他目的需要相同的数据，而它需要不同的格式，该怎么办？

最后，目前的解决方案是依靠浮点数学，**这是一个[坏主意，当谈到处理金钱](https://frontstuff.io/how-to-handle-monetary-values-in-javascript)** 。试着改变一些数量:

```
{  "items": [    {      ...      "price": 20.01    },    {      ...      "price": 15.03    },    ...  ]}
```

现在，看看你的购物车有多破？这不是一些有错误的 JavaScript 行为，而是我们如何用二进制机器表示十进制计数系统的一个**限制。如果你用浮点数做数学，你迟早会遇到那些不准确的地方。**

好消息是，**我们不需要使用 floats 来存储钱**。这正是 Dinero.js 发挥作用的地方。

### 钱的包装

金钱和金钱的关系就像约会和时间的关系一样。这是一个库，让你创建货币[值对象](https://en.wikipedia.org/wiki/Value_object)，操纵它们，问它们问题，并格式化它们。它依赖于马丁·福勒的[货币模式](https://martinfowler.com/eaaCatalog/money.html)，并帮助你解决所有由浮动引起的常见问题，主要是通过以整数形式存储次要货币单位的金额。

打开您的终端并安装 Dinero.js:

```
npm install dinero.js --save
```

然后导入到`App.vue`:

```
import Dinero from 'dinero.js'
```

```
export default {  ...}
```

你现在可以创建 Dinero 对象了？

```
// returns a Dinero object with an amount of $50Dinero({ amount: 500, currency: 'USD' })
```

```
// returns $4,000.00Dinero({ amount: 500 })  .add(Dinero({ amount: 500 }))  .multiply(4)  .toFormat()
```

让我们创建一个工厂方法，根据需要将我们的`price`属性转换成 Dinero 对象。我们有最多两位小数的浮点数。这意味着，如果我们想把它们转换成较小货币单位的等价物(在我们的例子中，是美元)，**我们需要把它们乘以 10 的 2 次方**。

我们将`factor`作为一个带有默认值的参数传递，因此我们可以对具有不同[指数](https://en.wikipedia.org/wiki/ISO_4217#Treatment_of_minor_currency_units_.28the_.22exponent.22.29)的货币使用该方法。

```
export default {  ...  methods: {    toPrice(amount, factor = Math.pow(10, 2)) {      return Dinero({ amount: amount * factor })    }  }}
```

美元是默认货币，所以我们不需要指定它。

因为我们在转换过程中进行浮点运算，一些计算可能会以稍微不准确的浮点结束。这很容易通过将结果四舍五入到最接近的整数来解决。

```
toPrice(amount, factor = Math.pow(10, 2)) {  return Dinero({ amount: Math.round(amount * factor) })}
```

现在我们可以在计算的属性中使用`toPrice`:

```
export default {  ...  computed: {    getShippingPrice() {      return this.toPrice(this.data.shippingPrice)    },    getSubtotal() {      return this.data.items.reduce(        (a, b) =>          a.add(            this.toPrice(b.price).multiply(b.quantity)          ),        Dinero()      )    },    getTotal() {      return this.getSubtotal.add(this.getShippingPrice)    }  }}
```

在我们的模板中:

```
<ul class="items">  <li :key="item.id" v-for="item in data.items" class="item">    <div class="item-preview">      <img :src="item.thumbnail" :alt="item.title" class="item-thumbnail">      <div>        <h2 class="item-title">{{ item.title }}</h2>        <p class="item-description">{{ item.description }}</p>      </div>    </div>    <div>      <input type="text" class="item-quantity" v-model="item.quantity">      <span class="item-price">{{ toPrice(item.price) }}</span>    </div>  </li></ul><h3 class="cart-line">  Subtotal  <span class="cart-price">{{ getSubtotal }}</span></h3><h3 class="cart-line">  Shipping  <span class="cart-price">{{ getShippingPrice }}</span></h3><h3 class="cart-line">  Total  <span class="cart-price cart-total">{{ getTotal }}</span></h3>
```

如果你看看你的购物车，你会看到`{}`代替了价格。那是因为我们试图展示一个物体。相反，**我们需要对它们进行格式化，这样它们就可以用正确的语法**显示价格，同时显示货币符号。

我们可以用迪内罗的`[toFormat](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~toFormat)` [方法](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~toFormat)来实现。

```
<ul class="items">  <li :key="item.id" v-for="item in data.items" class="item">    ...    <div>      ...      <span class="item-price">        {{ toPrice(item.price).toFormat() }}      </span>    </div>  </li></ul><h3 class="cart-line">  Subtotal  <span class="cart-price">    {{ getSubtotal.toFormat() }}  </span></h3><h3 class="cart-line">  Shipping  <span class="cart-price">    {{ getShippingPrice.toFormat() }}  </span></h3><h3 class="cart-line">  Total  <span class="cart-price cart-total">    {{ getTotal.toFormat() }}  </span></h3>
```

查看您的浏览器:**您现在有了一个格式良好、功能齐全的购物车**？

### 更进一步

现在你已经很好地掌握了 Dinero.js 的基础知识，是时候把标准提高一点了。

#### 介绍会；展示会

让我们把 JSON 文件中的`shippingPrice`改为`0`。您的购物车现在应该显示*“运费:$ 0.00”*，这是准确的，但不是用户友好的。上面写着*【免费】*不是更好吗？

幸运的是，Dinero.js 有很多方便的方法向您的实例提问。在我们的例子中，`[isZero](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~isZero)` [方法](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~isZero)正是我们所需要的。

在模板中，只要文本表示零，就可以显示文本而不是格式化的 Dinero 对象:

```
<h3 class="cart-line">  Shipping  <span class="cart-price">    {{      getShippingPrice.isZero() ?      'Free' :      getShippingPrice.setLocale(getLocale).toFormat()    }}  </span></h3>
```

当然，您可以通过将它包装在一个方法中来概括这种行为。它将接受一个 Dinero 对象作为参数，并返回一个`String`。这样，无论何时你试图显示零金额，你都可以显示*“免费”*。

#### 区域切换

想象你在做一个电子商务网站。您想要适应您的国际观众，所以您翻译内容并添加语言切换器。然而，有一个细节可能会让你忽略:**货币格式也会随着语言的不同而变化**。例如，美式英语中的 10.00 欧元翻译成法语中的 10.00 欧元。

Dinero.js 通过 [I18n API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl) 支持国际格式。这使您可以使用本地化格式显示金额。

Dinero.js 是不可变的，所以我们不能依靠改变`[Dinero.globalLocale](https://sarahdayan.github.io/dinero.js/global.html#Globals)`来重新格式化所有现有的实例。相反，我们需要使用`[setLocale](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~setLocale)` [方法](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~setLocale)。

首先，我们在`data`中添加一个新属性`language`，并将其设置为默认值。对于地区，你需要使用一个 [BCP 47 语言标签](http://tools.ietf.org/html/rfc5646)，比如`en-US`。

```
data() {  return {    data: {      ...    },    language: 'en-US'  }}
```

现在我们可以直接在 Dinero 对象上使用`setLocale`。当`language`改变时，格式也会改变。

```
export default {  ...  methods: {    toPrice(amount, factor = Math.pow(10, 2)) {      return Dinero({ amount: Math.round(amount * factor) })        .setLocale(this.language)    }  },  computed: {    ...    getSubtotal() {      return this.data.items.reduce(        (a, b) =>          a.add(            this.toPrice(b.price).multiply(b.quantity)          ),        Dinero().setLocale(this.language)      )    },    ...  }}
```

我们只需要在`toPrice`和`getSubtotal`中添加`setLocale`，这是我们创建 Dinero 对象的唯一地方。

现在我们可以添加我们的语言切换器:

```
// HTML<h1 class="title">  Order  <span>    <span class="language" @click="language = 'en-US'">English</span>    <span class="language" @click="language = 'fr-FR'">French</span>  </span></h1>
```

```
// CSS.language {  margin: 0 2px;  font-size: 60%;  color: rgba(#333a45, 0.6);  text-decoration: underline;  cursor: pointer;}
```

当你点击切换器时，它将重新分配`language`，这将改变对象的格式。因为库是不可变的，这将返回新的对象，而不是改变现有的对象。这意味着如果你创建了一个 Dinero 对象并决定在某个地方显示它，然后在其他地方引用它并对它应用一个`setLocale`，**你的初始实例不会受到影响**。没有讨厌的副作用！

#### 含税

在购物车上看到税线是很常见的。你可以用 Dinero.js 添加一个，使用`[percentage](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~percentage)` [方法](https://sarahdayan.github.io/dinero.js/module-Dinero.html#~percentage)。

首先，让我们在 JSON 文件中添加一个`vatRate`属性:

```
{  ...  "vatRate": 20}
```

以及`data`中的初始值:

```
data() {  return {    data: {      ...      vatRate: 0    }  }}
```

现在，我们可以使用这个值来计算购物车的含税总额。首先，我们需要创建一个`getTaxAmount`计算属性。然后我们也可以将它添加到`getTotal`中。

```
export default {  ...  computed: {    getTaxAmount() {      return this.getSubtotal.percentage(this.data.vatRate)    },    getTotal() {      return this.getSubtotal        .add(this.getTaxAmount)        .add(this.getShippingPrice)    }  }}
```

购物车现在显示含税总额。我们还可以添加一行来显示税额:

```
<h3 class="cart-line">  VAT ({{ data.vatRate }}%)  <span class="cart-price">{{ getTaxAmount.toFormat() }}</span></h3>
```

我们完事了。我们已经探索了 Dinero.js 的几个概念，但这只是它所提供的一些皮毛。你可以[通读文档](https://sarahdayan.github.io/dinero.js)并在 [GitHub](https://github.com/sarahdayan/dinero.js) 上查看项目。启动它，分叉它，给我发送反馈，甚至打开一个拉动请求！我有一个不错的小[投稿指南](https://github.com/sarahdayan/dinero.js/blob/master/CONTRIBUTING.md)来帮助你开始。

也可以在 [CodeSandbox](https://codesandbox.io/s/ojvmp7ryk5) 上看最终代码。

我目前正致力于为 Dinero.js 引入一个`convert`方法，以及更好地支持所有[T2 和 ISO 4217 货币。你可以在推特](https://en.wikipedia.org/wiki/ISO_4217)上关注我。

编码快乐！？？‍?

*最初发表于 [frontstuff.io](https://frontstuff.io/build-a-shopping-cart-with-vue-and-dinerojs) 。*