# 如何在与 GSAP 互动中创建一个可重用的淡入动画组件

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-reusable-fade-in-animation-component-in-react-with-gsap/>

如果你没有听说过或使用过 GSAP，你就错过了。GSAP 是一个组件和元素的动画库。他们的主页上展示了许多你可以用这个工具制作的精彩动画。

GSAP 有很多配置，没有一个正确的方法来实现一种类型的动画。所以我们将会看到一种在组件加载时创建“淡入”动画的方法(固执己见)。

这篇文章不会详细介绍如何使用 GSAP。如果您想要深入了解该工具，他们的文档是您的首选资源。

## 我们要制作的动画

下面是对我们将要制作的动画的一点描述:

这很简单。当一个组件被加载时(无论在哪里)，它会淡入。我们还将添加方向，以便组件从区域淡入到正常位置。

我们还将使动画组件可重用，以便我们可以将其应用于不同的元素。

## 我们开始吧！

### GSAP 装置

首先，您必须设置一个 react 项目。如果你需要为这个项目快速设置一个，create-react-app 随时恭候。

要安装 GSAP，请在终端中输入以下命令(当前目录是您的 react 项目目录):

```
npm install --save gsap 
```

### 创建可用的动画组件

#### 设置组件

让我们称我们的组件为:

```
import React, {useRef, useEffect} from 'react'

const FadeInAnimation = ({children, wrapperElement = 'div', direction = null, ...props}) => {
  const Component = wrapperElement;
  const compRef = useRef(null)
  useEffect(() => {
    // animations
  }, [compRef])
  return (
    <Component ref={compRef} {...props}>
      {children}
    </Component>
  )
}

export default FadeInAnimation 
```

我们的动画还没有准备好，但是让我们理解我们从什么开始。

*   `wrapperElement`:用于指定组件是什么。它的默认值是`div`。这比创建一个额外的 DOM 节点来包装我们想要制作动画的组件要好。
*   [`useRef`](https://reactjs.org/docs/hooks-reference.html#useref) : `gsap`我们需要这个来知道为什么要触发动画。这样，我们可以在 DOM 中引用我们的组件。
*   `useEffect`:没有这个，`gsap`会触发一个空引用的动画(`useRef(null)`)。我们必须确保组件已经安装，因此这个钩子。
*   `children`:这将是在`<FadeInAnimation>`和`</FadeInAnimation>`之间发现的。可能是文本，甚至是一组元素。
*   `...props`:为了扩展可重用性，这是必要的，这样组件就可以应用其他道具，比如`className`和`style`。
*   `direction`:对于我们想要给淡入效果添加方向的情况。默认值为 null。

现在让我们去 GSAP。

#### 设置动画

```
import React, { useRef, useEffect } from "react";
import { gsap } from "gsap";

const FadeInAnimation = ({
  children,
  wrapperElement = "div",
  direction = null,
  delay = 0,
  ...props
}) => {
  const Component = wrapperElement;
  let compRef = useRef(null);
  const distance = 200;
  let fadeDirection;
  switch (direction) {
    case "left":
      fadeDirection = { x: -distance };
      break;
    case "right":
      fadeDirection = { x: distance };
      break;
    case "up":
      fadeDirection = { y: distance };
      break;
    case "down":
      fadeDirection = { y: -distance };
      break;
    default:
      fadeDirection = { x: 0 };
  }
  useEffect(() => {
    gsap.from(compRef.current, 1, {
      ...fadeDirection,
      opacity: 0,
      delay
    });
  }, [compRef, fadeDirection, delay]);
  return (
    <Component ref={compRef} {...props}>
      {children}
    </Component>
  );
};

export default FadeInAnimation; 
```

让我们回顾一下这里发生的事情:

*   我们将变量`distance`初始化为 200。这对于应用方向的情况很有用。您还可以将它添加到输入属性中，以便使用它的组件可以决定。
*   我们有我们的`switch`案例。这是为了确定淡入的方向，默认情况下没有指定方向。
*   然后 [`gsap`](https://greensock.com/docs/v3/GSAP) 。这是从 GSAP 曝光的动画组件。你可以在文档中找到`.to`、`.from`、`.fromTo`等等。
*   `gsap.from`在我们的例子中是指组件在最终状态(在组件的样式表中设置)之前的初始状态。我们将 ref 的当前元素作为目标，应用 1 秒的持续时间，并应用动画选项。
*   `...fadeDirection`:我们扩展对象，使其以`{x: 200}`或指定的形式出现。`x`代表水平，`y`代表垂直。
*   然后，初始不透明度为 0，延迟由组件指定。

仅此而已。让我们制作一个使用这个令人敬畏的动画的组件。

### 如何使用我们可重用淡入组件

到你想制作动画的组件，做一些类似下面的事情:

```
import React from "react";
import FadeInAnimation from "./FadeInAnimation";

export default function App() {
  return (
    <div>
      <FadeInAnimation wrapperElement="h1" direction="down">
        Hello CodeSandbox
      </FadeInAnimation>
      <FadeInAnimation wrapperElement="h2" direction="right" delay={2}>
        Start editing to see some magic happen!
      </FadeInAnimation>
      <FadeInAnimation
        style={{
          width: 200,
          color: "white",
          height: 200,
          backgroundColor: "purple"
        }}
        direction='up'
      >
        <p>Hello</p>
      </FadeInAnimation>
    </div>
  );
} 
```

如上所示，我们的`FadeInAnimation`组件可以接受一个`style`道具。记得我们做过`...props`。

这是 [CodeSandBox](https://codesandbox.io/s/react-gsap-fadein-effect-z8xqd?file=/src/App.js) 中的结果

## 包裹

那是一个包裹。这是 GSAP 淡入效果的一个简单(固执己见)的用法。

当然，你可以进一步配置它，比如制作淡入反弹效果，淡入旋转，以及其他有趣的事情。但是我希望这篇文章已经向你简要介绍了 GSAP 有多棒，以及如何开始在网络上做令人惊奇的事情。

旁注:这类似于我在即将发布的新动画包中使用的设置。等这篇文章发表了我再分享: )