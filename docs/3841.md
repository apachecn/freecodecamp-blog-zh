# React Native 中的函数组件与类组件

> 原文：<https://www.freecodecamp.org/news/functional-vs-class-components-react-native/>

在 React Native 中，组成应用程序的组件主要有两种类型:**功能组件**和**类组件**。它们的结构与普通的 React web 应用程序相同。

## 类别组件

类组件是 JavaScript ES2015 类，它们从 React 扩展了一个名为`Component`的基类。

```
class App extends Component {
    render () {
        return (
            <Text>Hello World!</Text>
        )
    }
}
```

这使得类`App`可以访问像`render`这样的 React 生命周期方法以及来自父类的状态/属性功能。

## 功能组件

功能组件更简单。他们不管理自己的状态，也不能访问 React Native 提供的生命周期方法。它们实际上是普通的旧 JavaScript 函数，有时被称为无状态组件。

```
const PageOne = () => {
    return (
        <h1>Page One</h1>
    );
}
```

## 摘要

类组件被用作容器组件来处理状态管理和包装子组件。

功能组件通常只是用于显示目的——这些组件从父组件调用函数来处理用户交互或状态更新。

## 关于组件状态的更多信息

### 组件状态

在`Class`组件中，有一种存储和管理内置状态的方式来对本机做出反应。

```
class App extends Component {
  constructor () {
    super();
    this.state = {
      counter: 0
    };
  }
  incrementCount () {
    this.setState({
      counter: this.state.counter + 1
    });
  }
  decrementCount () {
    this.setState({
      counter: this.state.counter - 1
    });
  }
  render () {
    return (
      <View>
        <Text>Count: {this.state.counter}</Text>
        <Button onPress={this.decrementCount.bind(this)}>-</Button>
        <Button onPress={this.incrementCount.bind(this)}>+</Button>
      </View>
    );
  }
}
```

状态类似于道具，但它是私有的，完全由组件控制。这里，`constructor()`方法正在调用父类的构造函数，其中`super();` - ****`Component`**** 是`App`的父类，因为我们使用了`extends`关键字。`constructor()`方法还初始化组件的状态对象:

```
this.state = {
  counter: 0
};
```

状态可以显示在组件中:

```
{this.state.counter}
```

或通过拨打以下电话进行更新:

```
this.setState({});
```

****注意:**** 除了在你的组件的`constructor()`方法中初始创建之外，你永远不要用`this.state =`直接修改组件的状态。你必须使用上面的`incrementCount`和`decrementCount`功能中的`this.setState`。

通过调用传递给`onPress`处理程序的函数来递增和递减计数，就像在 web 上从 JavaScript 调用 click 处理程序一样。

*旁白:在第一个例子中，`<Button>`是一个定制组件；它是 React 本地 API*中`<TouchableOpacity>`和`<Text>`的组合

```
const Button = ({ onPress, children, buttonProps, textProps }) => {
  const { buttonStyle, textStyle } = styles;
  return (
    <TouchableOpacity onPress={onPress} style={[buttonStyle, buttonProps]}>
      <Text style={[textStyle, textProps]}>
        {children}
      </Text>
    </TouchableOpacity>
  );
};
```

## 关于 React Native 的更多信息:

*   [反应原生向导](https://www.freecodecamp.org/news/react-native-guide/)