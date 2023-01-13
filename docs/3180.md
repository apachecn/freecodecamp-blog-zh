# 无法在 React 中找到基于 URL 的图像

> 原文：<https://www.freecodecamp.org/news/unable-to-find-images-based-on-url-in-react/>

如果你是个新手，在访问本地存储的图像时遇到了困难，你并不孤单。

假设您将图像存储在如下组件旁边的目录中:

```
/src
  /components
    - component1
    - component2
/img
  - img1
  - img2
```

您试图从`component2`访问`/img`目录中的图像:

```
import React, { Component, useState, useEffect } from 'react';
import { render } from 'react-dom'
import { useTransition, animated, config } from "react-spring";
import imgArr from './images';
import '../App.css';

const Slideshow = () => {
  const [index, set] = useState(0)
  const transitions = useTransition(imgArr[index], item => item.id, {
    from: { opacity: 0 },
    enter: {opacity: 1 },
    leave: { opacity: 0 },
    config: config.molasses,
  })
  useEffect(() => void setInterval(() => set(state => (state + 1) % 4), 2000), [])
  return transitions.map(({ item, props, key }) => (
    <animated.div
      key={key}
      className="bg"
      style={{ ...props, slideshowContainerStyle}}
    >
      <img className="img" src={require(item.url)} alt=""/>
    </animated.div>
  ))
}

const slideshowContainerStyle = {
 width: '80%',
 height: '300px'
}

export default Slideshow; 
```

你已经尝试了路径`../img/img1.jpg`和`..img/img1.jpg`，但是得到了`Error: Cannot find module '<path>'`。

这是怎么回事？

## 稍微讲一下`create-react-app`

像大多数人一样，你可能使用`create-react-app`来引导你的项目。

在这种情况下，使用相对路径可能有点棘手，因为`create-react-app`将 HTML、CSS 和 JavaScript 文件构建到一个输出文件夹:

```
/public
  - index.html
  - bundle.js
  - style.css
```

有很多方法可以使用`create-react-app`中的图像，但最简单的方法之一是将您的图像包含到`/public`中。当您的项目被构建时，`/public`作为根目录。

您可以在 [Create React App 文档](https://create-react-app.dev/docs/adding-images-fonts-and-files/)中了解更多关于添加图像或其他资源的信息。

## 导入图像

如果你看一下文档，你会注意到代码中包含了`import`语句来加载像图像和字体这样的资产。

当您的资源(在本例中为图像)位于使用它的组件所在的同一目录或附近，并且不会在其他任何地方使用时，导入非常有用。例如，如果您有一个带有按钮的输入组件，这些按钮使用 SVG 来表示拇指向上和拇指向下的图标。

使用`import`的一点好处是，如果有输入错误，它会在构建时抛出一个错误。这种检查有助于确保用户不会看到错过的破损图像。