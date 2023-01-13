# React 中的代码拆分——可加载组件的拯救

> 原文：<https://www.freecodecamp.org/news/code-splitting-in-react-loadable-components/>

代码分割是一种将大文件中的代码分割成较小的代码束的方法。然后，可以按需或并行请求这些功能。

现在，这不是一个新概念，但它可能很难理解。

当您进行代码分割时，保持 HTML、CSS 和 JavaScript 的包尽可能小是很重要的。但是通常随着应用程序的扩展，更大的包是不可避免的。这可能会对你网站的[网站生命周期](https://web.dev/vitals/)产生负面影响。

因此，在本文中，我们将了解代码拆分是如何工作的，以及如何做好它。

## React 代码库中的代码拆分

下面列出了一些常见的代码拆分方法:

### 使用 Webpack 动态导入语法进行代码拆分

看看下面的例子:

`import(“module_name”).then({ default } => // do something)`

这个语法将让将要进行代码分割和捆绑的 Webpack 文件相应地生成。Webpack 中还有其他方法，比如使用**手动入口点**和 **SplitChunksPlugin** 。您可以在[文档](https://webpack.js.org/guides/code-splitting/)中找到更多信息。

### 使用 React 进行代码拆分。懒惰和悬念

这是 React 直接提供的功能。让我们从官方文档本身深入下面的例子:

```
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

这非常类似于 Webpack 动态导入语法，其中暂停组件可用于提供加载状态，直到组件被延迟加载。

我在另一篇文章中探讨了这一点，我谈到了 React 的未来，并带着悬念展开。

## React 中何时使用代码拆分

关于代码分割的唯一缺点是你只能在客户端渲染中使用它。

当涉及到反应🥺.时，上述两种技术在服务器端渲染时都不起作用这就是 React 团队建议在服务器中使用可加载组件进行代码拆分的原因。

## 但是为什么要使用可加载组件呢？

我听到并感觉到你。我也花了一段时间才想明白。

让我们来看一个例子。

让我们回到最初的问题陈述:我们希望在服务器端渲染(SSR)期间进行代码拆分。到目前为止，我们的武器库中最好的工具是 Webpack 的动态导入语法，我们在上一节中简要地讨论了它。

所以我们将构建一个名为 **AsyncComponent** 的组件，它的职责是接受模块并使用动态导入。

它将有三种状态:

*   isLoading *// as 动态导入返回承诺*
*   组件 *//从代码拆分得到的实际模块*
*   错误 *//导入失败时登录*

### 预期行为

我们的 AsyncComponent 将接受任何导入模块并解析它。如果成功，我们将把 isLoading 状态设置为 false，并呈现已解析的组件。

让我们快速浏览一下上面描述的代码:

```
import React from 'react';

class AsyncComponent extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            isLoading: true,
            component: null,
            error: null
        }
    }

    componentWillMount() {
        const { importModule } = this.props;
        importModule()
            .then((module) => {
                const component = module.default;
                console.log(component)
                this.setState(({
                    component,
                    isLoading: false
                }))
            })
            .catch(err => {
                console.log(error)
                this.setState({
                    isLoading: false,
                    error
                })
            })
    }

    render() {
        const { isLoading, component, error } = this.state;

        if(isLoading) {
            return <div>Loading</div>
        }
        if(error) {
            return <div>{error}</div>
        }
        return <div>{component}</div>
    }
}

const A = () => {
    return (
        <div>
            <AsyncComponent importModule={() => import('./F')} />
        </div>
    )
}

// We keep a reference to prevent uglify remove

export default A
```

### 实际行为

但是这个不行。因为我们试图在服务器端这样做，React 渲染生命周期不会等待动态导入来解决，这是一个承诺。所以我们会卡在加载屏幕上，这一点从下面我进行的实验的截图中可以明显看出。

![dC4gCS9bG_07BiEbljwNtEWu_ODXXizy8geQ45XqmNJLEcl_KlliePS3niWZnrNSnpSR1qwFNNuw4h91iyN-_xRgTIZ8WWrDlTR5ruUm1pu2SuRl_v6xlbiuWb6zJepfoxYi_UnT](img/cb651730d2816b58505ae57d0d5345a0.png)

这就是为什么 React 中的代码拆分在我们试图用现有工具简单地完成时并不简单。

## 在 React 中输入可加载组件

为了解决这个问题，我们有了[可加载组件](https://loadable-components.com/docs/server-side-rendering/)，这是 react 官方文档推荐的一个库。

我不会进入配置部分，因为官方文档有足够的例子和步骤，涵盖了您需要知道的内容。让我们从代码的角度来看。

```
import React from 'react';
import loadable from '@loadable/component';

const F = loadable(() => import('./F'))

const A = () => {
    return (
        <div>
            {/* <AsyncComponent importModule={() => import('./F')} /> */}
            <F />
        </div>
    )
}

// We keep a reference to prevent uglify remove

export default A
```

在我们当前的例子中，我们已经用一个**可加载组件替换了 **AsyncComponent** 。**

violà——它起作用了！

![wAkMpJ9Zsd-v6Pjj8rBSJe1N7ItLRSfq4OXtZuQO9ppOTYJsCCfkN3AySTDBbR8ulmsbMfvSh04U24l0G1dUWxMB1MfnwF1IML-DqZiaYXDGHp4MJkZjnzfNvyivRnAXpCJTZPiU](img/89a0ac79ec6a5c29af318103dfefc396.png)

这确实是一个复杂的问题，但是可加载的组件使它看起来很容易。文档也很容易理解！

babel 可加载插件在组件中传输可加载语法，并在服务器端渲染期间为 HTML 准备最终字符串，在客户端渲染期间准备动态导入语法。

请注意，借助于可以打开/关闭的 SSR 标记，可加载组件可以用于客户机和服务器中的代码拆分。

## 结论

我写这篇文章是想谈谈为什么可加载组件很重要，它们解决了什么问题。我还想表达为什么在服务器端渲染时代码分割是复杂的。我真的希望这能帮助你理解这个问题。

> **特别提到 [Kashyap](https://twitter.com/kgrz) 帮助我理解了* d *的概念。**

在 Twitter 上关注我，获取更多关于新文章的更新，并关注最新的前端开发。此外，在 Twitter 上分享这篇文章，以帮助其他人了解它。分享是关爱^_^.

### 资源

*   [https://loadable-components.com/docs/server-side-rendering/](https://loadable-components.com/docs/server-side-rendering/)
*   [https://webpack.js.org/guides/lazy-loading/](https://webpack.js.org/guides/lazy-loading/)
*   [https://webpack.js.org/guides/code-splitting/](https://webpack.js.org/guides/code-splitting/)
*   [https://reactjs.org/docs/code-splitting.html](https://reactjs.org/docs/code-splitting.html)

以下是我写的一些其他文章，你可能会感兴趣:

1.  [React 的未来，悬念迭起](https://blog.logrocket.com/the-future-of-react-unfolding-with-suspense/)
2.  [两次请求的故事——CORS](https://www.freecodecamp.org/news/the-story-of-requesting-twice-cors/)
3.  [迁移状态时如何使用 Redux Persist】](https://www.freecodecamp.org/news/how-to-use-redux-persist-when-migrating-your-states-a5dee16b5ead/)
4.  [还原可观察到的结果](https://codeburst.io/redux-observable-to-the-rescue-b27f07406cf2)
5.  [构建面向未来的 web app](https://codeburst.io/building-webapp-for-the-future-68d69054cbbd)