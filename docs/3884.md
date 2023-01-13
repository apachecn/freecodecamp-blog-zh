# 用 TinaCMS + Gatsby 编辑 Markdown 的 3 种方法

> 原文：<https://www.freecodecamp.org/news/3-ways-to-edit-markdown-with-tina-gatsby/>

**通过实时内容编辑增强您的静态网站！**？

在这篇文章中，我将探索 Tina 提供的在你的 Gatsby 网站上编辑 Markdown 的三种不同方法。您将学习如何为 Tina 设置页面查询和静态查询。

这篇文章不会涉及在盖茨比身上使用蒂娜的基础知识。请参考[文档](https://tinacms.org/docs/gatsby/manual-setup)关于如何最初设置 Tina 与 Gatsby。

## 页面查询和静态查询是怎么回事？

在我们开始使用 Tina 编辑 Markdown 之前，我们必须了解 Gatsby 如何使用 GraphQL 处理数据查询。

你几乎可以从盖茨比的任何地方获得数据。在我们的例子中，我们使用的是*降价*。当您构建站点时，Gatsby 会为所有数据创建一个 GraphQL 模式。然后在 React 组件中使用 [GraphQL](https://graphql.org/learn/) 来查询来源数据。

Gatsby 允许你以两种方式查询你的数据: [*页面查询和*](https://www.gatsbyjs.org/docs/static-vs-normal-queries/) 静态查询。
自从 Gatsby 中的 [React Hooks API](https://reactjs.org/docs/hooks-intro.html) 和 [`useStaticQuery` hook](https://www.gatsbyjs.org/docs/use-static-query/) 发布后，查询你的数据就非常容易了。

但是在某些情况下，您不能使用静态查询。首先，我们来探讨一下不同之处。

### 两个主要区别是:

*   页面查询可以接受 GraphQL 变量。静态查询不行。
*   页面查询只能添加到页面组件中。静态查询可以在所有组件中使用。

那么，为什么我们不能在静态查询中使用 GraphQL 变量呢？原因是静态查询不能像页面查询那样访问页面上下文。结果是静态查询将无法访问页面上下文中定义的变量。

您可以在您的`createPage`函数中定义您的`gatsby-node.js`文件中的页面上下文。在这里，您可以为页面提供不同的变量，这些变量将在构建时注入到页面中。

我尽可能多地使用静态查询，因为我喜欢 hooks API 和它带来的组合可能性。例如，您可以创建自定义挂钩，并在多个组件中重用它们。

假设您有一个 GraphQL 查询，它在多个页面上获取您想要的元数据。创建一个带有`useStaticQuery` Gatsby 钩子的自定义 React 钩子。然后，您可以在任何需要的地方使用您的定制钩子，并且总是可以轻松地将数据放入您的组件中。当您需要在组件中包含变量时，您必须使用页面查询。页面查询不能与 hooks API 一起使用，必须是唯一的，并附加到特定的页面组件。

静态查询的另一个优点是，您可以在需要数据的组件中获取数据。这防止了 [*道具钻*](https://kentcdodds.com/blog/prop-drilling) 并且你的数据更紧密地耦合到使用它的组件。

## 反应概述

为了获取数据，我们可以使用 Gatsby 的查询选项。为了构造我们的组件，React 还提供了几个选项。您可以将您的组件创建为一个[类或者一个功能组件](https://reactjs.org/docs/components-and-props.html)。在 React Hooks API 之前，您必须使用类组件在组件中拥有状态。现在有了钩子，你可以用功能组件来做这件事。？

## 用 Tina 编辑 markdown 的三种方法

考虑到在 Gatsby 中创建组件和处理数据的所有选项，我们必须为项目选择最合适的方法。Tina 可以使用所有这些选项，提供了三种不同的方法来编辑 Gatsby 的 Markdown，如下所述。

*   **remark form**——当您从 Gatsby 中的页面查询获取数据时使用的一个[高阶组件](https://reactjs.org/docs/higher-order-components.html)。该组件可以与功能组件和类组件一起使用。请注意这里的细微差别！渲染道具组件在命名上的唯一区别是小写的“r”。
*   **useLocalRemarkForm**—[React Hook](https://reactjs.org/docs/hooks-overview.html)，用于功能组件从静态或页面查询中获取数据。如果组件获取静态数据，Gatsby 的`useStaticQuery`钩子将被调用。
*   **remark form**—[Render Props 组件](https://reactjs.org/docs/render-props.html)，你可以在类组件中使用它，从静态或页面查询中获取数据。静态数据将通过 Gatsby 的`StaticQuery` render props 组件获得。

### remark form——如何使用它以及为什么它不能用于静态查询。

首先，让我们深入了解如何将 TinaCMS 与页面查询联系起来。TinaCMS 中的
分量是一个[高阶分量](https://reactjs.org/docs/higher-order-components.html)，简称为 HOC。这意味着它是一个接受另一个组件的函数，并将返回一个添加了功能的新组件。

如果你不熟悉 HOC 的，我建议你在 [**React 官方文档**](https://reactjs.org/docs/higher-order-components.html) 中阅读一下。在 React 世界中，它们被认为是“高级用法”。

`remarkForm`组件需要另一个组件作为参数，用于页面查询。页面查询将数据作为一个道具注入到组件中，我们从这个道具中访问数据。使用一个`useStaticQuery`钩子，数据被收集到一个变量中，这个变量是你在组件内部选择的。这意味着如果你在《盖茨比》中使用`useStaticQuery`钩子，你就没有组件来赋予`remarkForm` HOC。真扫兴。？这就是为什么你只能在页面查询中使用`remarkForm`组件。

那么**在 Gatsby 中，如何将这个组件用于页面查询**？首先，看看下面虚构的星球大战组件。它将展示连接一切所需的三个步骤:

```
// 1\. Import the `remarkForm` HOC
import { remarkForm } from 'gatsby-tinacms-remark'

const StarWarsMovie = ({ data: { markdownRemark } }) => {
  return <h1>{markdownRemark.frontmatter.title}</h1>
}

// 2\. Wrap your component with `remarkForm`
export default remarkForm(StarWarsMovie)

// 3\. Add the required ...TinaRemark fragment to your Page Query
export const pageQuery = graphql`
  query StarWarsMovieById($id: String!) {
    markdownRemark(fields: { id: { eq: $id } }) {
      id
      html
      frontmatter {
        title
        releaseDate
        crawl
      }
      ...TinaRemark
    }
  }
` 
```

上面的代码是一个显示星球大战电影信息的组件。目前，它只是显示一个标题，但它也可以显示电影介绍中的上映日期和爬行文本。但那是遥远星系的另一个故事了...⭐

这个例子的第一步是从 Gatsby 插件‘Gatsby-Tina CMS-remark’中导入`remarkForm`钩子。这是*让 TinaCMS 处理 Markdown 文件*的插件。

不需要在组件内部添加任何代码。它可以是按照你想要的方式构建的任何组件——功能性的或类的。对于组件本身，您唯一要做的就是在导出组件时用`remarkForm` HOC 包装它。

在准备好之前，还需要做一件事，就是在查询中添加 GraphQL 片段`...TinaRemark`。这是 TinaCMS 识别您的数据并在 TinaCMS 侧边栏中创建所需编辑器字段所必需的。之后，你只需要启动你的开发服务器来显示网站和 Tina 侧边栏。

很简单，不是吗？只需三个小步骤，你就会拥有一个漂亮的侧边栏来编辑你网站上的内容。？

但是如果您想使用静态查询而不是页面查询呢？

### 使用 LocalRemarkForm 进行救援！

我们已经了解到`remarkForm` HOC 不适用于静态查询。因此，我们必须找到另一种解决方案来使用 TinaCMS 的静态查询。

**好消息！**

`remarkForm`组件本质上是`useLocalRemarkForm`钩子的“包装器”。？它接受一个组件作为参数，用页面查询数据调用`useLocalRemarkForm`,并返回一个包含查询数据和与之连接的 TinaCMS 的新组件。

我们可以直接用`useLocalRemarkForm`钩子，不用`remarkForm` HOC。这对于静态查询或者我们更喜欢使用钩子的时候很有用！

看看下面的代码示例，了解一下`useLocalRemarkForm`是如何工作的。

```
// 1\. Import useLocalRemarkForm hook
import React from ‘react’;
import { useLocalRemarkForm } from ‘gatsby-tinacms-remark’;
import { useStaticQuery } from ‘gatsby’;

const StarWarsMovie = () => {
  // 2\. Add required TinaCMS fragment to the GrahpQL query
    const data = useStaticQuery(graphql`
      query StarWarsMovieById {
        markdownRemark(fields: { id: { eq: "sw-01" } }) {
          id
          html
          frontmatter {
            title
            releaseDate
            crawl
          }
          ...TinaRemark
        }
      }
    `);
  // 3\. Call the useLocalRemarkForm hook and pass in the data
  const [markdownRemark] = useLocalRemarkForm(data.markdownRemark);
  return <h1>{markdownRemark.frontmatter.title}</h1>
}

export default StarWarsMovie; 
```

这只是一个说明`useLocalRemarkForm`如何工作的示例组件。在现实世界中，使用静态查询并不是最佳的解决方案。正如你所看到的，这是因为你不能在`useStaticQuery`钩子中使用变量来使它动态化。你必须硬编码电影 id。所以这个查询只适用于特定的电影，这是不好的。

让我们来分析一下这里发生了什么:

1.  我们导入了`useLocalRemarkForm`定制钩子，这样我们就可以在我们的组件中使用它。
2.  和以前一样，GraphQL 查询中需要`...TinaRemark`片段。所以我们在那加了一个。
3.  当我们从 Gatsby `useStaticQuery`钩子中获得数据后，我们可以用这些数据调用 TinaCMS `useLocalRemarkForm`钩子。这个钩子将返回一个包含两个元素的数组。第一个元素实际上是我们用来调用钩子的数据。它有相同的形状，并准备在我们的组件中使用。第二个元素是对 Tina 表单的引用。我们不需要那个，所以我们不会像破坏`markdownRemark`那样破坏它。

如果你对这一行感到疑惑:

```
const [markdownRemark] = useLocalRemarkForm(heroData.markdownRemark) 
```

就是一个 [ES6 解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)的例子。当我们得到一个包含两个元素的数组时，我析构了第一个元素(这是我们的数据)并将其命名为`markdownRemark`。你想叫它什么都可以。

### RemarkForm -渲染道具组件

不能在类组件上使用 React 挂钩。这就是为什么 Tina 提供了一个使用[渲染道具](https://reactjs.org/docs/render-props.html)模式的 [`RemarkForm`](https://tinacms.org/docs/gatsby/markdown/#2-the-render-props-component-remarkform) 组件。

该组件可以处理页面查询和静态查询。我将在下面的页面查询中展示如何使用它。

看看下面的例子:

```
// 1\. import the RemarkForm render prop component
import { RemarkForm } from '@tinacms/gatsby-tinacms-remark'

class StarWarsMovie extends React.Component {
  render() {
    /*
     ** 2\. Return RemarkForm, pass in markdownRemark
     **    to the remark prop and pass in what you
     **    want to render to the render prop
     */
    return (
      <RemarkForm
        remark={this.props.data.markdownRemark}
        render={({ markdownRemark }) => {
          return <h1>{markdownRemark.frontmatter.title}</h1>
        }}
      />
    )
  }
}

export default StarWarsMovie

// 3\. Add the required ...TinaRemark fragment to your Page Query
export const pageQuery = graphql`
  query StarWarsMovieById($id: String!) {
    markdownRemark(fields: { id: { eq: $id } }) {
      id
      html
      frontmatter {
        title
        releaseDate
        crawl
      }
      ...TinaRemark
    }
  }
` 
```

好，让我们再来看看这里发生了什么:

1.  我们导入`RemarkForm`组件供我们在代码中使用。
2.  在我们的 return 语句中，我们返回了`RemarkForm`组件，并传入了它的预定义的和必需的属性。备注属性为`RemarkForm`提供来自页面查询的降价数据。渲染道具通过函数或者渲染道具获取我们想要渲染的 JSX。`RemarkForm`将连接蒂娜编辑数据，然后渲染渲染属性函数中指定的任何内容。
3.  就像之前一样，我们必须将`...TinaRemark`片段添加到页面查询中。

## 后续步骤

**就这样**！在《盖茨比》中使用 Tina 编辑降价文件的三种方法。？

在这篇文章中，我们学习了如何在 Gatsby 中用静态查询和页面查询来设置 Tina。我们还学习了根据 React 组件的类型用 Tina 编辑 markdown 的三种不同方法。

这只是让你开始的基础。如果你喜欢 Tina 并想了解更多，你应该查看一下官方文件。那里有更多可以阅读的内容和一些有趣的用例。

例如，你可以学习如何应用[内嵌编辑](https://tinacms.org/docs/gatsby/inline-editing)以及如何[定制 Tina 工具条中的表单域](https://tinacms.org/docs/gatsby/markdown#customizing-remark-forms)。

Tina 是 React 生态系统和像 Gatsby 这样的静态站点生成器的一个很好的补充。它为你的网站提供了一种轻松愉快的方式来编辑和交互你的内容。我很高兴看到 TinaCMS 将会有多大，以及随着它的发展它能做些什么！

> 最初发表于[TinaCMS.org](https://tinacms.org/blog/three-ways-to-edit-md/)

## 更多的阅读和学习

[蒂娜官方文档](https://tinacms.org/docs/)

[蒂娜社区](https://community.tinacms.org/)

Twitter 上的 Tina:[@ Tina _ CMS](https://twitter.com/tina_cms)

* * *

欢迎通过以下链接联系我:

观看 Weibenfalks 教程[蒂娜&盖茨比](https://www.youtube.com/watch?v=eZWJ9ZtF61A&t=265s)。
Twitter — [@weibenfalk](https://twitter.com/weibenfalk) ，
Weibenfalk on [Youtube](https://www.youtube.com/c/weibenfalk) ，
Weibenfalk [课程网站](https://www.weibenfalk.com)。