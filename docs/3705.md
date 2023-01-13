# React Native - Basic 命令举例说明

> 原文：<https://www.freecodecamp.org/news/react-native-basic-commands-explained-with-examples/>

在这里，您将找到使用 React Native 开始开发 iOS 和 Android 应用程序的基本命令列表。如果您还没有安装它，强烈建议您遵循[官方指南](https://facebook.github.io/react-native/docs/getting-started.html)。

## 开始一个新项目

有不同的方法可以引导 react 本地应用程序。你可以使用 ****Expo**** 或`create-react-native-app`(反过来使用 Expo-Cli)来启动你的新项目，但通过这种方法，你可以更好地控制你的项目中发生的事情，并可以与 iOS 和 Android 移动平台的原生库进行交流，调整和编写自己的模块。

```
react-native init [PROJECT-NAME]
cd [PROJECT-NAME]
```

## 在 Android 模拟器中运行应用程序

这个命令是不言自明的，因为它说它将启动 Android 模拟器并安装您刚刚创建的应用程序。您需要在项目的根目录下运行此命令。

```
react-native run-android
```

## 在 iOS 模拟器中运行应用程序

这个命令和`react-native run-android`做的完全一样，但是它打开的不是 Android 模拟器，而是 iPhone 模拟器。

```
react-native run-ios
```

## 将依赖项链接到本机项目

一些库具有需要在为 React Native 生成的本机代码中链接的依赖项。如果在你安装了一个新的库之后有些东西不工作，可能是因为你跳过了这一步。

```
react-native link [LIBRARY-NAME]
```

## 清除捆绑包

如果有些东西没有按预期运行，可能需要用这个命令清除并创建一个新的包。

```
watchman watch-del-all
```

## 支持装饰者

JSX 默认不支持装饰者，所以你需要安装 ****巴别塔**** 插件才能让它工作。

```
npm install babel-plugin-transform-decorators-legacy --save
npm install babel-plugin-transform-class-properties --save
```

## 导出 APK 以在设备中运行

使用下面的命令，您将拥有一个未签名的 apk，这样您就可以安装并与您的同事共享以进行测试。请记住，这个 apk 不准备上传到应用商店或生产。你将在`android/app/build/outputs/apk/app-debug.apk`中找到你的新 apk。

### 1.捆绑调试版本

```
react-native bundle --dev false --platform android --entry-file index.android.js --bundle-output ./android/app/build/intermediates/assets/debug/index.android.bundle --assets-dest ./android/app/build/intermediates/res/merged/debug
```

### 2.创建调试版本

```
cd android
./gradlew assembleDebug
```

## **React Native 上的更多资源:**

*   [如何使用 React Native 构建移动应用](https://www.freecodecamp.org/news/what-you-need-to-know-to-start-building-mobile-apps-in-react-native-dded951277b7/)
*   [React Native 中的函数与类组件](https://www.freecodecamp.org/news/functional-vs-class-components-react-native/)
*   [如何用 Jest 和 Enzyme 测试 React 原生应用](https://www.freecodecamp.org/news/setting-up-jest-enzyme-for-testing-react-native-40393ca04145/)