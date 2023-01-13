# 如何在 React Native 中构建项目和管理静态资源

> 原文：<https://www.freecodecamp.org/news/how-to-structure-your-project-and-manage-static-resources-in-react-native-6f4cfc947d92/>

作者 Khoa Pham

# 如何在 React Native 中构建项目和管理静态资源

![bJ4t6m7wdDX72qohmdL43MPFLox3ZH3djClN](img/c11164229d4ed35e5dd773588ef955b9.png)

Source: [stmed.net](https://stmed.net/wallpaper-176455)

React 和 React Native 只是框架，它们并没有规定我们应该如何构建我们的项目。这完全取决于你的个人品味和你正在做的项目。

在本帖中，我们将介绍如何构建项目以及如何管理本地资产。这当然不是一成不变的，你可以自由选择适合你的部分。希望你能学到一些东西。

对于用`[react-native init](https://medium.com/fantageek/what-is-create-react-native-app-9f3bc5a6c2a3)`启动的项目，我们只得到[的基本结构](https://medium.com/fantageek/what-is-create-react-native-app-9f3bc5a6c2a3)。

Xcode 项目有`ios`文件夹，Android 项目有`android`文件夹，React 原生起点有`index.js`和`App.js`文件。

```
ios/
android/
index.js
App.js
```

作为一个在 Windows Phone、iOS 和 Android 上都使用过 native 的人，我发现构建一个项目可以归结为通过类型或功能来分隔文件

### 类型与特征

按类型分类意味着我们根据文件的类型来组织文件。如果它是一个组件，有容器和表示文件。如果是 Redux，有 action，reducer，和 store 文件。如果是 view，有 JavaScript，HTML，CSS 文件。

#### 按类型分组

```
redux
  actions
  store
  reducers
components
  container
  presentational
view
  javascript
  html
  css
```

这样，我们可以看到每个文件的类型，并轻松地针对某个文件类型运行脚本。这对于所有项目都是通用的，但是它没有回答“这个项目是关于什么的”这个问题是新闻应用吗？是忠诚度 app 吗？是关于营养追踪的吗？

按类型组织文件是机器的事，不是人的事。很多时候，我们在一个特性上工作，在多个目录中查找要修复的文件是一件麻烦的事情。如果我们计划用我们的项目做一个框架，这也是一个痛苦，因为文件分散在许多地方。

#### 按功能分组

更合理的解决方案是按特征组织文件。与功能相关的文件应该放在一起。并且[测试文件](https://medium.com/@JeffLombardJr/organizing-tests-in-jest-17fc431ff850)应该靠近源文件。查看[这篇文章](https://medium.com/@JeffLombardJr/organizing-tests-in-jest-17fc431ff850)了解更多信息。

功能可以与登录、注册、入职或用户简档相关。一个特性可以包含子特性，只要它们属于同一个流。如果我们想移动子特征，这很容易，因为所有相关的文件都已经分组在一起了。

我基于特性的典型项目结构如下所示:

```
index.js
App.js
ios/
android/
src
  screens
    login
      LoginScreen.js
      LoginNavigator.js
    onboarding
      OnboardingNavigator    
      welcome 
        WelcomeScreen.js
      term
        TermScreen.js
      notification
        NotificationScreen.js
    main
      MainNavigator.js
      news
        NewsScreen.js
      profile
        ProfileScreen.js
      search
        SearchScreen.js
  library
    package.json
    components
      ImageButton.js
      RoundImage.js
    utils
      moveToBottom.js
      safeArea.js
    networking
      API.js
      Auth.js
  res
    package.json
    strings.js
    colors.js
    palette.js
    fonts.js
    images.js
    images
      logo@2x.png
      logo@3x.png
      button@2x.png
      button@3x.png
scripts
  images.js
  clear.js
```

除了传统的文件`App.js`和`index.js`以及`ios1`和`android`文件夹，我把所有的源文件都放在了`src`文件夹里。在`src`中，我有`res`用于资源，`library`用于跨特性使用的公共文件，`screens`用于内容屏幕。

#### 尽可能少的依赖

因为 React Native 严重依赖于大量的依赖项，所以在添加更多的依赖项时，我尽量保持警惕。在我的项目中，我只用`react-navigation`来导航。我也不喜欢`redux`，因为它增加了不必要的复杂性。只有在你真正需要的时候才添加依赖，否则你只会给自己带来更多的麻烦而不是价值。

我喜欢 React 的一点是它的组件。组件是我们定义视图、风格和行为的地方。React 有内联风格——就像用 JavaScript 定义脚本、HTML 和 CSS 一样。这符合我们的目标特征方法。这就是为什么我不使用[样式组件](https://github.com/styled-components/styled-components)。由于样式只是 JavaScript 对象，我们可以在`library`中共享注释样式。

### 科学研究委员会

我非常喜欢 Android，所以我命名为`src`和`res`来匹配它的文件夹惯例。

为我们建立了巴别塔。但是对于一个典型的 JavaScript 项目，在`src`文件夹中组织文件是很好的。在我的`electron.js`应用程序[图标生成器](https://github.com/onmyway133/IconGenerator/tree/master/src)中，我将源文件放在`src`文件夹中。这不仅有助于组织，也有助于 babel 一次性传输整个文件夹。只需一个命令，我就可以将`src`的文件瞬间传输到`dist`。

```
babel ./src --out-dir ./dist --copy-files
```

### 屏幕

React 是基于组件的。没错。有[容器和表示组件](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)，但是我们可以组合组件来构建更复杂的组件。它们通常以全屏显示结束。它在 Windows Phone 中被称为`Page`，在 iOS 中被称为`ViewController`，在 Android 中被称为`Activity`。React 原生指南经常提到[屏幕](https://facebook.github.io/react-native/docs/navigation)是覆盖整个空间的东西:

> 移动应用程序很少由单个屏幕组成。管理多个屏幕的呈现和在多个屏幕之间的转换通常由所谓的导航器来处理。

#### 到底是不是 index.js？

每个屏幕都被视为每个功能的入口点。您可以利用节点[模块](https://medium.freecodecamp.org/requiring-modules-in-node-js-everything-you-need-to-know-e7fbd119be8)的功能将`LoginScreen.js`重命名为`index.js`:

> 模块不一定是文件。我们还可以在`node_modules`下创建一个`find-me`文件夹，并在其中放置一个`index.js`文件。同一`require('find-me')`行将使用该文件夹的`index.js`文件

所以不用`import LoginScreen from './screens/LoginScreen'`，我们可以只做`import LoginScreen from './screens'`。

使用`index.js`导致封装，并为该特性提供了一个公共接口。这都是个人口味。我自己更喜欢对文件进行显式命名，因此命名为`LoginScreen.js`。

### 航海家

[react-导航](https://github.com/react-navigation/react-navigation)似乎是 React 原生应用中处理导航的最受欢迎的选择。对于像 onboarding 这样的特性，可能有许多屏幕由一个堆栈导航管理，所以有`OnboardingNavigator`。

您可以将 Navigator 视为分组子屏幕或功能的东西。因为我们是按特性分组的，所以将导航器放在特性文件夹中是合理的。基本上是这样的:

```
import { createStackNavigator } from 'react-navigation'
import Welcome from './Welcome'
import Term from './Term'

const routeConfig = {
  Welcome: {
    screen: Welcome
  },
  Term: {
    screen: Term
  }
}

const navigatorConfig = {
  navigationOptions: {
    header: null
  }
}

export default OnboardingNavigator = createStackNavigator(routeConfig, navigatorConfig)
```

### 图书馆

这是构建项目最有争议的部分。如果不喜欢`library`这个名字，可以取名为`utilities`、`common`、`citadel`、`whatever` …

这并不意味着无家可归的文件，但它是我们放置许多功能使用的公共工具和组件的地方。像原子组件、包装器、快速修复功能、网络材料和登录信息这样的东西被大量使用，很难将它们移动到特定的特性文件夹中。有时候我们只需要实际一点，把工作做完。

在 React Native 中，我们经常需要在许多屏幕中实现一个带有图像背景的按钮。这里有一个简单的保持在`library/components/ImageButton.js`里面的。`components`文件夹是存放可重用组件的，有时被称为[原子组件](https://medium.com/joeydinardo/a-brief-look-at-atomic-components-39cbe71d38b5)。根据 React 命名约定，第一个字母应该是大写的。

```
import React from 'react'
import { TouchableOpacity, View, Image, Text, StyleSheet } from 'react-native'
import images from 'res/images'
import colors from 'res/colors'

export default class ImageButton extends React.Component {
  render() {
    return (
      <TouchableOpacity style={styles.touchable} onPress={this.props.onPress}>
        <View style={styles.view}>
          <Text style={styles.text}>{this.props.title}</Text>
        </View>
        <Image
          source={images.button}
          style={styles.image} />
      </TouchableOpacity>
    )
  }
}

const styles = StyleSheet.create({
  view: {
    position: 'absolute',
    backgroundColor: 'transparent'
  },
  image: {

},
  touchable: {
    alignItems: 'center',
    justifyContent: 'center'
  },
  text: {
    color: colors.button,
    fontSize: 18,
    textAlign: 'center'
  }
})
```

如果我们想把按钮放在底部，我们使用一个实用函数来防止代码重复。这里是`library/utils/moveToBottom.js`:

```
import React from 'react'
import { View, StyleSheet } from 'react-native'

function moveToBottom(component) {
  return (
    <View style={styles.container}>
      {component}
    </View>
  )
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'flex-end',
    marginBottom: 36
  }
})

export default moveToBottom
```

#### 使用 package.json 避免相对路径

然后在`src/screens/onboarding/term/Term.js`的某个地方，我们可以通过使用相对路径来导入:

```
import moveToBottom from '../../../../library/utils/move'
import ImageButton from '../../../../library/components/ImageButton'
```

这在我眼里就是一面大红旗。这很容易出错，因为我们需要计算需要执行多少个`..`。如果我们移动特征，所有的路径都需要重新计算。

由于`library`将在很多地方使用，最好将其作为绝对路径引用。在 JavaScript 中，一个问题通常有 1000 个库。在谷歌上快速搜索会发现大量的图书馆来解决这个问题。但是我们不需要另一个依赖，因为这非常容易修复。

解决方法是将`library`转换成`module`，这样`node`就能找到它。将`package.json`添加到任何文件夹都会使其成为一个节点`module`。将`package.json`添加到`library`文件夹中，内容如下:

```
{
  "name": "library",
  "version": "0.0.1"
}
```

现在在`Term.js`中，我们可以很容易地从`library`导入东西，因为它现在是一个`module`:

```
import React from 'react'
import { View, StyleSheet, Image, Text, Button } from 'react-native'
import strings from 'res/strings'
import palette from 'res/palette'
import images from 'res/images'
import ImageButton from 'library/components/ImageButton'
import moveToBottom from 'library/utils/moveToBottom'

export default class Term extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.heading}>{strings.onboarding.term.heading.toUpperCase()}</Text>
        {
          moveToBottom(
            <ImageButton style={styles.button} title={strings.onboarding.term.button.toUpperCase()} />
          )
        }
      </View>
    )
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center'
  },
  heading: {...palette.heading, ...{
    marginTop: 72
  }}
})
```

### 表示留数

你可能想知道上面例子中的`res/colors`、`res/strings`、`res/images`、`res/fonts`是什么？嗯，对于前端项目，我们通常有组件，并使用字体，本地化字符串，颜色，图像和样式来设计它们。JavaScript 是一种非常动态的语言，很容易在任何地方使用字符串类型。我们可以有一堆跨许多文件的`#00B75D` `color`，或者将`Fira`作为许多`Text`组件中的`fontFamily`。这很容易出错，也很难重构。

让我们用更安全的对象将资源使用封装在`res`文件夹中。它们看起来像下面的例子:

**res/colors**

```
const colors = {
  title: '#00B75D',
  text: '#0C222B',
  button: '#036675'
}

export default colors
```

**res/strings**

```
const strings = {
  onboarding: {
    welcome: {
      heading: 'Welcome',
      text1: "What you don't know is what you haven't learn",
      text2: 'Visit my GitHub at https://github.com/onmyway133',
      button: 'Log in'
    },
    term: {
      heading: 'Terms and conditions',
      button: 'Read'
    }
  }
}

export default strings
```

**res/fonts**

```
const fonts = {
  title: 'Arial',
  text: 'SanFrancisco',
  code: 'Fira'
}

export default fonts
```

**分辨率/图像**

```
const images = {
  button: require('./images/button.png'),
  logo: require('./images/logo.png'),
  placeholder: require('./images/placeholder.png')
}

export default images
```

像`library`，`res`文件可以从任何地方访问，所以让我们把它变成一个`module`。将`package.json`添加到`res`文件夹中:

```
{
  "name": "res",
  "version": "0.0.1"
}
```

所以我们可以像普通模块一样访问资源文件:

```
import strings from 'res/strings'
import palette from 'res/palette'
import images from 'res/images'
```

#### 用调色板分组颜色、图像、字体

应用的设计应该是一致的。某些元素应该有相同的外观和感觉，这样才不会让用户感到困惑。例如，标题`Text`应该使用一种颜色、字体和字号。组件应该使用相同的占位符图像。在 React Native 中，我们已经将名称`styles`与`const styles = StyleSheet.create({})`一起使用，所以让我们使用名称`palette`。

下面是我简单的调色板。它定义了标题和`Text`的常用样式:

#### 分辨率/调色板

```
import colors from './colors'

const palette = {
  heading: {
    color: colors.title,
    fontSize: 20,
    textAlign: 'center'
  },
  text: {
    color: colors.text,
    fontSize: 17,
    textAlign: 'center'
  }
}

export default palette
```

然后我们可以在屏幕上使用它们:

```
const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center'
  },
  heading: {...palette.heading, ...{
    marginTop: 72
  }}
})
```

这里我们使用[对象扩展操作符](https://github.com/tc39/proposal-object-rest-spread)来合并`palette.heading`和我们的自定义样式对象。这意味着我们使用了来自`palette.heading`的样式，但是也指定了更多的属性。

如果我们要为多个品牌重新设计应用程序，我们可以有多个调色板。这是一个非常强大的模式。

#### 生成图像

您可以看到在`/src/res/images.js`中，我们在`src/res/images`文件夹中有每个图像的属性:

```
const images = {
  button: require('./images/button.png'),
  logo: require('./images/logo.png'),
  placeholder: require('./images/placeholder.png')
}

export default images
```

这是一项繁琐的手动操作，如果图像命名约定发生变化，我们必须进行更新。相反，我们可以添加一个脚本来基于我们拥有的图像生成`images.js`。在项目`/scripts/images.js`的根目录下添加一个文件:

```
const fs = require('fs')

const imageFileNames = () => {
  const array = fs
    .readdirSync('src/res/images')
    .filter((file) => {
      return file.endsWith('.png')
    })
    .map((file) => {
      return file.replace('@2x.png', '').replace('@3x.png', '')
    })

return Array.from(new Set(array))
}

const generate = () => {
  let properties = imageFileNames()
    .map((name) => {
      return `${name}: require('./images/${name}.png')`
    })
    .join(',\n  ')

const string = `const images = {
  ${properties}
}

export default images
`

fs.writeFileSync('src/res/images.js', string, 'utf8')
}

generate()
```

Node 很酷的一点是我们可以访问`fs`模块，这个模块非常擅长文件处理。这里我们简单地遍历图像，并相应地更新`/src/res/images.js`。

每当我们添加或更改图像时，我们可以运行:

```
node scripts/images.js
```

我们还可以在我们的 main `package.json`中声明脚本:

```
"scripts": {
  "start": "node node_modules/react-native/local-cli/cli.js start",
  "test": "jest",
  "lint": "eslint *.js **/*.js",
  "images": "node scripts/images.js"
}
```

现在，我们只需运行`npm run images`，就可以得到一个最新的`images.js` 资源文件。

#### 自定义字体怎么样

React Native 有一些[自定义字体](https://medium.com/react-native-training/list-of-available-react-native-fonts-ed78b48bd45e)，可能对你的项目足够好。您也可以使用自定义字体。

有一点需要注意的是，Android 使用的是字体文件的名称，而 iOS 使用的是全称。你可以在字体簿应用程序中看到全名，或者在运行的应用程序中检查

```
for (NSString* family in [UIFont familyNames]) {
  NSLog(@"%@", family);

for (NSString* name in [UIFont fontNamesForFamilyName: family]) {
    NSLog(@"Family name:  %@", name);
  }
}
```

对于要在 iOS 中注册的自定义字体，我们需要使用字体的文件名在`Info.plist`中声明`UIAppFonts`，对于 Android，字体需要放在`app/src/main/assets/fonts`。

将字体文件命名为全名是一个很好的做法。React Native 据说可以动态加载自定义字体，但如果您得到“无法识别的字体系列”，那么只需在 Xcode 中将这些字体添加到 target 中。

手工做这个需要时间，幸运的是我们有 rnpm 可以帮忙。首先添加`res/fonts`文件夹中的所有字体。然后只需在`package.json`中声明`rnpm`并运行`react-native link`。这应该在 iOS 中声明`UIAppFonts`,并将 Android 的所有字体移入`app/src/main/assets/fonts`。

```
"rnpm": {
  "assets": [
    "./src/res/fonts/"
  ]
}
```

通过名称访问字体容易出错，我们可以创建一个类似于我们对图像所做的脚本来生成一个更安全的访问。将`fonts.js`添加到我们的`scripts`文件夹

```
const fs = require('fs')

const fontFileNames = () => {
  const array = fs
    .readdirSync('src/res/fonts')
    .map((file) => {
      return file.replace('.ttf', '')
    })

return Array.from(new Set(array))
}

const generate = () => {
  const properties = fontFileNames()
    .map((name) => {
      const key = name.replace(/\s/g, '')
      return `${key}: '${name}'`
    })
    .join(',\n  ')

const string = `const fonts = {
  ${properties}
}

export default fonts
`

fs.writeFileSync('src/res/fonts.js', string, 'utf8')
}

generate()
```

现在您可以通过`R`命名空间使用自定义字体。

```
import R from 'res/R'

const styles = StyleSheet.create({
  text: {
    fontFamily: R.fonts.FireCodeNormal
  }
})
```

### R 命名空间

这一步取决于个人喜好，但我发现如果我们引入 R 名称空间会更有组织性，就像 Android 如何使用生成的 [R 类](http://App resources overview)处理资产一样。

> 一旦您将应用程序资源外部化，您就可以使用项目的`R`类中生成的资源 id 来访问它们。本文向您展示了如何在您的 Android 项目中对资源进行分组，并为特定设备配置提供替代资源，然后从您的应用程序代码或其他 XML 文件中访问它们。

这样，让我们在`src/res`中制作一个名为`R.js`的文件:

```
import strings from './strings'
import images from './images'
import colors from './colors'
import palette from './palette'

const R = {
  strings,
  images,
  colors,
  palette
}

export default R
```

并在屏幕上访问它:

```
import R from 'res/R'

render() {
  return (
    <SafeAreaView style={styles.container}>
      <Image
        style={styles.logo}
        source={R.images.logo} />
      <Image
        style={styles.image}
        source={R.images.placeholder} />
      <Text style={styles.title}>{R.strings.onboarding.welcome.title.toUpperCase()}</Text>
  )
}
```

将`strings`替换为`R.strings`，`colors`替换为`R.colors`，`images`替换为`R.images`。有了 R 注释，很明显我们正在从 app bundle 中访问静态资产。

这也符合 Airbnb 对于 singleton 的约定，因为我们的 R 现在就像一个全局常数。

> [23.8](https://github.com/airbnb/javascript#naming--PascalCase-singleton) 导出构造器/类/单体/函数库/裸对象时使用 PascalCase。

```
const AirbnbStyleGuide = {
  es6: {
  },
}

export default AirbnbStyleGuide
```

### 从这里去哪里

在这篇文章中，我已经向你展示了我认为你应该如何在一个 React 原生项目中构建文件夹和文件。我们还学会了如何以更安全的方式管理和访问资源。希望你觉得有用。以下是更多可供进一步探索的资源:

*   [组织一个 React 本地项目](https://medium.com/the-react-native-log/organizing-a-react-native-project-9514dfadaa0)
*   [在 React 中构建项目和命名组件](https://hackernoon.com/structuring-projects-and-naming-components-in-react-1261b6e18d76)
*   [使用 index.js 作为有趣的公共接口](https://alligator.io/react/index-js-public-interfaces/)

既然你在这里，你可以欣赏我的其他文章

*   [将 React 本机部署到 Bitrise、Fabric、CircleCI](https://medium.com/react-native-training/fixing-react-native-issues-and-happy-deploy-to-bitrise-fabric-circleci-44da4ab1487b)
*   [使用 React Native 中的 Flexbox 将元素定位在屏幕底部](https://medium.com/react-native-training/position-element-at-the-bottom-of-the-screen-using-flexbox-in-react-native-a00b3790ca42)
*   [在 React 原生项目中设置 ESLint 和 editor config](https://codeburst.io/setting-up-eslint-and-editorconfig-in-react-native-projects-31b4d9ddd0f6)
*   [2018 年 Firebase SDK 与 Firestore for React 原生应用](https://medium.com/react-native-training/firebase-sdk-with-firestore-for-react-native-apps-in-2018-aa89a67d6934)

如果你喜欢这个帖子，可以考虑访问[我的其他文章](https://github.com/onmyway133/blog/issues/165)和[应用](https://onmyway133.github.io/)？