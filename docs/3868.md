# 反应原生样式:样式组件、Flexbox 布局等等

> 原文：<https://www.freecodecamp.org/news/react-native-styling-styled-components-flexbox-layouts-and-more/>

React Native 提供了一个用于创建样式表和样式化组件的 API:[样式表](https://facebook.github.io/react-native/docs/stylesheet)。

```
import React, { Component } from 'react';
import { StyleSheet, View, Text } from 'react-native';

export default class App extends Component {
  render () {
    return (
      <View>
        <Text style={styles.header}>I am a header!</Text>
        <Text style={styles.text}>I am some blue text.</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  header: {
    fontSize: 20
  },
  text: {
    color: 'blue'
  }
});
```

虽然常规 CSS 样式表无效，但 React Native 的 CSS 超集与传统 CSS 非常相似。

许多 CSS 属性(例如`color`、`height`、`top`、`right`、`bottom`、`left`)在样式表中是相同的。任何带有连字符的 CSS 属性(如`font-size`、`background-color`)都必须更改为驼峰文字(如`fontSize`、`backgroundColor`)。

并非所有 CSS 属性都存在于样式表中。由于在移动设备上没有真正的悬停概念，所以在 React Native 中不存在 CSS 悬停属性。相反，React Native 提供了响应触摸事件的[可触摸组件](https://facebook.github.io/react-native/docs/handling-touches#touchables)。

样式也不会像在传统 CSS 中那样被继承。在大多数情况下，您必须声明每个组件的样式。

## Flexbox 布局

React Native 使用类似于 web 标准的 [flexbox](https://facebook.github.io/react-native/docs/flexbox) 的实现。默认情况下，视图中的项目将被设置为`display: flex`。

如果您不想使用 flexbox，您也可以通过`relative`或`absolute`定位来安排 React 本地组件。

React Native 中的 Flexbox 默认为`flexDirection: column`，而不是`flex-direction: row` (web 标准)。`column`值垂直显示灵活的项目，可容纳纵向的移动设备。

要了解更多关于 flexbox 的信息，请访问[这份关于 CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) 的详细指南，以及与 [Flexbox Froggy](http://flexboxfroggy.com/) 的游戏化学习方法。

## 样式组件

在一个组件文件中包含大量样式并不总是容易维护的。样式化组件可以解决这个问题。

例如，一个按钮组件可以在应用程序的多个地方使用。复制和粘贴每个按钮实例的样式对象是低效的。相反，创建一个可重用的、有风格的按钮组件:

```
import React from 'react';
import { Text, TouchableOpacity } from 'react-native';

const Button = ({ onPress, children }) => {
  const { buttonStyle, textStyle } = styles;
  return (
    <TouchableOpacity onPress={onPress} style={buttonStyle}>
      <Text style={textStyle}>
        {children}
      </Text>
    </TouchableOpacity>
  );
};

export default Button;

const styles = {
  textStyle: {
    alignSelf: 'center',
    color: '#336633',
    fontSize: 16,
    fontWeight: '600',
    paddingTop: 10,
    paddingBottom: 10
  },
  buttonStyle: {
    backgroundColor: '#fff',
    borderWidth: 1,
    borderColor: '#336633',
    paddingTop: 4,
    paddingBottom: 4,
    paddingRight: 25,
    paddingLeft: 25,
    marginTop: 10,
    width: 300
  }
};
```

样式化的按钮组件可以很容易地导入并在应用程序中使用，而无需重复声明样式对象:

```
import React, { Component } from 'react';
import { TextInput, View } from 'react-native';
import Button from './styling/Button';

export default class Login extends Component {
  render() {
    return (
        <View>
          <TextInput placeholder='Username or Email' />
          <TextInput placeholder='Password' />
          <Button>Log In</Button>
        </View>
    );
  }
}
```

## 样式库

有一些流行的库用于设计 React Native 的样式。其中一些提供了类似于 [Bootstrap](https://guide.freecodecamp.org/bootstrap/index.md) 的特性，包括默认表单、按钮样式和页面布局选项。

最流行的库之一是[风格化组件](https://github.com/styled-components/styled-components)。你可以在 npm 和 GitHub 上找到许多其他的方法来自己尝试。