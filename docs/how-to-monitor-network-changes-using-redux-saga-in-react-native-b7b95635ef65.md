# 如何在 React Native 中使用 Redux Saga 监控网络变化

> 原文：<https://www.freecodecamp.org/news/how-to-monitor-network-changes-using-redux-saga-in-react-native-b7b95635ef65/>

由普里斯蒂·瓦伊迪亚

# 如何在 React Native 中使用 Redux Saga 监控网络变化

![9nkd8lAoE4QWGclLuTK3DYa45MdCYXZX34jJ](img/daac32322b5b04557738705adc7519a4.png)

Image Credit — [Unspash](https://unsplash.com/photos/iCxR1u0lHLA)

#### 为什么要用 Redux Saga 监控网络变化？

![icYVSeaXDsfe83z7SkYSyKE-AKKWjQF7XOE5](img/3af823ef40426f13982584b958bb3fcd.png)

Image Credit — [ImgFlip](https://i.imgflip.com/2knj8b.jpg)

Redux Saga 是 Redux 应用程序的*替代* *副作用*模型。更容易管理、执行、测试和捕捉错误。

与其在 react 组件中实现管理*网络状态*的逻辑，不如单独处理`side-effects`。Redux Saga 为我们提供了 *eventChannel* 作为处理此类案件的开箱即用解决方案。

### 什么是事件通道？

![1OSYcBjFfR0OqQ1JNH20hEvGHZ1Xrbzzv2em](img/a43fa6b7fc219bb05ae5c3fa58f332f6.png)

Image Credit — [Unsplash](https://images.unsplash.com/photo-1538131688925-7e0eb2e7828b)

Redux Saga 由作为工厂功能的`eventChannels`组成，用于在*外部事件*和*Saga*之间进行通信。事件来自事件源*而不是 redux 存储*。

这里有一个来自[文档](https://github.com/redux-saga/redux-saga/blob/master/docs/advanced/Channels.md#using-the-eventchannel-factory-to-connect-to-external-events)的基本例子:

```
import { eventChannel } from 'redux-saga'

function countdown(secs) {
  return eventChannel(emitter => {
      const iv = setInterval(() => {
        secs -= 1
        if (secs > 0) {
          emitter(secs)
        } else {
          // this causes the channel to close
          emitter(END)
        }
      }, 1000);
      return () => {
        clearInterval(iv)
      }
    }
  )
}
```

需要注意的事项:

*   `eventChannel`的第一个参数是一个监听器函数。
*   返回方法是*取消注册监听器*函数。

发射器*初始化监听器一次*，之后来自监听器的所有事件通过调用发射器函数被*传递给发射器函数*。

#### 我应该如何将 Redux Saga 的事件通道与 React Native 的网络(NetInfo) API 挂钩？

React Native 的 **NetInfo** `[isConnected](https://facebook.github.io/react-native/docs/netinfo#isconnected)` API 异步获取一个*布尔*，它决定设备是`online`还是`offline`。

#### 深入研究代码

![1MwK-maPG4EYywcUFOeyEAZTUxleIx8DSSM0](img/beecb384852601516fdc163e667e77fc.png)

Image Credit — [Unsplash](https://unsplash.com/photos/pgSkeh0yl8o)

**首先，我们需要创建一个启动通道方法。**

```
import { NetInfo } from "react-native"
import { eventChannel} from "redux-saga"

function * startChannel() {
  const channel = eventChannel(listener => {
    const handleConnectivityChange = (isConnected) => {
      listener(isConnected);
    }

// Initialise the connectivity listener
    NetInfo.isConnected.addEventListener("connectionChange", handleConnectivityChange);

// Return the unregister event listener.
    return () =>
      NetInfo.isConnected.removeEventListener("connectionChange",    handleConnectivityChange);
  });
}
```

下一步是**监听通道内的事件变化**。

```
...

while (true) {
  const connectionInfo = yield take(channel);
}

...
```

最后一步是**向通道传递一个自定义动作**，这样就可以使用您的动作来同步*值。*

```
...
function * startChannel(syncActionName) {

...
while (true) {
  const connectionInfo = yield take(channel);
  yield put({type: syncActionName, status: connectionInfo });
}
```

这个**通道**可以在我们默认导出的发电机中使用 [*调用*](https://github.com/redux-saga/redux-saga/tree/master/docs/api#callfn-args) 操作。

```
export default function* netInfoSaga() {
  try {
    yield call(startChannel, 'CONNECTION_STATUS');
  } catch (e) {
    console.log(e);
  }
}
```

然后，*导出的生成器*可以被*导入*，并在我们的主传奇中使用 [spawn/fork](https://github.com/redux-saga/redux-saga/tree/master/docs/api#spawnfn-args) 操作作为*分离任务*。

### 使用

上面的代码将它作为一个包`react-native-network-status-saga`添加进来，以包含一些更有用、更酷的参数。

以下是链接

*   [GitHub](https://github.com/pritishvaidya/react-native-network-status-saga)
*   [npm](https://www.npmjs.com/package/react-native-network-status-saga)

*感谢阅读。* *如果你喜欢这篇文章，请鼓掌支持这篇文章，并在媒体上与他人分享。*

在我的 [StackOverflow](https://stackoverflow.com/users/6606831/pritish-vaidya) 和 [GitHub](https://github.com/pritishvaidya) 个人资料中可以找到更多有趣的东西。

*在 [LinkedIn](https://www.linkedin.com/in/pritish-vaidya-506686128/) 、 [Medium](https://medium.com/@pritishvaidya94) 、 [Twitter](https://twitter.com/PritishVaidya) 上关注我，进一步更新新文章。*