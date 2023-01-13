# 详细教程:如何配合 React 和 Redux 使用 Shopify 的 Storefront API

> 原文：<https://www.freecodecamp.org/news/a-detailed-tutorial-how-to-use-shopifys-storefront-api-with-react-and-redux-37f95cbab7f/>

克里斯·弗雷温

# 详细教程:如何配合 React 和 Redux 使用 Shopify 的 Storefront API

#### 全民电子商务！(…就是网站？)

*作者[克里斯](https://medium.com/@frewin.christopher)2018 年 8 月，更新 2018 年 11 月*

![QjOaB1iMaaTA4wYVDOKhmtNyjYnzcm-DDxjd](img/71f0822f0ede7109d18cd47264675736.png)

Courtesy of Negative Space on [pexels.com](https://www.pexels.com/photo/grayscale-photo-of-computer-laptop-near-white-notebook-and-ceramic-mug-on-table-169573/)

#### 背景和动机

所以这里的动机很简单。我希望我的网站访问者能够直接在我的自定义域上浏览、搜索和选择产品，而不必访问我们的 Shopify 网站。

第二个动机是，我更愿意拥有自己的网站代码库，而不是使用 Shopify 的工厂模板。无意冒犯 Shopify 团队！模板是现代和干净的，但它们相当基础。我确信这些模板是高度可定制的，但目前我不知道这是一个堆栈。

所以这是两全其美——我的定制 React 网站(已经建成并上线？)，配合 Shopify 增加的 API 和结账流程！

本教程结束时，您将能够在网站的任何页面上添加您的 Shopify 产品。购物过程中唯一会发生在 Shopify 上的部分是当用户点击“结帐”时。

我也为本教程创建了一个空的样板库。

在 Medium 上写这篇文章的动机很简单，我自己找不到关于这个过程的教程，所以我决定做一个！

我已经做了 4 年的专业开发人员，7 年的编程经验。我在老派的 Fortran 和 Perl、React、Javascript、Python 和 Node 的技术栈中工作过。

Siren Apparel 是我的副业/创业/制造商公司之一，我已经经营了 5 年，到目前为止，我们已经向 5 个不同的警察和消防部门捐赠了资金！

让我们最后开始这个教程。

#### Shopify 的店面 API

Shopify 的优秀员工已经整合了[店面 API](https://help.shopify.com/en/api/custom-storefronts/storefront-api) 。使用 Storefront API，您可以创建 React 组件，将产品图片、产品变化、产品尺寸、购物车以及“添加到购物车”和“结账”按钮添加到您自己的非 Shopify 站点中。

*注意，本教程不是关于 [Shopify Polaris](https://github.com/Shopify/polaris) ，它用于在 React 中为 Shopify 商店管理本身创建组件。

#### 入门:`react-js-buy`仓库

看看 Shopify 团队构建的这个 React 示例。本教程中的大部分代码都来自这个资源库。

…你看了吗？很好！？

现在我们要直接进入代码了！前往 React 站点的根文件夹，通过终端安装`shopify-buy`模块:

```
cd my-awesome-react-project/npm install --save shopify-buy
```

(如果你喜欢`yarn`，也可以叫`yarn add shopify-buy`)

然后，在你的前端`index.js`，(不是`App.js`！)您将需要从 JS Buy SDK 中导入`Client`:

```
import Client from 'shopify-buy';
```

然后在`ReactDOM.render()`调用上方添加以下配置对象:

```
const client = Client.buildClient({    storefrontAccessToken: 'your-access-token',    domain: 'your-shopify-url.myshopify.com'});
```

这就是目前的情况——我们很快就会回来。

现在，我们将添加流畅购物和结账体验所需的所有组件。从`react-js-buy`库中复制所有组件:

`Cart.js`

`LineItem.js`

`Product.js`

`Products.js`

`VariantSelector.js`

我们会将这些组件粘贴到您的`src/`文件夹中的`components/shopify/`文件夹中。如果你愿意，你可以把这些组件文件放在`src/`文件夹的任何地方。教程的其余部分假设您已经将它们放在了`components/shopify/`中。

#### 修改 App.js

将需要大规模的变革。首先，将刚刚复制购物车组件导入到您自己的项目中:

```
import Cart from './components/shopify/Cart';
```

如果您的`App.js`组件像我一样是无状态的，那么您应该可以安全地复制整个`constructor()`函数:

```
constructor() {    super();    this.updateQuantityInCart = this.updateQuantityInCart.bind(this);    this.removeLineItemInCart = this.removeLineItemInCart.bind(this);    this.handleCartClose = this.handleCartClose.bind(this);}
```

如果已经有了状态，只复制那些`bind`行。这三行是 Shopify 购物车正常运行所需的事件处理函数。

> “可是国家对马车又怎么样呢！?"

你可能会问；或者:

> “为购物车定义那些事件处理程序怎么样！?"

的确，那是要来的，但还不是时候！？

然后，您可以将`<Car` t/ >组件附加到 `your re` nder()函数的底部，在结束 div 之前。

在我看来，购物车应该在你的应用程序的任何地方都可以访问。我认为将`<Car` t/ >组件放在你的应用程序的根组件中是有意义的——在其他 w `ords,` App.js 中:

```
return (<div>...<Cart    checkout={this.state.checkout}    isCartOpen={this.state.isCartOpen}    handleCartClose={this.handleCartClose}    updateQuantityInCart={this.updateQuantityInCart}    removeLineItemInCart={this.removeLineItemInCart} /></div>);
```

同样，我还没有在购物车的事件处理程序中包含任何代码。此外，我没有解决 App.js 中购物车缺少状态组件的问题。

这是有充分理由的。

这个项目进行到一半时，我意识到我的产品组件当然不在我的`App.js`文件中。

相反，它被埋了大约三个子组件。

因此，与其将产品向下传递三层给子产品，然后再向上传递函数处理程序…

我决定用…

**？Redux！！！？**

唉！我知道，我知道，Redux 虽然不是很难，但却是%*$用所有需要的样板文件进行初步连接。但是，如果你是一个在电子商务商店工作的开发人员或电子商务商店的所有者，请这样想:Redux 将使你能够从我们网站或 webapp 中的任何组件或页面访问购物车的状态。

随着 Siren Apparel 的扩张和我们开发更多产品，这种能力将是必不可少的。随着我们创建更多的产品，我将为所有产品创建一个单独的专用商店页面，同时在主页上只保留少数特色产品。

如果用户逛了逛商店，读了一些关于塞壬服装的故事或信息，*然后*决定结账，那么访问购物车的能力是必不可少的。不管他们走了多远，他们的购物车都不会丢失任何东西！

**所以，简而言之，我决定趁[我们的网站](https://sirenapparel.us)的代码库还不太大的时候，最好现在就实现 Redux。**

#### **用最少的样板文件为 Shopify Buy SDK 实现 Redux】**

**安装 NPM 软件包`redux`和`react-redux` :**

**T2`npm install --save redux react-redux`**

**在`index.js`中，从`react-redux`导入`Provider`，从`./store`导入您的`store`:**

【T2`import { Provider } from 'react-redux';`
**`import store from './store';`**

**将`<Provid` er >组件用 p `assed`储存在`d you` r & l `t;App>`周围；在 index.jsto 中将您的应用程序连接到您的 Redux 商店:**

**`ReactDOM.render(`**
**`<Provider store={store}>`**
**`   <IntlProvider locale={locale} messages={flattenMessages(messages[locale.substring(0, 2)])}>`**
**`     <App locale={locale}/>`**
**`   </IntlProvider>`**
**`</Provider>,`**
**`document.getElementById('root')`**
**`);` `<App locale={locale}/>`**

**(注意，我也有一个`<IntlProvid` er >，但是[在另一篇文章中讲述了我如何应用国际化和本地化来动态呈现塞壬服饰](https://medium.com/@sirenapparel/internationalization-and-localization-of-sirenapparel-eu-sirenapparel-us-and-sirenapparel-asia-ddee266066a2)网站上的内容。不同的日子，不同的故事。)**

**当然，现在我们还没有制作`./store.js`文件。在`src/`根目录下的`store.js`创建你的商店，并把它放在:**

【T2`import {createStore} from 'redux';`
**`import reducer from './reducers/cart';export default createStore(reducer);`**

**在`src/reducers/cart.js`中创建你的 reducers 文件并粘贴代码:**

 ********`   case PRODUCTS_FOUND:`
`const ADD_VARIANT_TO_CART = 'ADD_VARIANT_TO_CART'``const SHOP_FOUND = 'SHOP_FOUND'`
`export default (state = initState, action) => {``const UPDATE_QUANTITY_IN_CART = 'UPDATE_QUANTITY_IN_CART'`
`     return {...state, client: action.payload}` `return {...state, checkout: action.payload}`
【********

********别担心，我不会只贴这个大减速器而不讨论是怎么回事；我们将了解每项活动！这里有几点需要注意。********

******我们从 Shopify GitHub 示例中的状态中提取初始状态，并将其放入我们的`initState`，即状态的以下四个部分:******

******`isCartOpen: false,`**
**`checkout: { lineItems: [] },`**
**`products: [],`**
**`shop: {}`******

******然而，在我的实现中，我还创建了一个`client`状态的一部分。我调用了一次`createClient()`函数，然后立即在`index.js`中将它设置为 Redux 状态。所以让我们进入`index.js` :******

#### ******返回 index.js******

******`const client = Client.buildClient({`**
**` storefrontAccessToken: 'your-shopify-token',`**
**` domain: 'your-shopify-url.myshopify.com'`**
**`});`**
**`store.dispatch({type: 'CLIENT_CREATED', payload: client});`******

******在 Shopify buy SDK 示例中，有几个异步调用来获取关于产品的信息，并将信息存储在 React 的`componentWillMount()`函数中。示例代码如下所示:******

******`componentWillMount() {`**
**`   this.props.client.checkout.create().then((res) => {`**
 ****`       checkout: res,`**
**`     });`**
**`   });this.props.client.product.fetchAll().then((res) => {`**
**`     this.setState({`**
**`       products: res,`
**`     });`**
**`   });this.props.client.shop.fetchInfo().then((res) => {`**
**`     this.setState({`********** 

****我选择直接在`index.js`中尽可能远离现场负载的上游进行。然后，当收到每个部分的响应时，我发出一个相应的事件:****

****`// buildClient() is synchronous, so we can call all these after!`**
**`client.product.fetchAll().then((res) => {`**
**` store.dispatch({type: 'PRODUCTS_FOUND', payload: res});`**
**`});`**
**`client.checkout.create().then((res) => {`**
**` store.dispatch({type: 'CHECKOUT_FOUND', payload: res});`**
**`});`**
**`client.shop.fetchInfo().then((res) => {`** **`store.dispatch({type: 'SHOP_FOUND', payload: res});`**
**`});`****

****至此，缩减器已经创建好了，Shopify API `client`的初始化也为`index.js`完成了。****

#### ****回`App.js`****

****现在在`App.js`中，将 Redux 的商店连接到应用状态:****

****T2`import { connect } from 'react-redux';`****

****别忘了导入商店:****

****T2`import store from './store';`****

****在底部应该是`export default App`的地方，修改成这样:****

****T2`export default connect((state) => state)(App);`****

****这将 Redux 状态连接到`App`组件。****

****现在在`render()`函数中，我们可以用 Redux 的`getState()`访问 Redux 的状态(与使用 vanilla react 的`this.state`相反):****

****`render() {`**
**`   ...    `**
**`   const state = store.getState();`**
**`}`****

#### ****最后:事件处理程序(我们仍然在 App.js 中)****

****从上面，您知道在`App.js`中我们只需要三个事件处理程序，因为购物车只使用了三个:`updateQuantityInCart`、`removeLineItemInCart`和`handleCartClose`。示例 GitHub 存储库中最初的 cart 事件处理程序使用了本地组件状态，如下所示:****

****`updateQuantityInCart(lineItemId, quantity) {`**
**` const checkoutId = this.state.checkout.id`**
**` const lineItemsToUpdate = [{id: lineItemId, quantity: parseInt(quantity, 10)}]return this.props.client.checkout.updateLineItems(checkoutId, lineItemsToUpdate).then(res => {`**
**`   this.setState({`**
**`     checkout: res,`**
 ********`     checkout: res,`********** 

********我们可以重构它们，将事件分派到 Redux 存储，如下所示:********

********如果你一直跟着我，我已经提到过我添加了自己的`handleCartOpen`函数，因为我将该函数作为道具传递给我的`<Na` v/ >组件，所以用户能够通过导航中的链接打开和关闭购物车。在未来的某个时间，我可以将该功能移动到导航本身，而不是将其作为一个道具传递，因为当然 Redux 商店也将在那里可用！********

#### ******最后加上那个<产品/ >组件！******

******所以，你有一个基本的商店，可能有一些简单的`href`链接到你的 Shopify 商店的相应产品？哈！扔掉它们，换上你全新的`<Product` s/ >组件！******

******首先，将组件导入到商店标记所在的位置(记住，在我的代码库中，我已经将 shopify 示例组件放在了一个名为`shopify/`的文件夹中)******

******这将是您的产品目前所处的位置。(在我制作的[样板文件库](https://github.com/frewinchristopher/react-redux-shopify-storefront-api-example)中，我将它放在了`GenericProductsPage`组件中，以表示该代码可以应用于任何包含产品部分的页面):******

******T2`import Products from './shopify/Products';`******

******现在，过去 15-20 分钟的 redux 样板代码编辑终于有了回报:我们可以获取状态的`products`部分——不是通过道具一遍又一遍传递的普通反应状态——而是通过 Redux 状态，在一个简洁的命令行中`const state = store.getState();` :******

******`render () {`**
**`   const state = store.getState(); // state from redux store`**
**`   let oProducts = <Products`**
**`     products={state.products}`**
**`     client={state.client}`**
**`     addVariantToCart={this.addVariantToCart}`**
********

******不要忘记将组件本身放到你的`render()`函数中它应该在的地方。对我来说，这个位置隐藏在引导风格的类和 HTML:******

******`...`**
**`<div className="service-content-one">`**
**`   <div className="row">`**
**`       <Products/>`**
**`   </div>{/*/.row*/}`**
**`</div>{/*/.service-content-one*/}`**
********

******最后，我们需要一个单一的事件函数`addVariantToCart`,购物车才能使用这个产品组件。同样，作为参考，这里是`addVariantToCart`(同样，来自 shopify 示例存储库):**的原始、香草反应本地`state`版本****

******`addVariantToCart(variantId, quantity){`**
**` this.setState({`**
**`   isCartOpen: true,`**
**` });const lineItemsToAdd = [{variantId, quantity: parseInt(quantity, 10)}]`**
**` const checkoutId = this.state.checkout.idreturn this.props.client.checkout.addLineItems(checkoutId, lineItemsToAdd).then(res => {`**
**`   this.setState({`**
**`     checkout: res,`**
**`   });`** **`});`**
**`}`******

******和新的，Redux 友好的`store.dispatch()`版本:******

******`addVariantToCart(variantId, quantity) {`**
**`   const state = store.getState(); // state from redux store`**
**`   const lineItemsToAdd = [{variantId, quantity: parseInt(quantity, 10)}]`**
**`   const checkoutId = state.checkout.id`**
**`   state.client.checkout.addLineItems(checkoutId, lineItemsToAdd).then(res => {`**
**`     store.dispatch({type: 'ADD_VARIANT_TO_CART', payload: {isCartOpen: true, checkout: res}});`**
**`   });`**
**`}` `const checkoutId = state.checkout.id`******

****这当然是我们将要使用的一个。？****

******别忘了在构造函数中绑定:******

******T2`this.addVariantToCart = this.addVariantToCart.bind(this);`******

******此外，您需要像`App.js`一样将这个组件连接到商店，并导入商店:******

****【T2`import { connect } from 'react-redux'`
**`import store from '../store';`******

******在最上面，并且(假设你放 Shopify `Product`的组件名是`GenericProductPage` :******

******T2`export default connect((state) => state)(GenericProductsPage);`******

******在底部。******

******太好了！现在，无论在组件中隐藏得多深，或者在声明产品组件的任何地方，它都可以与购物车的状态进行通信！******

#### ******最终奖励示例:标题中的购物车或导航******

******如果你想在你的标题/导航中有一个“购物车”按钮，在你的导航组件的渲染功能中添加这个按钮(同样，我当前网站的一个例子，有引导样式——一个非常简单的版本在[样板示例](https://github.com/frewinchristopher/react-redux-shopify-storefront-api-example) :******

******`<div className="App__view-cart-wrapper">`**
**`<button className="App__view-cart" onClick={this.props.handleCartOpen}>`**
**`   Cart`**
**`   </button>`**
**`</div>`******

******其中`handleCartOpen`是一个新的处理程序方法，你必须添加到`App.js` :******

******`constructor() {`**
**` super();`**
**` ...`**
**` this.handleCartOpen = this.handleCartOpen.bind(this);`**
**` ...`**
********

******在构造函数中。然后当你在 App.js 中引用你的 Nav 组件时(或者你放置 Nav 的任何地方)，你传递函数处理程序:******

******T2`<Nav handleCartOpen={this.handleCartOpen}/>`******

****在 Redux 中，这也可以被重构为一个事件，但是因为它只有一个子节点，所以我用了普通的 React 方式。****

#### ******造型组件******

******我依赖的是 Shopify 的 CSS 文件`app.css`，位于`storefront-api-example`资源库的`shared/`文件夹中(你不会错过的，这是`shared/`中唯一的文件)！******

******确保将其复制到您的`styles/`文件夹或任何需要的地方，并包含在您的`index.js`文件中。在我的`index.js`里是这样的:******

******T2`import './styles/shopify.css';`******

******因为我将 Shopify 范例库中的`app.css`重命名为`shopify.css`，并将它放入文件夹`styles`。这个约定也用在样板存储库代码中。******

******从这里很容易识别出`shopify.css`中定义按钮默认亮蓝色的确切位置，等等。我将保存详细的 CSS 定制供您处理。？******

****但是谁知道呢，也许我最终会把它贴上去——但是我发现 Shopify 的风格非常好，而且很容易修改。****

#### ******外卖******

******在我看来，这是一个完美的(非待办事项？)Redux 的使用。Redux 清晰地组织了 Shopify 购物车的事件功能和状态，并使从任何其他组件访问购物车的状态变得容易。这比在整个 React 应用程序中将状态片段传递给子节点并使用多个事件处理程序将事件传递回父函数要容易维护得多。******

****如教程中的示例所示，购物车的状态可以在导航组件和首页的商店部分轻松访问。我也可以很容易地将它添加到一个“特色”产品部分，一旦塞壬服装准备好了。****

#### ******找到代码******

****这个实现的样板库[可以在这里找到](https://github.com/frewinchristopher/react-redux-shopify-storefront-api-example)。它是一个近乎空白的`create-react-app`应用，但在`index.js`和`App.js`中实现了本教程的所有更改，以及一个超级基本的`GenericStorePage`和`Nav`组件。****

****我在重新阅读和更新我自己的教程时，在回购上构建了代码，以确保本教程有意义！****

******因为我疯了？，海妖服饰的网站都是开源的。因此，如果你想摆弄我的实现，c [检查仓库！](https://github.com/frewinchristopher/sirenapparel.us)******

****我希望你喜欢这个教程！如果有任何不清楚的地方，或者只是不起作用，请告诉我！我会尽力帮助你的！****

******感谢[丽莎·卡塔拉诺](http://css-snippets.com/author/lisa/)为[的简单导航示例](http://css-snippets.com/simple-horizontal-navigation/#code)提供 CSS-Snippets，我在样板文件库中使用了它！******

******干杯！？******

****克里斯****