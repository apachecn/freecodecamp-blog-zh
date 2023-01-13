# Next.js 中的路由–完整的初学者指南

> 原文：<https://www.freecodecamp.org/news/routing-in-nextjs-beginners-guide/>

Next.js 是一个 React 框架，提供了生产所需的所有特性。它通过使用基于文件系统的路由在您的应用程序中启用路由。

在本指南中，我将向您展示在 Next.js 中路由是如何工作的。

*   [next . js 中路由的工作原理](#how-routing-works-in-next-js)
*   [页面之间的链接](#linking-between-pages)
*   [通过路线参数](#passing-route-parameters)
*   [动态路线](#dynamic-routes)
*   [动态嵌套路线](#dynamic-nested-routes)

## Next.js 中路由的工作方式

Next.js 使用文件系统在应用程序中启用路由。Next 自动将`pages`目录下扩展名为`.js`、`.jsx`、`.ts`或`.tsx`的每个文件视为一个路径。

Next.js 中的页面是一个 React 组件，它有一个基于文件名的路由。

以此文件夹结构为例:

```
├── pages
|  ├── index.js
|  ├── contact.js
|  └── my-folder
|     ├── about.js
|     └── index.js 
```

在这里，我们有四页:

*   `index.js`可以在 [http://localhost:3000](http://localhost:3000) 上访问主页吗
*   `contact.js`是否可以在[http://localhost:3000/contact](http://localhost:3000/contact)上访问联系页面
*   `my-folder/index.js`位于子文件夹*我的文件夹*的页面在[http://localhost:3000/我的文件夹](http://localhost:3000/my-folder)上可访问吗
*   `my-folder/about.js`在[http://localhost:3000/my-folder/about](http://localhost:3000/my-folder/about)上可以访问位于子文件夹 *my-folder* 上的 about 页面吗

## 如何在页面之间链接

默认情况下，Next.js 会预渲染每个页面，以使您的应用程序快速且用户友好。它使用由 *next/link* 提供的`Link`组件来实现路由之间的转换。

```
import Link from "next/link"

export default function IndexPage() {
  return (
    <div>
      <Link href="/contact">
        <a>My second page</a>
      </Link>
      <Link href="/my-folder/about">
        <a>My third page</a>
      </Link>
    </div>
  )
} 
```

这里，我们有两条路线:

*   第一个链接指向页面`http://localhost:3000/contact`
*   第二个链接指向页面`http://localhost:3000/my-folder/about`

`Link`组件可以接收几个属性，但是只有`href`属性是必需的。这里，我们使用一个`<a></a>`标签作为链接页面的子组件。但是也可以在`Link`组件上使用任何支持`onClick`事件的元素。

## 如何传递路线参数

Next.js 允许您传递路由参数，然后使用`useRouter`钩子或`getInitialProps`取回数据。它允许您访问包含参数的路由器对象。

*   索引. js

```
import Link from "next/link"

export default function IndexPage() {
  return (
    <Link
      href={{
        pathname: "/about",
        query: { id: "test" },
      }}
    >
      <a>About page</a>
    </Link>
  )
} 
```

正如您在这里看到的，我们没有向`href`属性提供字符串，而是传递了一个包含`pathname`属性的对象。这是路由，以及保存数据的查询元素。

*   about.js

```
import { useRouter } from "next/router"

export default function AboutPage() {
  const router = useRouter()
  const {
    query: { id },
  } = router
  return <div>About us: {id}</div>
} 
```

这里，我们导入`useRouter`钩子来获取传入的数据。接下来，我们使用析构从`query`对象中提取它。

如果你使用的是服务器端渲染，你必须使用`getInitialProps`来获取数据。

```
export default function AboutPage({ id }) {
  return <div>About us: {id}</div>
}

AboutPage.getInitialProps = ({ query: { id } }) => {
  return { id }
} 
```

## 动态路线

Next.js 使您能够使用方括号(`[param]`)在应用程序中定义动态路线。您可以使用动态名称，而不是在页面上设置静态名称。

让我们以此文件夹结构为例:

```
├── pages
|  ├── index.js
|  ├── [slug].js
|  └── my-folder
|     ├── [id].js
|     └── index.js 
```

Next.js 将获取传入的路由参数，然后将其用作路由的名称。

*   索引. js

```
export default function IndexPage() {
  return (
    <ul>
      <li>
        <Link href="/">
          <a>Home</a>
        </Link>
      </li>
      <li>
        <Link href="/[slug]" as="/my-slug">
          <a>First Route</a>
        </Link>
      </li>
      <li>
        <Link href="/my-folder/[id]" as="/my-folder/my-id">
          <a>Second Route</a>
        </Link>
      </li>
    </ul>
  )
} 
```

这里，我们必须定义`as`属性的值，因为路径是动态的。路线的名称将是您在`as`道具上设置的名称。

*   【鼻涕虫】。射流研究…

```
import { useRouter } from "next/router"

export default function DynamicPage() {
  const router = useRouter()
  const {
    query: { id },
  } = router
  return <div>The dynamic route is {id}</div>
} 
```

您也可以通过客户端的`useRouter`钩子或服务器端的`getInitialProps`获得路线参数。

*   我的文件夹/[id]。射流研究…

```
export default function MyDynamicPage({ example }) {
  return <div>My example is {example}</div>
}

MyDynamicPage.getInitialProps = ({ query: { example } }) => {
  return { example }
} 
```

这里，我们使用`getInitialProps`来获得传入的动态路由。

## 动态嵌套路由

使用 Next.js，还可以用括号(`[param]`)嵌套动态路由。

让我们考虑这个文件结构:

```
├── pages
|  ├── index.js
|  └── [dynamic]
|     └── [id].js 
```

*   索引. js

```
export default function IndexPage() {
  return (
    <ul>
      <li>
        <Link href="/">
          <a>Home</a>
        </Link>
      </li>
      <li>
        <Link href="/[dynamic]/[id]" as="/my-folder/my-id">
          <a>Dynamic nested Route</a>
        </Link>
      </li>
    </ul>
  )
} 
```

正如您在这里看到的，我们像在前面的例子中一样设置了属性`as`的动态值。但是这一次，我们必须定义文件夹及其文件的名称。

```
import { useRouter } from "next/router"

export default function DynamicPage() {
  const router = useRouter()
  const {
    query: { dynamic, id },
  } = router
  return (
    <div>
      Data: {dynamic} - {id}
    </div>
  )
} 
```

这里，我们用`useRouter`钩子从查询对象中取出路由参数。

就是这样！感谢阅读。

如果你有兴趣全面学习 Next.js，我强烈推荐这门[畅销书课程](https://click.linksynergy.com/deeplink?id=o1JCNdqL0gw&mid=39197&murl=https%3A%2F%2Fwww.udemy.com%2Fcourse%2Freact-the-complete-guide-incl-redux%2F)。

你可以在[我的博客](https://www.ibrahima-ndaw.com)上找到类似这样的精彩内容，或者在 Twitter 上关注我[以获得通知。](https://twitter.com/ibrahima92_)

Javier Allegue Barros 在 [Unsplash](https://unsplash.com/s/photos/route?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片