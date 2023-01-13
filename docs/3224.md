# Web 开发中你应该知道的 4 种设计模式:观察者、单例、策略和装饰者

> 原文：<https://www.freecodecamp.org/news/4-design-patterns-to-use-in-web-development/>

你有没有在一个团队中需要从头开始一个项目？很多初创公司和其他小公司通常都是这样。

有这么多不同的编程语言、架构和其他关注点，很难弄清楚从哪里开始。这就是设计模式出现的原因。

设计模式就像是项目的模板。它使用某些约定，你可以从它那里期待一种特定的行为。这些模式是由许多开发人员的经验组成的，因此它们实际上就像不同的最佳实践集。

您和您的团队可以决定哪组最佳实践对您的项目最有用。基于您选择的设计模式，您将开始对代码应该做什么以及您将使用什么词汇有所期望。

编程设计模式可以跨所有编程语言使用，并且可以用于适应任何项目，因为它们只给你一个解决方案的大致轮廓。

《设计模式——可重用面向对象软件的元素》一书有 23 种官方模式，这本书被认为是面向对象理论和软件开发方面最有影响力的书籍之一。

在本文中，我将介绍其中的四种设计模式，只是为了让您对其中的一些模式以及何时使用它们有所了解。

## 单体设计模式

singleton 模式只允许一个类或对象有一个实例，它使用一个全局变量来存储这个实例。您可以使用延迟加载来确保该类只有一个实例，因为它只会在您需要时创建该类。

这可以防止多个实例同时处于活动状态，这可能会导致奇怪的错误。大多数时候，这是在构造函数中实现的。单例模式的目标通常是管理应用程序的全局状态。

你可能一直在使用的一个例子就是你的日志记录器。

如果您使用一些前端框架，如 React 或 Angular，您就会知道处理来自多个组件的日志有多复杂。这是一个很好的单例实例，因为您永远不会想要一个 logger 对象的多个实例，尤其是在您使用某种错误跟踪工具的情况下。

```
class FoodLogger {
  constructor() {
    this.foodLog = []
  }

  log(order) {
    this.foodLog.push(order.foodItem)
    // do fancy code to send this log somewhere
  }
}

// this is the singleton
class FoodLoggerSingleton {
  constructor() {
    if (!FoodLoggerSingleton.instance) {
      FoodLoggerSingleton.instance = new FoodLogger()
    }
  }

  getFoodLoggerInstance() {
    return FoodLoggerSingleton.instance
  }
}

module.exports = FoodLoggerSingleton
```

An example of the singleton class

现在您不必担心丢失来自多个实例的日志，因为您的项目中只有一个实例。因此，当您想要记录已经订购的食物时，您可以在多个文件或组件中使用同一个 *FoodLogger* 实例。

```
const FoodLogger = require('./FoodLogger')

const foodLogger = new FoodLogger().getFoodLoggerInstance()

class Customer {
  constructor(order) {
    this.price = order.price
    this.food = order.foodItem
    foodLogger.log(order)
  }

  // other cool stuff happening for the customer
}

module.exports = Customer
```

An example of a Customer class using the singleton

```
const FoodLogger = require('./FoodLogger')

const foodLogger = new FoodLogger().getFoodLoggerInstance()

class Restaurant {
  constructor(inventory) {
    this.quantity = inventory.count
    this.food = inventory.foodItem
    foodLogger.log(inventory)
  }

  // other cool stuff happening at the restaurant
}

module.exports = Restaurant
```

An example of the Restaurant class using the same singleton as the Customer class

有了这个单例模式，您就不必担心只从主应用程序文件中获取日志。你可以从你的代码库中的任何地方得到它们，它们都将进入日志记录器的同一个实例，这意味着你的日志不会因为新的实例而丢失。

## 战略设计模式

策略模式就像是 if else 语句的高级版本。基本上就是为基类中的方法创建一个接口。然后，该接口用于查找应该在派生类中使用的该方法的正确实现。在这种情况下，实现将在运行时根据客户端来决定。

