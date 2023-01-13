# 如何添加仅用于键盘交互的聚焦环

> 原文：<https://www.freecodecamp.org/news/focus-rings-for-keyboard-interactions-only/>

在任何项目中，有一件事不可避免地会进入我们的 QA 流程，那就是焦点环的意外出现。

![focus-ring](img/425cbbdf371a4d22318f2ba95536ca4e.png)

A wild focus ring appeared!

我们已经就如何处理这些问题进行了多次讨论。项目经理和设计师经常建议删除它们。虽然这是一个简单的解决方案，但这是一个网页设计的反模式。所有浏览器都提供了默认的焦点环，以便键盘用户可以确定哪个元素当前处于焦点中。事实上，**对焦环需要满足无障碍标准**:

> 任何键盘可操作的用户界面都有一种操作模式，其中键盘焦点指示器是可见的。
> - [**W3 网站内容无障碍指南**](https://www.w3.org/TR/WCAG21/#focus-visible)

即使我们决定不移除聚焦环，设计师通常对默认风格也不满意。最近出现的一个问题是，如果焦点环样式是为键盘用户制作的，以跟踪页面上的焦点，为什么它们需要在我单击某个元素时显示出来？我们可以只为键盘用户添加对焦环吗？

答案是肯定的！只有当用户使用键盘导航时，我们才可以使用 [**`:focus-visible`【多填充】**](https://github.com/WICG/focus-visible) 来添加聚焦环。

## **如何使用`:focus-visible` polyfill**

以下是你现在如何在你的项目中实现`:focus-visible`的方法。

如果使用 ES6 模块，通过 npm: `npm install --save focus-visible`安装 polyfill

将模块导入到主 JavaScript 文件中:

```
import 'focus-visible'; 
```

当您的页面加载时，您的`<body>`将获得一个`.js-focus-visible`类，这样您就可以有条件地隐藏默认的焦点环，只有当 polyfill 被加载时。另外，当你通过键盘导航时，聚焦的元素会得到一个`.focus-visible`类。

现在我们可以添加我们的 css:

```
// override UA stylesheet, only when polyfill is loaded
.js-focus-visible :focus:not(.focus-visible) {
    outline-width: 0;
}

// establish desired focus ring appearance for appropriate input modalities
.focus-visible {
  outline: 2px solid $bright-brand-color;
} 
```

## **其他资源**

*   [**`:focus-visible`Github 上的 polyfill**](https://github.com/WICG/focus-visible)
*   [](https://www.youtube.com/watch?v=ilj2P5-5CjI&feature=youtu.be)
*   **[**CSS 工作组焦点-可见伪类规范**](https://drafts.csswg.org/selectors-4/#the-focus-visible-pseudo)**

**想要更深入地构建无障碍网站吗？加入我的免费邮箱课程:？ *[**常见无障碍错误及如何避免**](https://benrobertson.io/courses/common-accessibility-mistakes/) 。30 天，10 节课，100%的乐趣！*？ [***在这里报名***](https://benrobertson.io/courses/common-accessibility-mistakes/) ！**

***本帖原载于 [benrobertson.io](https://benrobertson.io/accessibility/focus-ring-keyboard-only) 。***