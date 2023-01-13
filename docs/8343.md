# 反应模式:提取子组件以避免绑定

> 原文：<https://www.freecodecamp.org/news/react-pattern-extract-child-components-to-avoid-binding-e3ad8310725e/>

React 中有一个常见的场景:您正在映射一个数组，您需要每个项目调用一个点击处理程序并传递一些相关数据。

这里有一个例子。我正在遍历一个用户列表，并将要删除的 userId 传递给第 31 行的 deleteUser 函数。

```
import React from 'react';

class App extends React.Component {
  constructor() {
    this.state = {
      users: [
        { id: 1, name: 'Cory' }, 
        { id: 2, name: 'Meg' }
      ]
    };
  }

  deleteUser = id => {
    this.setState(prevState => {
      return { users: prevState.users.filter( user => user.id !== id)}
    })
  }

  render() {
    return (
      <div>
        <h1>Users</h1>
        <ul>
        { 
          this.state.users.map( user => {
            return (
              <li key={user.id}>
                <input 
                  type="button" 
                  value="Delete" 
                  onClick={() => this.deleteUser(user.id)} 
                /> 
                {user.name}
              </li>
            )
          })
        }
        </ul>
      </div>
    );
  }
}

export default App;
```

这里有一个关于 Codesandbox 的[工作示例。(哪个牛逼？)](https://codesandbox.io/s/0OP2Yq87)

### 那么问题出在哪里？

我在点击处理程序中使用了一个箭头函数。这意味着**每次渲染运行时，都会分配一个新的函数**。在许多情况下，这没什么大不了的。但是如果你有子组件，即使数据没有改变，它们也会被重新渲染，因为每次渲染都会分配一个新的函数。

**底线**:为了获得最佳性能，避免在渲染中声明箭头函数或绑定。我的团队使用[这个 ESLint 规则](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)来帮助提醒我们这个问题。

### 有什么解决办法？

那么在 render 中如何避免绑定和箭头函数呢？一种选择是提取一个子组件。在这里，我将列表项提取到 UserListItem.js:

```
import React from 'react';
import PropTypes from 'prop-types';

class UserListItem extends React.Component {
  onDeleteClick = () => {
    // No bind needed since we can compose 
    // the relevant data for this item here
    this.props.onClick(this.props.user.id);
  }

  // No arrow func in render! ?
  render() {
    return (
      <li>
        <input 
          type="button" 
          value="Delete" 
          onClick={this.onDeleteClick} 
        /> 
        {this.props.user.name}
      </li>
    );
  }
}

UserListItem.propTypes = {
  user: PropTypes.object.isRequired,
  onClick: PropTypes.func.isRequired
};

export default UserListItem;
```

然后，父组件的渲染变得更简单，不再需要包含箭头功能。它通过新的“renderUserListItem”函数中的 props 向下传递每个列表项的相关上下文。

```
import React from 'react';
import { render } from 'react-dom';
import UserListItem from './UserListItem';

class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      users: [{ id: 1, name: 'Cory' }, { id: 2, name: 'Sherry' }],
    };
  }

  deleteUser = id => {
    this.setState(prevState => {
      return { users: prevState.users.filter(user => user.id !== id) };
    });
  };

  renderUserListItem = user =>
    <UserListItem key={user.id} user={user} onClick={this.deleteUser} />;

  render() {
    return (
      <div>
        <h1>Users</h1>
        <ul>
          {this.state.users.map(this.renderUserListItem)}
        </ul>
      </div>
    );
  }
}

render(<App />, document.getElementById('root'));
```

请注意，我们在第 19 行调用了一个在 render 之外声明的新函数，而不是在 render 中使用 arrow 函数。每次渲染不再有函数分配。？

这里有一个最终重构的[工作示例。](https://codesandbox.io/s/jqQ0AlQlW)

### 耶还是恶心？

这种模式通过消除冗余的函数分配来提高性能。因此，当这种情况适用于您的组件时，它是最有用的:

1.  经常调用 Render。
2.  渲染孩子成本高。

不可否认，正如我在上面建议的，提取一个子组件也是额外的工作。它需要更多的活动部件和更多的代码。所以，如果你没有性能问题，这可以说是一个不成熟的优化？。

因此，您有两个选择:要么允许到处使用箭头和绑定(如果出现性能问题，就处理它们)，要么为了优化性能和一致性而禁止它们。

**底线:**我建议在渲染中禁用箭头和绑定。原因如下:

1.  你必须禁用我上面建议的[有用的 ESLint 规则](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)来允许它。
2.  一旦你禁用了林挺规则，人们很可能会复制这种模式，并开始禁用其他林挺规则。一个地方的例外会很快变成常态…

> 代码评审的通用规则:
> 
> 每一行代码都要值得复制。
> 
> 因为人会。[#清洁码](https://twitter.com/hashtag/cleancode?src=hash&ref_src=twsrc%5Etfw)
> 
> — Cory House (@housecor) [March 8, 2017](https://twitter.com/housecor/status/839511073279598594?ref_src=twsrc%5Etfw)