这种模式在一个类有必需和可选方法的情况下非常有用。该类的一些实例不需要可选方法，这给继承解决方案带来了问题。您可以为可选方法使用接口，但是每次使用该类时都必须编写实现，因为没有默认的实现。

这就是战略模式拯救我们的地方。不是客户寻找实现，而是委托给策略接口，策略找到正确的实现。它的一个常见用途是支付处理系统。

你*可能*有一个只允许顾客用信用卡结账的购物车，但你会失去想用其他支付方式的顾客。

策略设计模式让我们将支付方法从结账流程中分离出来，这意味着我们可以添加或更新策略，而无需更改购物车或结账流程中的任何代码。

这里有一个使用付款方式实现策略模式的例子。

```
class PaymentMethodStrategy {

  const customerInfoType = {
    country: string
    emailAddress: string
    name: string
    accountNumber?: number
    address?: string
    cardNumber?: number
    city?: string
    routingNumber?: number
    state?: string
  }

  static BankAccount(customerInfo: customerInfoType) {
    const { name, accountNumber, routingNumber } = customerInfo
    // do stuff to get payment
  }

  static BitCoin(customerInfo: customerInfoType) {
    const { emailAddress, accountNumber } = customerInfo
    // do stuff to get payment
  }

  static CreditCard(customerInfo: customerInfoType) {
    const { name, cardNumber, emailAddress } = customerInfo
    // do stuff to get payment
  }

  static MailIn(customerInfo: customerInfoType) {
    const { name, address, city, state, country } = customerInfo
    // do stuff to get payment
  }

  static PayPal(customerInfo: customerInfoType) {
    const { emailAddress } = customerInfo
    // do stuff to get payment
  }
}
```

An example of the strategy pattern implementation

为了实现我们的支付方法策略，我们创建了一个包含多个静态方法的类。每个方法都采用相同的参数 *customerInfo* ，并且该参数有一个定义的类型 *customerInfoType* 。(嘿，所有打字稿开发者！？？)注意，每个方法都有自己的实现，并使用来自 *customerInfo* 的不同值。

使用策略模式，您还可以动态地更改运行时使用的策略。这意味着您将能够根据用户输入或应用程序运行的环境来更改正在使用的策略或方法实现。

您还可以在一个简单的 *config.json* 文件中设置默认实现，如下所示:

```
{
  "paymentMethod": {
    "strategy": "PayPal"
  }
}
```

config.json for setting the default implementation of paymentMethod to "PayPal" at run time

每当客户开始在你的网站上进行结账过程时，他们遇到的默认支付方式将是来自 *config.json* 的 PayPal 实现。如果客户选择不同的支付方式，这可以很容易地更新。

现在我们将为我们的结帐过程创建一个文件。

```
const PaymentMethodStrategy = require('./PaymentMethodStrategy')
const config = require('./config')

class Checkout {
  constructor(strategy='CreditCard') {
    this.strategy = PaymentMethodStrategy[strategy]
  }

  // do some fancy code here and get user input and payment method

  changeStrategy(newStrategy) {
    this.strategy = PaymentMethodStrategy[newStrategy]
  }

  const userInput = {
    name: 'Malcolm',
    cardNumber: 3910000034581941,
    emailAddress: 'mac@gmailer.com',
    country: 'US'
  }

  const selectedStrategy = 'Bitcoin'

  changeStrategy(selectedStrategy)

  postPayment(userInput) {
    this.strategy(userInput)
  }
}

module.exports = new Checkout(config.paymentMethod.strategy)
```

这个 *Checkout* 类是展示策略模式的地方。我们导入了几个文件，这样我们就有了可用的支付方式策略和来自*配置*的默认策略。

然后我们用构造函数和默认*策略*的后备值创建类，以防在*配置*中没有设置。接下来，我们将*策略*的值赋给一个本地状态变量。

我们需要在我们的 *Checkout* 类中实现的一个重要方法是改变支付策略的能力。客户可能会改变他们想要使用的支付方式，你需要能够处理这种情况。这就是*改变策略*方法的目的。

在您完成一些花哨的编码并获得客户的所有输入后，您可以根据他们的输入立即更新支付策略，它会在发送支付进行处理之前动态设置*策略*。

