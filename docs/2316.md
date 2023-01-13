# 如何将路由的 React 应用程序部署到 GitHub 页面

> 原文：<https://www.freecodecamp.org/news/deploy-a-react-app-to-github-pages/>

当我们建立项目时，我们希望在网上展示它们。与其购买域名并花时间配置它，不如使用 [GitHub Pages](https://pages.github.com/) 托管它更容易。

一个只使用 JavaScript、HTML 和 CSS 的项目很容易在 GitHub 页面上托管。不过，在 React、Vue 或 Angular 中构建的项目需要一些配置。这使得任何在线访问您的应用程序的人都能获得与您在本地构建应用程序时相同的体验。

在本文中，我将向您展示如何创建一个使用路由的简单 React 应用程序，然后我们将学习如何将它上传到 GitHub 页面。我们将特别关注路由部分，因为理解和实现它非常重要。

> ⚠️:本文假设你对 React 和 Git 有所了解。

### 先决条件

你需要在你的机器上安装 Node，yarn 和 npm。要检查它们是否已安装，请打开终端窗口并键入以下内容:

```
npm -v
yarn -v
node -v
```

如果这些命令在终端中打印出一个版本号，就可以了。如果没有，您需要继续安装缺少的部分。

*   [节点](https://nodejs.org/en/download/)(包含 npm)
*   [纱线](https://classic.yarnpkg.com/en/docs/install/#windows-stable)

我们还需要在 GitHub 上创建一个存储库。转到您的帐户，创建一个新的存储库。选择您认为适合这个项目的任何名称，但是对于本文的其余部分，我将使用**starter-project****。**

**为了创建我们的项目，我们将使用 **create-react-app** 。这是一个可以让你轻松创建单页应用程序的包。要创建项目，您需要在终端中键入以下内容:**

```
`npx create-react-app starter-project`
```

**一旦操作完成，您将有一个样板 React 项目，随时可以运行。要查看它是否正常工作，请进入项目目录(在我们的示例中是 starter-project)并运行命令:**

```
`yarn start`
```

**如果一切运行正常，您将在终端中看到一条消息，说明您的应用程序正在本地服务器上运行，地址是: **http://localhost:3000****

**如果你打开浏览器，你会看到这个:**

**![IB8uRE3cjN](img/33f0142bb04763ef88d2a29a956d5e72.png)**

## **如何将您的项目部署到 GitHub**

**您可能已经注意到，我们没有在 GitHub 中创建任何存储库。所以在我们继续之前，我们必须把我们的项目上传到那里。转到您的 GitHub 帐户，创建一个与 React 项目同名的存储库。**

> **☝️确保将你的库标记为公共的。如果您将其标记为私有，您将无法使用 GitHub 页面。**

**我们将把这个存储库作为远程设备添加到我们的项目中。为此，请在终端中键入:**

```
`git remote add <name-of-remote> <url-of-repository>`
```

**因此，在我们的例子中，命令看起来像这样:**

```
`git remote add origin https://github.com/TomerPacific/starter-project`
```

> **调用远程 ***原点*** 很重要，因为它将在我们的部署过程中使用。**

**执行完上面的命令后，我们还不能推送我们的代码。首先，我们需要配置一个上游分支，并将远程设置为起点。**

```
 `git push --set-upstream origin master`
```

**现在，我们可以将所有项目文件推送到我们的存储库中。**

**为了能够将我们构建的应用程序上传到 GitHub Pages，我们首先需要安装 [gh-pages 包](https://www.npmjs.com/package/gh-pages)。**

```
`yarn add gh-pages`
```

**这个包将帮助我们将代码部署到 gh-pages 分支，该分支将用于在 GitHub 页面上托管我们的应用程序。**

**为了让我们能够正确地使用 gh-pages 包，我们需要在 package.json 文件中为我们的脚本值添加两个键:**

```
`"scripts": {
    "start": "react-scripts start",
    "predeploy": "npm run build", <----------- #1
    "deploy": "gh-pages -d build", <---------- #2
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },`
```

**接下来，我们需要通过添加**主页**字段来修改我们的 package.json 文件。React 使用这个字段来计算构建的 HTML 文件中的根 URL。在其中，我们将放置我们的 GitHub 库的 URL。**

```
`{
  "name": "starter-project",
  "homepage": "https://tomerpacific.github.io/starter-project/", <----
  "version": "0.1.0",
  /....
}`
```

**要部署我们的应用程序，请在终端中键入以下内容:**

```
`npm run deploy`
```

**运行上面的命令负责构建您的应用程序，并将其推送到一个名为 gh-pages 的分支，GitHub 用它来链接 GitHub 页面。**

> **🚧如果您没有将您的 remote ***命名为 origin*** ，您将在此阶段得到一个错误，指出:**未能获得 remote.origin.url(任务必须在配置了 origin remote 的 git 存储库中运行，或者必须配置了“repo”选项)**。**

**如果在这个过程的最后你看到单词 **Published** ，你就会知道这个过程是成功的。我们现在可以进入设置下的 GitHub 库，向下滚动到 GitHub 页面部分。**

**![chrome_egdTtIso1X](img/b5366648b9b4375a05963a4fe6292c5a.png)**

**如果您看到类似上面的消息，这意味着您的应用程序现在成功地托管在 GitHub 页面上。**

**![image-308](img/a21f4aff7fed5282d560e0054eaeb393.png)

Photo by [Noah Glynn](https://unsplash.com/@noahglynn?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)** 

## **React 中的路由**

**到目前为止，一切顺利:**

1.  **我们有一个托管在 GitHub 页面上的基本 React 应用程序**
2.  **当我们想要进行更改时，我们也有一个简化的流程来部署它**

**但是由于本文的目的是展示一个比我们最初创建的应用程序更复杂的应用程序，所以我们将讨论路由。**

**我们的应用程序缺少的一个组件是导航。我们的应用程序不会只有一页，它可能会有很多页。那么，用户将如何在它们之间导航呢？**

**路由是为网络中的流量选择路径的实践。或者用更基本的术语来说，当你点击网页内部的链接时会发生什么，你会被重定向到哪里。**

**React 是一个库，它不包含您的应用程序开箱即用所需的一切(在我们的例子中是路由)。因此，我们将需要安装[反应路由器](https://reactrouter.com/web/guides/quick-start)。**

**React router 为 web 应用程序和本地应用程序提供了不同的组件。因为我们正在构建一个 web 应用程序，所以我们将使用 **react-router-dom** 。**

```
`yarn add react-router-dom`
```

**为了在我们的应用程序中使用路由，让我们创建一个导航元素，它将在应用程序的顶部可见。我们将把它添加到 App.js 文件中，并替换那里当前的 HTML 标记。**

```
 `<div>
     <nav>
         <ul id="navigation">
             <li>
                 <Link to="/">Home</Link>
             </li>
             <li>
                 <Link to="/about">About</Link>
             </li>
             <li>
                 <Link to="/contact">Contact</Link>
             </li>
         </ul>
     </nav>
</div>`
```

**通常，在一个非 React 项目中，我们会为每个部分放置一个到 HTML 页面的相对路径。这样，浏览器知道从哪里加载数据。**

**但是在我们的项目中，每个部分不会有不同的 HTML 页面。我们将加载一个不同的组件。以前在 App.js 中的标记现在可以在一个名为 Home 的组件中找到。**

```
`import './App.css';
import React from 'react';
import logo from './logo.svg';

class Home extends React.Component {
    render() {
        return (
            <div className="App">
                <header className="App-header">
                <img src={logo} className="App-logo" alt="logo" />
                <p>
                    Edit <code>src/App.js</code> and save to reload.
                </p>
                <a
                    className="App-link"
                    href="https://reactjs.org"
                    target="_blank"
                    rel="noopener noreferrer"
                >
                    Learn React
                </a>
                </header>
            </div>

        );
    }

}

export default Home;`
```

**Home.jsx**

**由于我们已经在导航中创建了三个部分，并处理了 home 部分，让我们再举一个关于部分的例子。**

**我们将创建一个名为 **About.jsx** 的新文件，它将保存我们的模板和 About 部分的代码。**

```
`import React from 'react';

const divStyle = {
    color:'white'
};

class About extends React.Component {

    render() {
        return (
            <div style={divStyle}>
                <h2>About Page</h2>
                <main>
                    <p>This section contains information about...</p>
                </main>
            </div>
        )
    }
}

export default About;`
```

**你可能会问自己，一旦用户点击 about 链接，应用程序如何知道重定向用户？为此，我们将使用一个名为 **Route** 的组件。**

**路由是 react-router 中最重要的组件之一，因为它允许您基于 URL 的路径呈现不同的组件。对于我们的项目，我们将使用 App.js 中导航标记下面的代码。**

```
`<Switch>
    <Route exact path="/">
    <Home />
    </Route>
    <Route path="/about">
    <About />
    </Route>
</Switch>`
```

**您可以看到，我们为 home 和 about 创建了两条路线。开关组件允许我们将路由组件组合在一起，它将只匹配其中一个。**

**我们合并的 App.js 文件如下所示:**

```
`import './App.css';
import React from 'react';
import { Route, Switch, Link } from "react-router-dom";
import About from './About';
import Home from './Home';

class App extends React.Component {
  render() {
      return (
        <div className="App">
          <div>
            <nav>
              <ul id="navigation">
                <li>
                  <Link to="/">Home</Link>
                </li>
                <li>
                <Link to="/about">About</Link>
                </li>
                <li>
                <Link to="/contact">Contact</Link>
                </li>
              </ul>
            </nav>
          </div>
            <Switch>
            <Route exact path="/">
              <Home />
            </Route>
            <Route path="/about">
              <About />
            </Route>
          </Switch>
          </div>
            );
  }
}

export default App;` 
```

**我们应该做的最后一件事是将我们的整个项目包装在路由器组件中。我们需要这样做，因为它使我们能够在应用程序中使用路由。我们将使用 BrowserRouter 组件，因为它使用 HTML5 的历史 API。**

```
`ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById('root')
);`
```

**index.js**

**如果我们在本地运行，一切似乎都正常。让我们将我们的增强项目部署到 GitHub 页面，看看结果如何。**

## **如何使用 HashRouter 处理路由**

**乍一看，一切似乎都很好。但是当你尝试刷新页面或浏览浏览器本身时，你会不断得到 404 错误。**

**为什么会这样？因为 GitHub Pages 不像你的浏览器那样支持浏览器历史。在我们的例子中，路由**https://tomerpacific.github.io/starter-project/about**并不能帮助 GitHub 页面理解将用户指向哪里(因为它是一个前端路由)。**

**为了克服这个问题，我们需要在我们的应用程序中使用散列路由器而不是浏览器路由器。这种类型的路由器使用 URL 的散列部分来保持 UI 与 URL 同步。**

```
`ReactDOM.render(
  <React.StrictMode>
    <HashRouter>
      <App />
    </HashRouter>
  </React.StrictMode>,
  document.getElementById('root')
);`
```

**index.js**

**你可以在这里阅读更多关于这个[的内容。](https://create-react-app.dev/docs/deployment/#github-pages-https-pagesgithubcom)**

**再次部署您的应用程序，您会对结果感到满意。不再有 404 错误。**

**这篇文章的灵感来自于我的一个项目。您可以在下面查看:**

**[](https://tomerpacific.github.io/julOnSaleReact/)[What’s On Sale @ Jul![favicon](img/0f7c30cf96936c72c2c2f1365b5fc879.png)](https://tomerpacific.github.io/julOnSaleReact/)**

**你可以在这里看到源代码:**

**[TomerPacific/julOnSaleReactSee what’s on sale @ Jul :school_satchel: :computer: :shower: :baby_bottle: :cocktail: :bread: :banana: :tomato: - TomerPacific/julOnSaleReact![favicon](img/0973ea8ce7121c320f68413e2a2f23ab.png)TomerPacificGitHub![23402988?s=400&v=4](img/9643c6145d37985e2afd64f06a742b7b.png)](https://github.com/TomerPacific/julOnSaleReact)**