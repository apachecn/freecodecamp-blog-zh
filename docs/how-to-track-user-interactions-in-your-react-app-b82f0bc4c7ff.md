# 如何在 React 应用中跟踪用户交互

> 原文：<https://www.freecodecamp.org/news/how-to-track-user-interactions-in-your-react-app-b82f0bc4c7ff/>

作者:法乌齐·乌杜

# 如何在 React 应用中跟踪用户交互

![S1PoCnzhuzdNYifn3sWvFXEettiJZD7eEQr1](img/bce13d032026ccd58ec7b0a85eeddb85.png)

Credit: [Marketingtuig Digital Creatives](https://www.pexels.com/photo/analytics-text-185576/)

不要担心你需要哪个分析提供商来收集你的应用程序中的用户交互。

相反，更担心如何收集这些互动。

几个月前，我参与了一个大型电子商务组织的分析项目。该组织有一个数据驱动的业务，其中分析比什么都重要。

我们正在构建一个数据层解决方案，以便在将所有用户交互和动作推送给分析提供商(例如，Google Tag Manager)之前保存它们。我们在构建数据层解决方案时没有考虑 React，因为 React 的迁移是后来才开始的。

### 反应时间！

我们开始逐步迁移到 React，这意味着 React 只负责渲染平台的某些部分。我负责整合我们已经用 React Land 构建的数据层解决方案。

突然，困难开始出现了:

*   这个解决方案是基于 jQuery 的
*   这是不可预测的
*   很难测试和维护
*   与其他没有分析经验的开发人员分享知识是很可怕的！

我开始在社区中寻找适合我们需求的现成解决方案。只是没有机会。

这就是[反应追踪器](https://github.com/faouzioudouh/react-tracker)的想法出现的地方。

为什么 [React-tracker](https://github.com/faouzioudouh/react-tracker) ？

*   它易于使用、测试和维护(类似 Redux)
*   它可以与任何分析提供商一起使用
*   它是可扩展和可预测的
*   它有一个最小的 API

借助 React-tracker，我们能够轻松集成两个分析提供商(Google Tag manager 和 Adobe Analytics)。

### 怎么会？

为了简单起见，*把它想成 Redux* 。

*   实例化你的追踪器~ *存储你的事件*
*   创建您的事件监听器~ ~*缩减器*
*   事件~ *动作*
*   向根组件提供跟踪器实例。
*   React-tracker 将[神奇地](https://reactjs.org/docs/context.html)负责向所有组件提供你的 tracker 实例。

在实例化任何东西之前，让我们仔细检查上面列表中的每个术语并解释它。

#### 什么是追踪器？

跟踪器是一个包，它保存跟踪历史以及一些监听/调度事件的功能。

*   每当调度一个`event.type`等于给定的`eventType`的事件时，就会调用给定的回调。
*   `tracker.trackEvent(event)`是一个接受一个`event`并调用所有监听这个`event`的事件监听器的函数。
*   `tracker.getHistory()`返回一个数组，包含所有已保存的跟踪事件

#### 什么是事件？

事件是表示用户交互的普通对象，比如用户点击、页面浏览和购买。

它应该是一个带有`type`和关联`data`的对象(如果有)。下面是一个`PageView`事件的例子:

```
const PageViewEvent = {  type: 'PAGE_VIEW', // Required  data: { // Optional    pageId: '123',    userId: 'UID-123'  }}
```

#### 什么是事件监听器？

event-listener 是一个函数，如果它的`eventType`与调度事件的类型匹配，就会被调用。

`eventListener.eventType === event.type`

事件监听器的示例:

```
const pageViewListener = (event, ) => {  // For example let's push the received event to our DataLayer.  window.dataLayer.push(event);
```

```
 return event;};
```

让我们允许我们的`pageViewListener`只监听`PAGE_VIEW`事件:

```
pageViewListener.eventType = 'PAGE_VIEW';
```

这里有四点需要注意:

*   返回事件会将其保存在 trackingHistory 中。否则将被忽略:)
*   如果没有为事件监听器指定`eventType`，它将在每个事件分派时被调用。
*   `eventHistory`是作为第二个参数提供的，以帮助您轻松地对您的事件应用限制，比如跟踪产品-单击一次。为了实现这一点，你需要掌握事件的历史。
*   将我们的活动推迟到`window.dataLayer`只是一个例子。你可以在这个函数中做任何事情，比如直接调用`GTM`或者`Facebook Pixel`

### 是时候整合一切了

首先要做的是:

#### 1.实例化我们的英雄`Tracker:`

```
import { Tracker } from 'react-tracker';
```

```
const tracker = new Tracker();
```

就是这样！

现在我们有了我们的`Tracker`，但是没有事件监听器:-(

有两种方法可以将事件监听器添加到您的`Tracker`:

*   实例化时:

```
const anOtherTracker = new Tracker([  pageViewListener,  productClickListener,  ...]);
```

*   或者您可以在使用`on`实例化您的`Tracker`之后添加事件监听器:

```
const anOtherTracker = new Tracker();
```

```
tracker.on('PAGE_VIEW', pageViewListener);
```

#### 2.创建页面视图事件监听器:

我希望我的事件监听器将接收到的`PAGE_VIEW`事件直接推送到我的`dataLayer.`

```
const pageViewListener = (event, trackingHistory) {
```

```
 window.dataLayer.push(event);
```

```
};
```

让我们的`tracker`了解一下`pageViewListener`:

```
tracker.on('PAGE_VIEW', pageViewListener);
```

#### 3.创建事件创建者:

Event-creator 只是一个返回事件对象的函数:

```
const pageViewEvent = (pageId, userId) => ({  type: 'PAGE_VIEW',  data: {    pageId,    userId  }});
```

我们的追踪器现在已经配置好了。

### 介绍我们的`tracker`反应过来

![5pYC8r-p6vhMiA9nRpopQDn4QK25YObvq7oG](img/3336a27ed30cfa45d927ed6a2bde22ab.png)

Credit: [rawpixel.com](https://unsplash.com/photos/sHzMcXkJNrw)

#### 4.向根组件提供我们的`tracker`:

```
import React from 'react;import ReactDOM from 'react-dom';import { TrackerProvider } from 'react-tracker'
```

```
import RootComponent from '../RootComponent';
```

```
const RootComponentWithTracking = (  <TrackerProvider tracker={tracker}>    <RootComponent />  </TrackerProvider>);
```

```
const domElement = document.getElementById('root');
```

```
ReactDOM.render(<RootComponentWithTracking />, domElement);
```

通过将我们的`tracker`提供给根组件，它将[神奇地](https://reactjs.org/docs/context.html)可用于所有子组件。

现在，既然我们有了可用的`tracker`，让我们用它来跟踪`RootComponent`挂载上的`PAGE_VIEW`事件。

#### 4.轨道`Page View Event.`

```
import React from 'react';import { withTracking } from 'react-tracker';// We created this function earlier at (3.)import { pageViewEvent} from '../tracking/events';
```

```
class RootComponent extends React.Component {  componentDidMount() {    this.props.trackPageView(this.props.pageId, this.props.userId)  }
```

```
 render() {    return (<h1> My App is awesome </h1>)  }};
```

```
const mapTrackingToProps = trackEvent => ({  trackPageView: (pageId, userId) =>     trackEvent(pageViewEvent(pageId, userId))});
```

```
export default withTracking(mapTrackingToProps)(RootComponent);
```

HOC 将负责从我们的`tracker`向我们提供`trackEvent`，这样我们就可以用它来跟踪`pageView`事件。

`mapTrackingToProps`会将返回的物体与`RootComponent`的道具合并，这意味着`trackPageView`将在`RootComponent.`内作为道具可用

就这样，你完成了；)

#### 5.演示

请参考这个[演示](https://faouzioudouh.github.io/react-tracker/)和 [GitHub](https://github.com/faouzioudouh/react-tracker) 以获得更深入的文档和更好的方法来组织您的跟踪文件。

### 试试看！

[React-tracker](https://github.com/faouzioudouh/react-tracker) 旨在通过提供最小的 API 和与 React 应用的轻松集成，尽可能促进分析工具的集成。

### 谢谢

感谢[多哈·法里迪](https://www.freecodecamp.org/news/how-to-track-user-interactions-in-your-react-app-b82f0bc4c7ff/undefined)、[阿卜杜拉里·艾拉姆利](https://www.freecodecamp.org/news/how-to-track-user-interactions-in-your-react-app-b82f0bc4c7ff/undefined)和[哈立德·本拉菲克](https://www.freecodecamp.org/news/how-to-track-user-interactions-in-your-react-app-b82f0bc4c7ff/undefined)的有益反馈。