在某些时候，你可能需要向你的购物车添加更多的支付方式，你所要做的就是将它添加到 *PaymentMethodStrategy* 类中。在任何使用该类的地方都可以立即使用它。

当您处理具有多个实现的方法时，策略设计模式是一个强大的模式。这可能感觉像是在使用一个接口，但您不必每次在不同的类中调用该方法时都为其编写实现。它比界面给你更多的灵活性。

## 观察者设计模式

如果您曾经使用过 MVC 模式，那么您已经使用过观察者设计模式。模型部分就像一个主题，而视图部分就像这个主题的观察者。您的主题保存所有数据和数据的状态。然后你有观察器，像不同的组件，当数据被更新时，它将从主体获得数据。

观察者设计模式的目标是在主题和所有等待数据的观察者之间创建一对多的关系，以便更新数据。因此，每当对象的状态发生变化时，所有的观察者都会收到通知并立即更新。

使用这种模式的一些例子包括:发送用户通知、更新、过滤和处理订阅者。

假设您有一个单页应用程序，它有三个功能下拉列表，这些列表依赖于从更高级别的下拉列表中选择的类别。这在很多购物网站上都很常见，比如家得宝。页面上有一组过滤器，它们依赖于顶级过滤器的值。

顶级下拉菜单的代码可能类似于以下内容:

```
class CategoryDropdown {
  constructor() {
    this.categories = ['appliances', 'doors', 'tools']
    this.subscriber = []
  }

  // pretend there's some fancy code here

  subscribe(observer) {
    this.subscriber.push(observer)
  }

  onChange(selectedCategory) {
    this.subscriber.forEach(observer => observer.update(selectedCategory))
  }
}
```

The subject that updates the observers

这个 *CategoryDropdown* 文件是一个简单的类，它有一个构造函数来初始化下拉列表中可用的类别选项。这是您要处理的文件，用于从后端检索列表，或者在用户看到选项之前进行任何排序。

*subscribe* 方法是用这个类创建的每个过滤器如何接收关于观察者状态的更新。

*onChange* 方法是我们如何向所有订阅者发送通知，告知他们正在观察的观察者发生了状态变化。我们只是遍历所有的订阅者，用*选择的类别*调用他们的*更新*方法。

其他过滤器的代码可能如下所示:

```
class FilterDropdown {
  constructor(filterType) {
    this.filterType = filterType
    this.items = []
  }

  // more fancy code here; maybe make that API call to get items list based on filterType

  update(category) {
    fetch('https://example.com')
      .then(res => this.items(res))
  }
}
```

A potential observer of the subject

这个 *FilterDropdown* 文件是另一个简单的类，它代表了我们可能在页面上使用的所有潜在下拉列表。当创建这个类的新实例时，需要向它传递一个 *filterType。*这可用于进行特定的 API 调用，以获取项目列表。

*update* 方法是一个实现，一旦观察者发送了新类别，您就可以对其进行操作。

现在，我们来看看在 observer 模式中使用这些文件意味着什么:

```
const CategoryDropdown = require('./CategoryDropdown')
const FilterDropdown = require('./FilterDropdown')

const categoryDropdown = new CategoryDropdown() 

const colorsDropdown = new FilterDropdown('colors')
const priceDropdown = new FilterDropdown('price')
const brandDropdown = new FilterDropdown('brand')

categoryDropdown.subscribe(colorsDropdown)
categoryDropdown.subscribe(priceDropdown)
categoryDropdown.subscribe(brandDropdown)
```

An example of the observer pattern in action

这个文件向我们展示的是，我们有 3 个下拉列表，它们是可观察到的类别下拉列表的订户。然后我们为观察者订阅每一个下拉列表。每当观察者的类别更新时，它会将值发送给每个订阅者，订阅者会立即更新各个下拉列表。

## 装饰设计模式

使用装饰设计模式相当简单。您可以拥有一个基类，当您用该类创建一个新的对象时，该基类具有存在的方法和属性。现在假设你有一些类的实例需要来自基类的方法或属性。

