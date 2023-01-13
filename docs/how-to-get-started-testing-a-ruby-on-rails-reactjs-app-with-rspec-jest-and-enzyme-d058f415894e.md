# 如何开始:用 RSpec、Jest 和 Enzyme 测试 Ruby-on-Rails / ReactJS 应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-get-started-testing-a-ruby-on-rails-reactjs-app-with-rspec-jest-and-enzyme-d058f415894e/>

我最近用 ReactJS、Ruby-on-Rails 和 PostgreSQL 做了一个简单的 ideas board web 应用。下面，我将带你经历我为我的应用程序的一个特性设置基本单元测试的最初步骤，添加一个新的想法。

![yToemQSqCMdhO5ADFrrEyf8WwRNFKWl2Zq69](img/17eb94a0ea07dcf6c7034ee3be5d5399.png)

Photo by [Dan DeAlmeida](https://unsplash.com/photos/awU3XEzdU94?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/ideas?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

注意:我不打算在这篇文章中讨论测试的*范围*；相反，我的重点是理解如何安装一些*依赖项*，以便能够开始编写测试。

### 背景:成立创意委员会

我使用节点包管理器来管理我的项目的依赖项。如果您想继续编写代码，您需要确保您的计算机上安装了 Node。

*项目特点*

**在铁轨上**

模型:创意

关系:无

**在作出反应**

组件:导航栏、Idea 容器、Idea

### 【RSpec 入门

我使用 RSpec 测试了我的 ideas board web 应用程序的 Rails 部分。要开始使用:

1.  我将宝石“rspec-rails”和“~> 3.8”添加到我的 gem 文件中。
2.  然后我在我的终端窗口中运行`bundle`(确保我在 Rails 目录中)。

3.接下来，在我的 Rails 目录中，我创建了一个名为`spec`的新文件夹。然后，在那个名为`requests`的文件夹中有另一个新文件夹。

4.我创建了一个名为`ideas_spec.rb`的新文件。您可以将名称`ideas_spec`替换为您想要测试的任何模型的名称，确保包含`_spec`部分以表示它是一个测试文件。

5.在我的 ideas_spec.rb 文件的顶部，我添加了以下文本:

`require ‘rails_helper’`

6.然后，在同一个文件中，我包含了描述我想要运行的第一个测试的代码:

```
describe “add an idea”, :type => :request dodescribe “add an idea”, :type => :request do
before do
 post ‘/api/v1/ideas’
end
it ‘returns a created status’ do
  expect(response).to have_http_status(201)
end
end
```

7.为了运行我的测试，我在我的终端窗口中键入了`rspec`，确保我在我的 rails 项目目录中。

如果您一直在跟进，RSpec 应该在此时运行，并且您的第一个测试应该通过！

### 【Jest 入门

我惊喜地发现测试框架 Jest 包含在 create-react-app 中！然而，我还想使用 Enzyme，一个测试工具，为此我需要安装一些依赖项。

1.  首先，我在应用程序的`src`目录中创建了一个名为 `_tests_`的新文件夹。
2.  在我的客户端目录中，我运行了以下命令:

```
npm i -D react-test-renderer
```

```
npm install --save-dev jest-enzyme
```

```
npm install --save-dev enzyme enzyme-adapter-react-16
```

3.为了配置 Enzyme，我在`src`中创建了一个名为`setupTests.js`的文件，包含以下内容:

```
const Enzyme = require('enzyme');
const EnzymeAdapter = require('enzyme-adapter-react-16');
Enzyme.configure({ adapter: new EnzymeAdapter() });
```

4.在我的`_tests_`文件夹中，我创建了一个名为`App.tests.js`的新文件

5.我在该文件的顶部添加了以下文本，以导入我的组件和所有测试功能，这些功能是我在所有测试中需要的:

```
import React from 'react';
import App from '../App';
import Idea from '../components/Idea';
import IdeasContainer from '../components/IdeasContainer';
import { create, update } from 'react-test-renderer';
import '../setupTests';
import { shallow, mount, render } from 'enzyme';
import { shallowToJson } from 'enzyme-to-json';
```

6.在下面，我包含了我的第一个*单元测试:*

```
describe('Idea', () => {
  it('should render a new idea correctly', () => {
    const output = shallow(
      <Idea
      key="mockKey"
      idea={"1", "Test", "Test"}
      onClick={"mockFn"}
      delete={"mockFn2"}
      />
    );
    expect(shallowToJson(output)).toMatchSnapshot();
  });
});
```

7.我在服务器端目录中运行`rails s`,然后在客户端目录中运行`npm start`,在 localhost:3001 上启动我的 ideas board。

8.为了运行我的第一个测试，我在终端窗口中输入了以下内容(确保我在客户端目录中):

```
npm run test
```

如果您一直在跟进，Jest 应该会运行到这一步，您的测试应该会通过——并且您现在处于编写更多测试的最佳位置！

关于 ideas board 项目的更多信息，我的回购可以在[这里](https://github.com/atkinsonholly/tracr)找到。

如果你做到了这一步，感谢你的阅读！我希望我的文章能帮助你开始设置你的测试。