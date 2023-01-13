# 如何使用 react-router-hash-link 在 React 中实现垂直滚动

> 原文：<https://www.freecodecamp.org/news/how-to-implement-vertical-scrolling-in-react-using-react-router-hash-link/>

平滑滚动是一种功能，它使网页更有用，并在大多数浏览器中提供更好的用户滚动体验。

在 react-router-dom 中实现平滑的页面滚动一直是 React.js 中的一个问题。

### 什么是反应路由器哈希链接？

根据其[文档](https://github.com/rafgraph/react-router-hash-link)，react-router-hash-link 是 React Router 在使用链接组件导航时无法滚动到#hash-fragments 的问题的解决方案。

### 先决条件。

您必须使用 react-router-dom 中的 *BrowserRouter* 来实现 react-router-hash-link 平滑滚动。要安装和使用 react-router，请输入以下命令:

```
npm i react-router-dom 
```

或者对于纱线:

```
yarn add react-router-dom 
```

为了理解如何实现垂直滚动，您将构建一个简单的带有导航栏和三个部分的登录页面。

## 如何使用反应路由器哈希链接

### 步骤 1:安装 react-router-hash-link

要安装 react-router-hash-link 包，请运行以下命令:

```
npm install --save react-router-hash-link
```

或者用纱线:

```
yarn add react-router-hash-link
```

### 步骤 2:将 react-router-hash-link 包导入 react 应用程序。

打开 LandingPage.js 文件，从 react-router-hash-link 包中导入 *hashlink* 。

```
import React from 'react';
import { HashLink } from 'react-router-hash-link';

const LandingPage = () => {
    return (
        <>
            <nav>
            . . .
            </nav>

            <section>
            . . .
            </section>
        </>
    )
}

export default LandingPage 
```

### 步骤 3:添加 Hashlink 组件以指向应用程序的不同区域

将 Hashlink 组件添加到 LandingPage.js 文件中，如下所示:

```
import React from 'react';
import { HashLink } from 'react-router-hash-link';

const LandingPage = () => {
    return (
    <>
        <nav>
            <HashLink smooth to="/#home">
                About
            </HashLink>

            <HashLink smooth to="/#services">
            	Profile
            </HashLink>

            <HashLink smooth to="/#testimonial">
            	Services
            </HashLink>
        </nav>

        <section>
        . . .
        </section>
    </>
    )
}
export default LandingPage 
```

### 步骤 4:将 Hashlink 的 ID 添加到任何元素中。

当你用你的 id 点击链接时，会实现一个滚动效果。您将看到滚动到一个元素或页面，该元素或页面的 id 与链接中的#hashfragment 匹配。

```
import React from 'react';
import { HashLink } from 'react-router-hash-link';

const LandingPage = () => {

    return (
    <>
        <nav>
            <HashLink smooth to="/#home">
            	About
            </HashLink>

            <HashLink smooth to="/#services">
            	Profile
            </HashLink>

            <HashLink smooth to="/#testimonial">
            	Services
            </HashLink>
        </nav>

        <section id=”about”>
        	<h1> About</h1>

            <p>
                Lorem ipsum dolor sit, amet consectetur adipisicing elit.
                Vero, nam! Iure officia aut esse tempore accusantium
                explicabo? Corporis deleniti ipsa fuga quas aut neque
                dicta nostrum laboriosam, iusto ullam minima est porro,
                totam saepe. Facilis aliquid praesentium, voluptates rem
                quibusdam sequi numquam illo eius adipisci eaque,
                necessitatibus consectetur, labore vero et ipsum.
                Officiis, ea vero. Praesentium, et. Enim, nostrum illo.
            </p>        
        </section>

        <section id=”profile”>
        	<h1> Profile </h1>
            <p>
                Lorem ipsum dolor sit, amet consectetur adipisicing elit.
                Vero, nam! Iure officia aut esse tempore accusantium
                explicabo? Corporis deleniti ipsa fuga quas aut neque
                dicta nostrum laboriosam, iusto ullam minima est porro,
                totam saepe. Facilis aliquid praesentium, voluptates rem
                quibusdam sequi numquam illo eius adipisci eaque,
                necessitatibus consectetur, labore vero et ipsum.
                Officiis, ea vero. Praesentium, et. Enim, nostrum illo.
            </p>
        </section>

        <section id=”services”>
        	<h1> Services </h1>
            <p>
                Lorem ipsum dolor sit, amet consectetur adipisicing elit.
                Vero, nam! Iure officia aut esse tempore accusantium
                explicabo? Corporis deleniti ipsa fuga quas aut neque
                dicta nostrum laboriosam, iusto ullam minima est porro,
                totam saepe. Facilis aliquid praesentium, voluptates rem
                quibusdam sequi numquam illo eius adipisci eaque,
                necessitatibus consectetur, labore vero et ipsum.
                Officiis, ea vero. Praesentium, et. Enim, nostrum illo.
            </p>
        </section>
    </>
    )
}

export default LandingPage 
```

现在你知道了！现在，您已经在一个简单的网页上实现了平滑滚动。

## 结论

React-router-hash-link 允许您在使用 react-router-dom 进行路由时无缝地利用平滑滚动。

它有很多功能，包括滚动到顶部和滚动偏移功能，所以你可以探索它还能做什么。