您可以将这些额外的方法和属性添加到基类中，但是这可能会搞乱您的其他实例。您甚至可以创建子类来保存您需要的特定方法和属性，这些方法和属性不能放在基类中。

这两种方法都可以解决你的问题，但是它们都很笨重，效率很低。这就是装饰模式介入的地方。不要仅仅为了给一个对象实例添加一些东西而使你的代码库变得难看，你可以把那些特定的东西直接添加到实例中。

因此，如果您需要添加一个保存对象价格的新属性，您可以使用 decorator 模式将它直接添加到特定的对象实例中，而不会影响该类对象的任何其他实例。

你曾经在网上订购食物吗？那么您可能已经遇到了装饰模式。如果你得到了一个三明治，你想添加特殊的配料，该网站不会将这些配料添加到当前用户试图订购的每个三明治实例中。

下面是一个客户类的示例:

```
class Customer {
  constructor(balance=20) {
    this.balance = balance
    this.foodItems = []
  }

  buy(food) {
    if (food.price) < this.balance {
      console.log('you should get it')
      this.balance -= food.price
      this.foodItems.push(food)
    }
    else {
      console.log('maybe you should get something else')
    }
  }
}

module.exports = Customer
```

An example of a customer class

这是一个三明治阶级的例子:

```
class Sandwich {
  constructor(type, price) {
    this.type = type
    this.price = price
  }

  order() {
    console.log(`You ordered a ${this.type} sandwich for $ ${this.price}.`)
  }
}

class DeluxeSandwich {
  constructor(baseSandwich) {
    this.type = `Deluxe ${baseSandwich.type}`
    this.price = baseSandwich.price + 1.75
  }
}

class ExquisiteSandwich {
  constructor(baseSandwich) {
    this.type = `Exquisite ${baseSandwich.type}`
    this.price = baseSandwich.price + 10.75
  }

  order() {
    console.log(`You ordered an ${this.type} sandwich. It's got everything you need to be happy for days.`)
  }
}

module.exports = { Sandwich, DeluxeSandwich, ExquisiteSandwich }
```

An example of a sandwich class

这个三明治类是使用装饰模式的地方。我们有一个*三明治*基类，它为订购普通三明治时发生的事情设置规则。顾客可能想要升级三明治，这仅仅意味着成分和价格的改变。

你只是想增加功能来提高价格并更新*豪华三明治*的三明治类型，而不改变*的订购方式。*尽管你可能需要一种不同的订购方式来购买*精致和威奇*菜肴，因为食材的质量发生了巨大的变化。

decorator 模式允许您动态地更改基类，而不会影响它或任何其他类。你不必担心实现你不知道的函数，比如接口，也不必在每个类中包含你不会用到的属性。

现在我们来看一个例子，在这个例子中，这个类被实例化，就好像一个顾客下了一个三明治订单。

```
const { Sandwich, DeluxeSandwich, ExquisiteSandwich } = require('./Sandwich')
const Customer = require('./Customer')

const cust1 = new Customer(57)

const turkeySandwich = new Sandwich('Turkey', 6.49)
const bltSandwich = new Sandwich('BLT', 7.55)

const deluxeBltSandwich = new DeluxeSandwich(bltSandwich)
const exquisiteTurkeySandwich = new ExquisiteSandwich(turkeySandwich)

cust1.buy(turkeySandwich)
cust1.buy(bltSandwich)
```

## 最后的想法

我曾经认为设计模式是这些疯狂的、遥远的软件开发指南。然后我发现我一直在用它们！

我提到的一些模式在很多应用程序中都有使用，这可能会让你大吃一惊。归根结底，它们只是理论。作为开发人员，我们有责任使用这一理论，使我们的应用程序易于实现和维护。

你在项目中使用过其他的设计模式吗？大多数地方通常会为他们的项目选择一种设计模式，并坚持使用它，所以我想听听你们都使用了什么。

感谢阅读。你应该在 Twitter 上关注我，因为我通常会发布有用/有趣的东西: [@FlippedCoding](https://twitter.com/FlippedCoding)