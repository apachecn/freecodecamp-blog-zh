# 如何编写可测试的代码| Khalil 的方法论

> 原文：<https://www.freecodecamp.org/news/how-to-write-testable-code/>

理解如何编写可测试的代码是我完成学业并开始第一份现实工作时遇到的最大挫折之一。

今天，当我在写 solidbook.io 中的一章时，我正在分解一些代码，找出所有的错误。我意识到几个原则支配着我如何编写可测试的代码。

在本文中，我想向您介绍一种简单明了的方法，您可以将这种方法应用于前端和后端代码，以了解如何编写可测试的代码。

## 必备读物

你可能想提前阅读以下文章。？

*   [依赖注入&反转解释| Node.js w/ TypeScript](https://khalilstemmler.com/articles/tutorials/dependency-injection-inversion-explained/)
*   [依赖规则](https://khalilstemmler.com/wiki/dependency-rule/)
*   [稳定依赖原则——SDP](https://khalilstemmler.com/wiki/stable-dependency-principle/)

## 依赖是关系

你可能已经知道了这一点，但是首先要明白的是，当我们从一个类(让我们称之为*源类*)中导入或者*甚至提到*另一个类、函数或者变量的名字时，无论提到什么都会变成对源类的依赖。

在[依赖倒置&注入文章](https://khalilstemmler.com/articles/tutorials/dependency-injection-inversion-explained/)中，我们看了一个`UserController`需要访问`UserRepo`才能**获得所有用户**的例子。

```
// controllers/userController.ts

import { UserRepo } from '../repos' // Bad

/**
 * @class UserController
 * @desc Responsible for handling API requests for the
 * /user route.
 **/

class UserController {
  private userRepo: UserRepo;

  constructor () {
    this.userRepo = new UserRepo(); // Also bad.
  }

  async handleGetUsers (req, res): Promise<void> {
    const users = await this.userRepo.getUsers();
    return res.status(200).json({ users });
  }
} 
```

这种方法的问题是，当我们这样做时，我们创建了一个硬的**源代码依赖**。

这种关系如下所示:

![before-dependency-inversion](img/76c7aecb90fc9fc656dc4500e6dba19e.png)

用户控制器直接依赖于用户报告。

这意味着如果我们想要测试`UserController`，我们也需要带上`UserRepo`。关于`UserRepo`的事情是，它也带来了一个该死的数据库连接。这可不好。

如果我们需要启动一个数据库来运行单元测试，那会使我们所有的单元测试变慢。

最终，我们可以通过使用**依赖倒置**来解决这个问题，在两个依赖之间放置一个抽象。

> 能够反转依赖流的抽象是*接口*或*抽象类*。

![after-dependency-inversion](img/2a6b46393117496e173df1e876cbb777.png)

*使用接口实现依赖倒置。*

这是通过在您想要导入的依赖项和源类之间放置一个抽象(接口或抽象类)来实现的。源类导入抽象，并保持可测试性，因为我们可以传入任何符合抽象契约的东西，即使它是一个 T2 模仿对象。

```
// controllers/userController.ts

import { IUserRepo } from '../repos' // Good! Refering to the abstraction.

/**
 * @class UserController
 * @desc Responsible for handling API requests for the
 * /user route.
 **/

class UserController {
  private userRepo: IUserRepo; // abstraction here

  constructor (userRepo: IUserRepo) { // and here
    this.userRepo = userRepo;
  }

  async handleGetUsers (req, res): Promise<void> {
    const users = await this.userRepo.getUsers();
    return res.status(200).json({ users });
  }
} 
```

在我们使用`UserController`的场景中，它现在指的是一个`IUserRepo`接口(它不需要任何成本),而不是指潜在的沉重的`UserRepo`,无论它去哪里都带着一个 db 连接。

如果我们希望测试控制器，我们可以通过用我们的数据库支持的`UserRepo`代替*内存实现*来满足`UserController`对`IUserRepo`的需求。我们可以创建一个这样的:

```
class InMemoryMockUserRepo implements IUserRepo { 
  ... // implement methods and properties
} 
```

## 方法论

以下是我保持代码可测试性的思考过程。当你想创建一个类和另一个类的关系时，这一切就开始了。

> Start:您想要从另一个文件中导入或提及一个类名。

问:您关心将来能够针对*源类*编写测试吗？

如果**否**，继续导入，因为这无关紧要。

如果**是**，考虑以下限制。只有在*以下至少一个*的情况下，你才可以依赖这个职业:

*   依赖是一种抽象(接口或抽象类)。
*   依赖性来自同一层或内层(参见[依赖性规则](https://khalilstemmler.com/wiki/dependency-rule/))。
*   是一个[稳定依赖](https://khalilstemmler.com/wiki/stable-dependency-principle/)。

> 如果在*这些条件中至少有一个*通过，则导入依赖关系——否则，不要导入。

导入依赖项可能会导致将来很难测试源组件。

同样，您可以通过使用[依赖反转](https://khalilstemmler.com/articles/tutorials/dependency-injection-inversion-explained/)来修复依赖违反其中一个规则的场景。

## 前端示例(使用类型脚本进行反应)

前端开发呢？

同样的规则也适用！

以这个 React 组件(预挂钩)为例，它包含一个依赖于一个`ProfileService`(外层- infra)的*容器组件*(内层关注)。

```
// containers/ProfileContainer.tsx

import * as React from 'react'
import { ProfileService } from './services'; // hard source-code dependency
import { IProfileData } from './models'      // stable dependency

interface ProfileContainerProps {}

interface ProfileContainerState {
  profileData: IProfileData | {};
}

export class ProfileContainer extends React.Component<
  ProfileContainerProps, 
  ProfileContainerState
> {

  private profileService: ProfileService;

  constructor (props: ProfileContainerProps) {
    super(props);
    this.state = {
      profileData: {}
    }
    this.profileService = new ProfileService(); // Bad.
  }

  async componentDidMount () {
    try {
      const profileData: IProfileData = await this.profileService.getProfile();

      this.setState({
        ...this.state,
        profileData
      })
    } catch (err) {
      alert("Ooops")
    }
  }

  render () {
    return (
      <div>Im a profile container</div>
    )
  }
} 
```

如果`ProfileService`对 RESTful API 进行网络调用，我们就没有办法测试`ProfileContainer`并阻止它进行真正的 API 调用。

我们可以通过做两件事来解决这个问题:

### 1.在`ProfileService`和`ProfileContainer`之间放置一个接口

首先，我们创建抽象，然后确保`ProfileService`实现它。

```
// services/index.tsx
import { IProfileData } from "../models";

// Create an abstraction
export interface IProfileService { 
  getProfile: () => Promise<IProfileData>;
}

// Implement the abstraction
export class ProfileService implements IProfileService {
  async getProfile(): Promise<IProfileData> {
    ...
  }
} 
```

接口形式的 ProfileService 的抽象。

然后我们更新`ProfileContainer`来依赖抽象。

```
// containers/ProfileContainer.tsx
import * as React from 'react'
import { 
  ProfileService, 
  IProfileService 
} from './services'; // import interface
import { IProfileData } from './models' 

interface ProfileContainerProps {}

interface ProfileContainerState {
  profileData: IProfileData | {};
}

export class ProfileContainer extends React.Component<
  ProfileContainerProps, 
  ProfileContainerState
> {

  private profileService: IProfileService;

  constructor (props: ProfileContainerProps) {
    super(props);
    this.state = {
      profileData: {}
    }
    this.profileService = new ProfileService(); // Still bad though
  }

  async componentDidMount () {
    try {
      const profileData: IProfileData = await this.profileService.getProfile();

      this.setState({
        ...this.state,
        profileData
      })
    } catch (err) {
      alert("Ooops")
    }
  }

  render () {
    return (
      <div>Im a profile container</div>
    )
  }
} 
```

### 2.用包含有效的`IProfileService`的 HOC 组成一个`ProfileContainer`。

现在我们可以创建使用我们想要的任何类型`IProfileService`的 hoc。它可能是连接到 API 的那个，如下所示:

```
// hocs/withProfileService.tsx

import React from "react";
import { ProfileService } from "../services";

interface withProfileServiceProps {}

function withProfileService(WrappedComponent: any) {
  class HOC extends React.Component<withProfileServiceProps, any> {
    private profileService: ProfileService;

    constructor(props: withProfileServiceProps) {
      super(props);
      this.profileService = new ProfileService();
    }

    render() {
      return (
        <WrappedComponent
          profileService={this.profileService}
          {...this.props}
        />
      );
    }
  }
  return HOC;
}

export default withProfileService; 
```

或者它也可以是一个使用内存中配置文件服务的模拟程序。

```
// hocs/withMockProfileService.tsx

import * as React from "react";
import { MockProfileService } from "../services";

interface withProfileServiceProps {}

function withProfileService(WrappedComponent: any) {
  class HOC extends React.Component<withProfileServiceProps, any> {
    private profileService: MockProfileService;

    constructor(props: withProfileServiceProps) {
      super(props);
      this.profileService = new MockProfileService();
    }

    render() {
      return (
        <WrappedComponent
          profileService={this.profileService}
          {...this.props}
        />
      );
    }
  }
  return HOC;
}

export default withProfileService; 
```

对于我们的`ProfileContainer`来说，要利用来自一个特设的`IProfileService`，它必须期望在`ProfileContainer`中接收一个`IProfileService`作为道具，而不是作为属性添加到类中。

```
// containers/ProfileContainer.tsx

import * as React from "react";
import { IProfileService } from "./services";
import { IProfileData } from "./models";

interface ProfileContainerProps {
  profileService: IProfileService;
}

interface ProfileContainerState {
  profileData: IProfileData | {};
}

export class ProfileContainer extends React.Component<
  ProfileContainerProps,
  ProfileContainerState
> {
  constructor(props: ProfileContainerProps) {
    super(props);
    this.state = {
      profileData: {}
    };
  }

  async componentDidMount() {
    try {
      const profileData: IProfileData = await this.props.profileService.getProfile();

      this.setState({
        ...this.state,
        profileData
      });
    } catch (err) {
      alert("Ooops");
    }
  }

  render() {
    return <div>Im a profile container</div>
  }
} 
```

最后，我们可以用任何我们想要的 HOC 来组合我们的`ProfileContainer`——包含真实服务的那个，或者包含用于测试的虚假服务的那个。

```
import * as React from "react";
import { render } from "react-dom";
import withProfileService from "./hocs/withProfileService";
import withMockProfileService from "./hocs/withMockProfileService";
import { ProfileContainer } from "./containers/profileContainer";

// The real service
const ProfileContainerWithService = withProfileService(ProfileContainer);
// The mock service
const ProfileContainerWithMockService = withMockProfileService(ProfileContainer);

class App extends React.Component<{}, IState> {
  public render() {
    return (
      <div>
        <ProfileContainerWithService />
      </div>
    );
  }
}

render(<App />, document.getElementById("root")); 
```

* * *

我是哈利勒。我是一名开发者拥护者@ [Apollo GraphQL](https://www.apollographql.com/docs/?utm_source=khalil&utm_medium=khalil_post_footer&utm_campaign=how_to_write_testable_code) 。我还为有志于开发 Enterprise Node.js、领域驱动设计和编写可测试、灵活的 JavaScript 的人编写课程、书籍和文章。

这最初发布在我的博客@[khalilstemmler.com](https://khalilstemmler.com)上，出现在 [solidbook.io -软件设计介绍&架构 w/ Node.js &打字稿](https://solidbook.io)的第 11 章。

你可以在推特上问我任何问题！