# 原生反应–可触摸和屏幕尺寸

> 原文：<https://www.freecodecamp.org/news/react-native-touchables-and-screen-dimensions/>

React Native 使得在 Android 和 iOS 设备上开发应用程序的过程比以前容易得多。虽然在此之前，您必须使用至少两种编程语言和截然不同的 API，但 React Native 提供了一些现成的有用工具。

这里有两个纲要，可以帮助你开发下一个应用。

## 触地得分

移动设备的一些主要功能围绕着用户触摸交互。移动应用如何处理和响应这些交互可以决定用户体验的成败。

React Native ships 带有一个用于许多标准`onPress`交互的`Button`组件。默认情况下，它会通过改变不透明度来显示按钮被按下，从而给用户反馈。用法:

```
<Button onPress={handlePress} title="Submit" />
```

对于更复杂的用例，React Native 内置了 API 来处理名为`Touchables`的媒体交互。

```
TouchableHighlight
TouchableNativeFeedback
TouchableOpacity
TouchableWithoutFeedback
```

这些可触摸组件中的每一个都可以被设计风格并与一个库一起使用，比如内置的`Animated`，允许你制作自己类型的定制用户反馈。

使用这些组件的一些示例:

```
// with images
<TouchableHighlight onPress={this.handlePress}>
  <Image
    style={styles.button}
    source={require('./logo.png')}
  />
</TouchableHighlight>

// with text
<TouchableHighlight onPress={this.handlePress}>
  <Text>Hello</Text>
</TouchableHighlight>
```

您也可以处理不同类型的按钮按压。默认情况下，按钮和可触摸按钮被配置为处理常规点击，但您也可以指定一个函数来调用例如按住交互。

```
<TouchableHighlight onPress={this.handlePress} onLongPress={this.handleLongPress}>
```

要查看所有可用的道具以及这些组件是如何工作的，你可以在这里查看可触摸的 JavaScript 源代码。

## 屏幕尺寸

React Native 使用每英寸点数(DPI)来衡量用户界面(UI)和 UI 上显示的任何内容的大小。这种类型的度量允许应用程序在各种屏幕尺寸和像素密度下看起来一致。

对于标准用例，开发应用程序时无需了解用户设备的细节(例如，像素密度)，因为 UI 元素会自动缩放。

当需要时，有可用的 API 如`PixelRatio`来帮助你找出用户设备的像素密度。

为了获得用户设备的窗口或屏幕高度/宽度，React Native 有一个名为`Dimensions`的 API。

```
import { Dimensions } from 'react-native';
```

下面是`Dimensions` API 提供的方法:

```
Dimensions.get('window').height;
Dimensions.get('window').width;
Dimensions.get('screen').height;
Dimensions.get('screen').width;
```

****注意:**** 在过去，Dimensions API 存在一些已知问题，例如当用户旋转其设备时无法返回正确的信息。在部署应用程序之前，最好确保在实际设备上进行测试。

### 关于响应式设计的更多信息:

*   [免费响应式设计课程](https://www.freecodecamp.org/news/master-responsive-website-design/)
*   [响应式网页设计最佳自举教程](https://www.freecodecamp.org/news/p/f401cbed-6c27-46e5-a56f-c9853b87e244/freecodecamp.org/news/best-bootstrap-tutorial-responsive-web-design/)
*   [如何有针对性地思考](https://www.freecodecamp.org/news/how-to-start-thinking-responsively/)
*   [响应图像指南](https://www.freecodecamp.org/news/your-complete-guide-to-truly-responsive-images/)
*   [在 5 分钟内学会响应式设计](https://www.freecodecamp.org/news/learn-responsive-web-design-in-5-minutes/)