# 如何在 React 中使用 YouTube IFrame API

> 原文：<https://www.freecodecamp.org/news/use-the-youtube-iframe-api-in-react/>

作为一家数字原型制作公司，[我的公司](https://spin-up.io)与客户合作，帮助他们与客户和投资者一起快速测试他们想法的核心。

我们的一个客户是一群医生，他们正在探索帮助老年患者进行术后护理的方法。

该应用程序有许多分支，但其中之一是与患者分享练习并跟踪他们的成功。这些练习在 YouTube 上进行，并在 NextJS 应用程序中共享。

上周，我们收到产品经理的请求:

> 嘿，
> 我们可以触发一个弹出窗口来询问患者是否完成了练习吗？这应该在视频结束或暂停超过大约 95%时触发。

到目前为止，我们嵌入了 YouTube 播放器，没有与 API 交互。这会很有趣的。

## 浏览文档

YouTube 播放器 API 的文档非常清楚。我可以看到有各种事件侦听器和数据属性可供探索。我遇到的一个小问题是，它是用普通的 JavaScript 范式编写的，而我是在 React 中工作的。

我以前做过这种转换。探索`refs`的创建和传递，在 NextJS 中使用`_document.js`来使用外部脚本，以及在您完成这样一个项目时处理一些使自己为人所知的特质。

在我深入研究之前，我决定寻找一个 React 库，它可能会为我处理所有这些问题。

## react-youtube 库

有时很难找到你要找的图书馆，有时则不然。在这种情况下，对这个库的描述似乎正是我想要的:

> 简单的 [React](http://facebook.github.io/react/) 组件充当 [YouTube IFrame 播放器 API](https://developers.google.com/youtube/iframe_api_reference) 上的一个薄层

我没有发现这个库的文档很有帮助，但是，因为它声称是一个很薄的层，我希望我可以依靠 YouTube 文档本身(剧透警告:我可以！).

## 规划项目

我需要:

*   一个模型和一种跟踪其状态的方法；
*   将 YouTube URL 转换为`react-youtube`需要的 videoid 的功能
*   测试视频是否在目标点的功能
*   `react-youtube`嵌入。

## 反应模态

我倾向于喜欢名字起得好的项目，这是其中的一个。

对于一些人来说，使用 modals 库似乎有点过分，但是我是一个爱好者。如果没有这样的库，我将不得不自己构建许多功能(键盘事件、可访问性、在模式外单击)。

文档给出了一些不错的默认样式，所以我将把它们添加到项目的顶部。

```
const modalStyles = {
  content: {
    top: "50%",
    left: "50%",
    right: "auto",
    bottom: "auto",
    marginRight: "-50%",
    transform: "translate(-50%, -50%)"
  }
};
```

在我的函数中，我将添加一个`useState`钩子来处理模态的状态。

```
 const [modalIsOpen, setModalIsOpen] = React.useState(false);
```

现在我准备添加我的模态。我通常把它加在组件的底部，但是实际上你可以把它加在任何有意义的地方。

```
 <Modal
        isOpen={modalIsOpen}  
        onRequestClose={() => setModalIsOpen(false)}
        contentLabel="Exercise Completed"
        style={modalStyles}
      >
        <div>
          <h3>Completed the exercise?</h3>
          <button
            onClick={handleExerciseComplete}
          >
            Complete exercise
          </button>
        </div>
      </Modal>
```

有几件事需要注意:

*   isOpen 属性采用我们在上面创建的状态值。
*   onRequestClose 正在将该状态值切换为 false。你可以有一个单独的句柄函数，但这似乎有点大材小用。
*   style prop 正在接收我们上面创建的常量。

然后，在模态中，我们提出问题，并提供一个按钮，如果病人确实完成了这个问题，就点击这个按钮。我不打算探究我们在实时代码中用`handleExerciseComplete`函数做了什么，所以现在我们只登录控制台。

```
const handleExerciseComplete = () => console.log("Do something");
```

## 准备视频 ID

`react-youtube`库使用视频 ID 而不是 URL。我们的内容团队更喜欢 URL，我不想让他们的日子更难过。

通常，我从一个内容管理系统中获取，但是对于这个例子，我将添加一个带有`useState`的输入字段来跟踪值。

```
const [videoUrl, setVideoUrl] = React.useState("");
```

```
<input value={videoUrl} onChange={(e) => setVideoUrl(e.target.value)} />
```

厉害！现在，我们需要能够从 URL 获得 id。如果你从来没有仔细看过一个 YouTube 网址，它可能看起来像这样【https://www.youtube.com/watch?t=746[T4【v = LRini _ YIs2I&feature = youtu . be](https://www.youtube.com/watch?t=746&v=LRini_YIs2I&feature=youtu.be)。视频 id 是在`v=`之后和`&`之前的字符串。

URL 的一个更简单的形式可以看起来像这样[https://www.youtube.com/watch?v=N1pIYI5JQLE](https://www.youtube.com/watch?v=N1pIYI5JQLE)，我们也需要能够处理这个。

这是我解决那个问题的尝试。

```
 let videoCode;
  if (videoUrl) {
    videoCode = videoUrl.split("v=")[1].split("&")[0];
  }
```

## 检查经过的时间

YouTube API 提供了许多帮助功能。我们要用的两个是`.getDuration()`和`.getCurrentTime()`。

我们将使用这两个值来检查是否超过 95%的视频已经过去。如果有，我们将触发模态打开。

```
 const checkElapsedTime = (e) => {
    const duration = e.target.getDuration();
    const currentTime = e.target.getCurrentTime();
    if (currentTime / duration > 0.95) {
      setModalIsOpen(true);
    }
  };
```

`e.target`相当于 YouTube 文档中的`player`。因此，你可以[查看这些文档](https://developers.google.com/youtube/iframe_api_reference#onStateChange)，为你的项目找到与视频互动的其他方式。

## react-youtube

现在，我们终于可以添加 YouTube 组件了。我们将使用包装器的`onStateChange` prop，并将我们的函数传递给它。

```
 <YouTube
            videoId={videoCode}
            onStateChange={(e) => checkElapsedTime(e)}
          />
```

现在看到这个感觉有点不合时宜，但是我们结束了。我们将事件传递给我们的`checkElapsedTime`函数，如果它通过了条件，模态将打开。

有很多其他方法可以连接到这个 API。该文档列出了以下内容:

```
 onReady={func}                   
  onPlay={func}                   
  onPause={func}                    
  onEnd={func}                     
  onError={func}                    
  onStateChange={func}              
  onPlaybackRateChange={func}      
  onPlaybackQualityChange={func} 
```

其中的每一个都将接受一个类似我们上面创建的函数。

## 尝试一下

我已经用本文中的示例代码建立了一个[代码沙箱](https://codesandbox.io/s/react-youtube-demo-f6l29)。走到那里，把一个 YouTube 的 URL 放到输入框里。测试当您浏览视频时会发生什么，特别是当您暂停或跟踪到 95%或更多时。