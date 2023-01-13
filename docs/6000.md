# 如何像老板一样建立 Jest & Enzyme

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-jest-enzyme-like-a-boss-8455a2bc6d56/>

![image-249](img/a2acd8953e3d6d4ce1a205e711868a3d.png)

Photo by [Quino Al](https://unsplash.com/@quinoal?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

当我开始为我的 React 应用程序编写测试时，我尝试了几次才弄明白如何使用`Jest` & `Enzyme`来设置我的测试环境。本教程假设您已经用`webpack` & `babel`设置了一个 React 应用程序。我们将从那里继续。

这是我写的一系列文章的一部分。我将讨论如何以正确和简单的方式为生产建立一个 React 应用程序。

*   **Part 1** [如何将 Webpack 4 和 Babel 7 结合起来创建一个奇妙的 React app](https://medium.freecodecamp.org/how-to-combine-webpack-4-and-babel-7-to-create-a-fantastic-react-app-845797e036ff) (谈到用 Babel 设置 Webpack，连同。scss 支持)
*   **第二部分** [这些工具将帮助你写干净的代码](https://medium.freecodecamp.org/these-tools-will-help-you-write-clean-code-da4b5401f68e)(谈论自动化你的代码，所以你写的所有代码都是好代码)
*   这是**第三部分**，我将在其中讲述如何用酶来创造笑话。

在我们开始之前，如果在任何时候你觉得卡住了，请随时检查 [**代码库**](https://github.com/adeelibr/react-starter-kit) 。如果你觉得事情可以改善，公关是最受欢迎的。

### 先决条件

您需要安装节点才能使用 npm(节点软件包管理器)。

首先，创建一个名为`app`的文件夹，然后打开你的终端，进入那个`app`文件夹，输入:

```
npm init -y
```

这将为您创建一个`package.json`文件。在您的`package.json`文件中添加以下内容:

```
{
  "name": "react-boiler-plate",
  "version": "1.0.0",
  "description": "A react boiler plate",
  "main": "src/index.js",
  "author": "Adeel Imran",
  "license": "MIT",
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage --colors",
  },
  "devDependencies": {
    "@babel/core": "^7.0.0",
    "@babel/polyfill": "^7.0.0-beta.51",
    "@babel/preset-env": "^7.0.0-beta.51",
    "@babel/preset-react": "^7.0.0-beta.51",
    "babel-core": "^7.0.0-bridge.0",
    "babel-jest": "^23.4.2",
    "enzyme": "^3.3.0",
    "enzyme-adapter-react-16": "^1.1.1",
    "jest": "^23.4.2"
  }
}
```

This is all your need, for setting up your testing environment. Promise :D

其次，在您的`app`文件夹中创建一个名为`src`的文件夹。`src/app/`文件夹是你所有的 React 代码及其测试将驻留的地方。但在此之前，让我们了解一下为什么我们在`package.json`文件中做了这些。

我一会儿会谈到`scripts`(承诺)。但是在此之前，让我们了解一下为什么我们需要下面的依赖项。我想让你知道你的`package.json`里面是什么，让我们开始吧。

因为我们通常使用 webpack 来编译 react 代码。Babel 是一个主要的依赖项，帮助告诉 webpack 如何编译代码。这也是使用 Jest 的对等依赖。

`@babel/polyfil` Jest 需要一个叫`regenerator-runtime`的东西，@babel/polyfill 自带了它和一些其他很酷的功能。

`@babel/preset-env` & `@babel/preset-react`是针对 ES6 和 React 这样的特性，所以在写单元测试的时候`Jest`知道 **ES6** 语法和 **JSX。**

这主要是对`Jest`的依赖，因为我们需要`babel-core`才能让 Jest 工作。

`babel-jest`将帮助巴别理解我们在`Jest`中写的代码

这是一个断言库，使得断言、操作和遍历 React 组件的输出变得更加容易。

`enzyme-adapter-react-16`帮助 Jest 与`enzyme`连接的适配器/中间件

Jest 是我们将在其上运行测试的测试库。

你可以看看由 **jest 的酷人们做的一个非常简单的例子。**它使用 babel 运行一个简单的测试 [**这里**](https://github.com/facebook/jest/tree/master/examples/babel-7) **。**

此外，如果你想为 React 设置 webpack，这是一个关于我如何做的详细演练。或者你可以简单地浏览整个代码库，它使用你在设置你的 React 应用程序和 jest/enzyme([**starter-kit 这里**](https://github.com/adeelibr/react-starter-kit) )时需要的基本框架结构。

接下来，让我们在主`app`文件夹中创建一个名为`jest.config.js`的文件，并向其中添加以下代码。我一会儿会谈到它的作用。

```
// For a detailed explanation regarding each configuration property, visit:
// https://jestjs.io/docs/en/configuration.html

module.exports = {
  // Automatically clear mock calls and instances between every test
  clearMocks: true,

  // An array of glob patterns indicating a set of files for which coverage information should be collected
  collectCoverageFrom: ['src/**/*.{js,jsx,mjs}'],

  // The directory where Jest should output its coverage files
  coverageDirectory: 'coverage',

  // An array of file extensions your modules use
  moduleFileExtensions: ['js', 'json', 'jsx'],

  // The paths to modules that run some code to configure or set up the testing environment before each test
  setupFiles: ['<rootDir>/enzyme.config.js'],

  // The test environment that will be used for testing
  testEnvironment: 'jsdom',

  // The glob patterns Jest uses to detect test files
  testMatch: ['**/__tests__/**/*.js?(x)', '**/?(*.)+(spec|test).js?(x)'],

  // An array of regexp pattern strings that are matched against all test paths, matched tests are skipped
  testPathIgnorePatterns: ['\\\\node_modules\\\\'],

  // This option sets the URL for the jsdom environment. It is reflected in properties such as location.href
  testURL: 'http://localhost',

  // An array of regexp pattern strings that are matched against all source file paths, matched files will skip transformation
  transformIgnorePatterns: ['<rootDir>/node_modules/'],

  // Indicates whether each individual test should be reported during the run
  verbose: false,
};
```

Jest config file ***app/jest.config.js***

其次，在主`app`文件夹中创建一个名为`enzyme.config.js`的文件，并向其中添加以下代码。

```
/** Used in jest.config.js */
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });
```

Enzyme config file ***app/enzyme.config.js***

先说一下`jest.config.js`

`clearMocks`将清除所有模拟，以便`nth` 测试中的模拟不会变异或影响`n+1`位置的测试。

告诉 jest 从所有的。js 文件放在`src/`文件夹中。覆盖率告诉你测试用例覆盖了多少百分比的代码。

`coverageDirectory`告诉 Jest 覆盖目录应该在主`app/`文件夹中命名为`coverage`。

接受一个扩展名数组，告诉 Jest 它可以测试哪些文件。我们告诉它要测试一切。js|。jsx|。json 文件。

这真的很重要，因为它告诉 Jest 它可以从哪里获得酶的配置(稍后会详细介绍)

指定 Jest 将在什么环境下运行测试，因为我们正在测试一个 web 应用程序。我已经将环境设置为`jsdom`

告诉 Jest 它将测试哪些文件。我在这里传入了两个配置，一个是测试任何名为`__tests__`的文件夹中的所有文件，或者测试所有以`spec.js|.jsx`或`test.js|.jsx`结尾的文件

我不希望 Jest 在我的`node_modules`文件夹中运行测试。所以我忽略了这些文件。

`testURL`该选项设置 jsdom 环境的 URL。它反映在 location.href 等属性中

与所有源文件路径匹配的 regexp 模式字符串数组，匹配的文件将跳过转换。这里我只给它一个`node_modules`

如果为真，当你运行测试时会给你一个非常详细的日志。我不想看到这些，只关注我测试的要点。所以我把它的值设为`false`

我们来谈谈`enzyme.config.js`

我在我的 Jest 配置中传递了路径`enzyme.config.js`。当它进入这个文件时，Jest 接受酶的配置。这意味着所有的测试都将在 Jest 上运行。但是断言和其他一切都将由酶来完成。

有了这些，我们的配置就完成了。我们来谈谈剧本:

```
"scripts": {    
    "test": "jest",
    "test:watch": "jest --watch",    
    "test:coverage": "jest --coverage --colors",  
},
```

这将运行 Jest 并执行所有测试

将运行所有的测试并保持观察模式，这样当我们对我们的测试用例做任何改变时，它将再次执行那些测试用例。

`npm run test:coverage`将基于它执行的所有测试生成一个覆盖率报告，并在`app/coverage`文件夹中给你一个详细的覆盖率报告。

在运行测试之前，我们需要创建一个。那我们开始吧。在你的`app/src/`文件夹中创建一个名为 **WelcomeMessage.js** 的文件。

```
import React, { Fragment } from 'react';

const styles = {
  heading: {
    color: '#fff',
    textAlign: 'center',
    marginBottom: 15,
  },
  logo: {
    width: 250,
    heading: 250,
    objectFit: 'cover',
  },
};

const WelcomeMessage = ({ imgPath }) => {
  return (
    <Fragment>
      <h1 style={styles.heading}>
        Welcome To
      </h1>
      <img src={imgPath} alt="app logo" style={styles.logo} />
    </Fragment>
  );
};

export default WelcomeMessage;
```

***app/src/WelcomeMessage.js***

在同一个文件夹中创建一个名为[**welcome message . test . js**](https://gist.github.com/adeelibr/ac60da132758c7ebbcb30e28672975fe)的文件

```
import React from 'react';
import { shallow } from ‘enzyme’;

// Components
import WelcomeMessage from './WelcomeMessage';

function setup() {
  const props = {
    imgPath: 'some/image/path/to/a/mock/image',
  };
  const wrapper = shallow(<WelcomeMessage />);
  return { wrapper, props };
}

describe('WelcomeMessage Test Suite', () => {
  it('Should have an image', () => {
    const { wrapper } = setup();
    expect(wrapper.find('img').exists()).toBe(true);
  });
});
```

***app/src/WelcomeMessage.***[***test***](https://gist.github.com/adeelibr/ac60da132758c7ebbcb30e28672975fe)***.js***

这里要注意的一点是，你将不能实际运行`WelcomMessage.js`文件，因为你没有用`babel`设置`webpack`。如果你正在寻找一种方法来设置它，看看我的教程[如何结合 Webpack 4 和 Babel 7 来创建一个奇妙的 React 应用](https://medium.freecodecamp.org/how-to-combine-webpack-4-and-babel-7-to-create-a-fantastic-react-app-845797e036ff)。另外，如果你只是想要本教程的源代码，这里有 [**代码库**](https://github.com/adeelibr/react-starter-kit) 。它已经有了笑话&酶设置。随意做一个分叉，开始玩代码库。

回到我们刚刚写的代码，在你的终端输入`npm run test`。它将执行一个脚本，找到所有以`*.test.js`结尾的文件并执行它们。执行之后，您将看到如下消息:

```
Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
```

现在我知道这不是一个很实用的单元测试，但我希望本教程专注于纯粹设置 Jest & Enzyme。

再次这里是这个 [**教程**](https://github.com/adeelibr/react-starter-kit) 的源代码。