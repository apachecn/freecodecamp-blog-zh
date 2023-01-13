# 这种常见的 Redux-saga 模式的集合将使您的生活更加轻松。

> 原文：<https://www.freecodecamp.org/news/redux-saga-common-patterns-48437892e11c/>

安德烈斯·米哈斯

# 这种常见的 Redux-saga 模式的集合将使您的生活更加轻松。

![joGl-sdh8Pw3yT6fyRHbIPGUTTNbepcAcSLs](img/20ba51102ab8e35987c5e86bb2eb5579.png)

Photo by [Lauren Mancke](https://unsplash.com/photos/sil2Hx4iupI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这是一个由两部分组成的系列，你可以在这里随意查看第一部分[。](https://medium.freecodecamp.org/async-operations-using-redux-saga-2ba02ae077b3)

我成为 redux-saga 用户已经一年了，我仍然记得我被介绍到图书馆的时候。我记得当时我是多么惊讶(那个‘灵光一现’的时刻！)当我在几个小时内解决了几个问题。

它太棒了，以至于我需要和其他人分享这一切——所以我坐下来写了一篇关于它的[帖子](https://medium.freecodecamp.org/async-operations-using-redux-saga-2ba02ae077b3)。从那天起，由于知识的[诅咒](https://en.wikipedia.org/wiki/Curse_of_knowledge)，我无法想象没有它的生活。

它是如此之好，以至于现在我还将它用作编排层来管理在 [Shiftgig](http://www.shiftgig.com) 工作的所有异步操作。所以，是的，这些代码行有助于支持几百万风投公司的日常运营——我们的应用程序架构很大一部分依赖于它。

如果你对此感兴趣，另一篇文章可以给你指明方向。

**补充说明:**如果你的公司非常依赖这个或任何开源项目，我强烈建议你(和你的雇主)成为这个项目的[支持者。这小小的捐赠可以带来很大的不同，这是一个建立善缘的好方法。？？。](https://opencollective.com/redux-saga#contributors)

### 情绪化的介绍到此为止——让我们看看模式。

这篇文章假设你对这个库有基本的了解。如果你需要更多关于如何注册一个传奇，甚至如何配置的信息，请参考本系列的第一部分[这里](https://medium.freecodecamp.org/async-operations-using-redux-saga-2ba02ae077b3)。

经过一年与图书馆的合作和解决问题，我们已经确定了一些我们反复重复的模式。让我们一个一个地看看它们，每个都有一个可能的用例。

声明:我用了我知道的名字。但是如果你碰巧知道他们的一些官方名字，请在下面评论。

### 拿着叉子

从定义上来说，是我清单上最常见的。您还记得以前在 angular 1 应用程序中到处放置监听器和观察器的方法吗？嗯，我有点喜欢…？。

这种模式主要用于在一个动作被分派后触发一个流程——是的！像一个听众:

```
/* this is the saga you are going to register */export function* aListenerOnlySaga() {  const somePossibleData = yield take('SOME_ACTION')  yield fork(someOtherSagaProcess)}
```

```
function* someOtherSagaProcess() {    /* Any process calculation you need to do */}
```

#### 使用案例

有很多…但是让我们保持真实。在我们的应用程序中，我们需要支持不同的分支/状态，这些分支/状态需要显示信息并根据当前选择采取行动。

如果我是玛莎，一个 45 岁的秘书，不喜欢技术，我需要能够从下拉列表中选择一个分支，并通过魔法，查询与之相关的信息。

```
/* Some ugly react component*/class CompanyDropDown extends React.Component {   state = {      company: null,       branches: [],   }   componentDidUpdate ({company, branches}) {     this.setState(({company}) => ({company, branches}))   }   onChangeCompany (company) {      this.props.dispatch('company_change', company)   }   render () { /* omitted for convenience */}}
```

```
const mapStateToProps = ({company, branches}) => ({company, branches})
```

```
export connect(mapStateToProps)(CompanyDropDown)
```

一些细节，如渲染方法和减少，将被省略，以直接进入要点。

```
/* somewhere in your code... */export function* listenForChangeCompany() {   /* this variable holds the argument passed */   const company = yield take('company_change')   yield fork(changeCompanySaga, company)}
```

```
function* changeCompanySaga(company) {   const branchesPerCompany = yield call(getBranchesByCompany, company)   yield put({     type: 'company_change_success',     payload: branchesPerCompany,   })}
```

现在我们的 UI 与我们的业务逻辑分离了，我们很高兴。稍后我们将增加更多的复杂性。

这样做的主要好处是，您可以创建一个过程目录(稍后将详细介绍),它可以隔离特定的功能，并根据您的判断向您的团队公开。

但是这种模式有一个小问题。如果你注意到了，这只会起一次作用。执行该流程后，它将不再工作。这就是下一个模式派上用场的地方。

### 手表和叉子

Take and Fork 模式的一个问题是我们将执行次数限制为一次。如您所见，之前的用例可能与模式的使用不匹配。我是故意这样做的，这样我们就可以一步一步地根据我们的需要不断增强和增强它。

更适合的情况可能是登录或注销过程，在这种情况下，您知道您只需要它们一次。

继续这个案例，我们需要确保我们的朋友玛莎可以根据需要在不同的公司之间转换，而不仅仅是一次。我们可以稍微调整一下来解决这个问题。让我们看看手表和叉子的模式，让我们把我们的听众传奇再次带到游戏中。

```
export function* listenForChangeCompany() {   while (true) {      const company = yield take('company_change')      yield fork(changeCompanySaga, company)   } } /* eh viola! */
```

很漂亮，是吧？如果你不习惯使用函数发生器，那么使用 while/true 可能看起来很奇怪。但是符合目的。不过，还有一个更好的方法:我们可以使用另一个库助手快捷方式对此进行更多迭代。

```
/* Where you register the sagas */function* rootSagas () {   yield [    takeEvery('company_change', changeCompanySaga)   ]}
```

在幕后，公司参数被传递给 changeCompanySaga。我非常喜欢这种模式，尤其是当您需要处理一个有数百个进程的大型应用程序时。你只知道它响应一个单一的调度动作。

### 放和拿

这种模式非常有用。正如我之前提到的，您将您的流程操作组织成不同的传奇。然后，您创建一个服务目录，您可以在您指定的所有团队/人员/单位之间共享该目录。这意味着你的每个服务都有一个有限的功能来改变你的状态。有时这就足够了，而其他时候您希望扩展单个服务的功能。让我们看一个用例。

假设您公司的一个团队告诉您，他们已经创建了这个非常复杂的服务，您可以重用它。它叫做...`fetchDataOverFiveDifferentLocations`。这是很多必要的东西，但最终您将获得所有需要解析的信息并准备好使用。厉害！

您和您的团队就以下命名约定达成一致:{ service _ name } _ { microservice } _ { status }。所以让我们说:

*   **fetchSomeData_events** 这将开启传奇。
*   **fetchSomeData _ events _ start**这个动作一启动就被服务调度。
*   **fetchSomeData _ events _ success**该动作在完成时由服务调度。
*   **fetchSomeData _ events _ error**如果在处理过程中出现错误，则调度该动作。

这意味着我们的服务库公开了一个类似这样的传奇:

```
export function* fetchDataOverFiveDifferentLocations() {    while (true) {        yield put({type: 'fetchSomeData_events_start'})       /*         computing stuff...       */       yield put({type: 'fetchSomeData_events_success'})     }}
```

在您的应用程序中，您可以像这样使用服务:

```
function* rootSagas () {   yield [    takeEvery('fetchSomeData_events', fetchDataOverFiveDifferentLocations)   ]}
```

如果我们需要扩展那个功能呢？

```
/* We create a manager saga */function* fetchDataManager () {   /* we need to start the service/saga */   yield put({type: 'fetchSomeData_events'})   /* we need to wait/listen when it ends...*/   yield take('fetchSomeData_events_success')   /*      fork another process,     query info from the state,     do imperative stuff,     whatever you need to do when the previous saga finishes, the sky is the limit...    */}
```

```
/* We create an orchestrator saga */function* orchestratorSaga () {   while (true) {    yield fork(fetchDataManager)   }}
```

```
/* your root saga then looks like this */function* rootSagas () {   yield [    takeEvery('other_action_trigger', orchestratorSaga),   ]}
```

可能你们中的一些人在想…错误处理怎么样？先别急着想，我稍后再来谈这个。

### 用于收集

这一个是挑剔的，因为大多数时候我们默认不这样解决问题。但是当你需要它的时候，你就需要它。

假设我们从任何来源获取一个集合。我们收到 100 个对象，我们需要对每个对象应用一个操作/服务。换句话说，我们需要为每个元素分派一个或多个动作。通常，这是您可以在 reducer 中管理的东西，但是让我们保持服务目录的精神。

问题是，当你在一个传奇故事中，你不能做这样的事情:

```
function* someSagaName() {   /* code omitted for convenience */   const events = yield call(fetchEvents)   events.map((event) => {      /* this is syntactically invalid */      yield put({type: 'some_action', payload: event})   })}
```

这时，for/of 循环就来帮忙了。在我们开始打破我们的架构服务规则之前，让我们解决这个问题？？。

```
function* someSagaName() {   /* code omitted for convenience */   const events = yield call(fetchEvents)   for (event of events) {     yield put({type: 'some_action', payload: event}) /* ?? */     /* or maybe something like: */     yield fork(someOtherSagaOrService, event) /* ?? */   }} 
```

for/of 循环如何工作超出了本文的范围，但是你可以在这里找到更多的。此外，也可以使用常规的 for 循环并在数组上迭代——这由您决定。

### 错误处理

哦是的！Javascript 不是灵丹妙药，所以我们仍然需要做防御性编程，防止出错。？？‍?基于这种目录结构，我们如何确保不吞下错误？或者如何正确管理每个错误？500 不同于 401，因此我们仍然需要一种灵活的方式，以友好的方式向用户传达出现了问题。

我们使用的经验法则很简单:

*   所有的错误都在传奇内部处理。
*   管理流程的 saga 负责处理错误。

让我们回到我们的活动管理器服务:

1.  该服务是通用的。
2.  如果服务处理错误，我们就不能自定义错误。我们只是耦合到传统的错误。
3.  如果我们需要定制一个处理程序，我们需要创建一个处理错误的服务。

```
/* Case 1, service that manage the error */export function* fetchDataOverFiveDifferentLocations() {    try {      while (true) {          yield put({type: 'fetchSomeData_events_start'})         /*           computing stuff...         */         yield put({type: 'fetchSomeData_events_success'})       }    } catch (error) {      yield put({type: 'fetchSomeData_events_error', error})    }}
```

在这种情况下，我们会遇到服务错误，因此我们需要创建一个监听该操作的服务:

```
function* rootSagas () {  yield [   takeEvery('fetchSomeData_events_error', yourErrorHandlerService),   /* ... */,     ]}
```

到目前为止，我发现的唯一缺点是处理单个错误有多冗长。但它也给了你很大的灵活性，因为你决定你的减速器，如果你想对这个错误作出反应。重要的是它被捕获了，并且通知了您的应用程序。

```
/* Case 2, manager takes care of the error */function* fetchDataOverFiveDifferentLocations() {  while (true) {     yield put({type: 'fetchSomeData_events_start'})         /*           computing stuff...         */    yield put({type: 'fetchSomeData_events_success'})   }}
```

```
function* fetchDataManager () {  try {     yield put('fetchSomeData_events')     /*...*/     yield take('fetchSomeData_success')  } catch (error) {    yield put('some_custom_error_action', error)  }}
```

然后，您可以处理错误，例如，通过减速器。可能是布尔型。这取决于你的需求——这两种方式都非常有效。这将取决于你的情况和你与团队的协议。记住:**约定胜于配置**是关键。

### 结论

正如您可能看到的，当您需要一种可靠的方法来跨团队共享架构实践时，或者当您需要创建一个非常描述性的服务层时，这个库非常方便。最重要的是，它真的很容易扩展到其他人。

你使用任何其他模式吗？我总是很高兴知道其他人在做什么，以及我们如何相互学习。请让我知道！

现在终于可以随意查看我的开源项目了:

*   [**反应日历多日**](https://github.com/andresmijares/react-calendar-multiday)

[**平庸的工程师**](https://www.youtube.com/channel/UCSBzbeNuDamKpX6N4Q5SaHA)
[*更多类似这样的内容，请考虑订阅我的频道*www.youtube.com](https://www.youtube.com/channel/UCSBzbeNuDamKpX6N4Q5SaHA)