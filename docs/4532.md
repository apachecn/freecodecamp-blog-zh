# 如何通过 4 个简单的步骤将 CSS 模块样式表添加到 React 组件中

> 原文：<https://www.freecodecamp.org/news/how-to-add-a-css-modules-stylesheet-to-your-react-component-in-4-simple-steps/>

假设您想在项目中添加一个 CSS 模块样式表。你可以在这里找到 Create React App 的[指南](https://facebook.github.io/create-react-app/docs/adding-a-css-modules-stylesheet)，但是本质上——正如指南所说——CSS 模块让你在不同的文件中使用*相同的* CSS 选择器，而不用担心命名冲突。这样做是因为你的文件中每个你想要设计样式的 HTML 元素都会自动被赋予一个*唯一的*类名。

乍一看，这似乎很令人困惑，但实际上实现 CSS 模块的过程可以简化为 4 个步骤，如下例所示。

![1_lmno_4nmNyohGa-gM_GaEw](img/e852d17bff4e9a11a7478a681d6e1ddd.png)

Yes, really!

### 向简单的<link>组件添加模块化 CSS

1.  React 的一个特性是 CSS 模块对于以`.module.css`扩展名结尾的文件是“打开”的。使用以下格式的特定文件名创建 CSS 文件:

```
Link.module.css
```

2.将样式导入组件:

```
import styles from ‘../styling/components/Link.module.css’
```

3.CSS 文件中的样式可以遵循您喜欢的命名约定，例如:

```
.bold {  font-weight: bold;}
```

4.样式应用于 HTML 元素，如下所示:

```
className={styles.bold}
```

就是这样！

*图片来源:* [*阿德里安·斯旺卡*](https://unsplash.com/photos/72El6N0cmj4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *上* [*下*](https://unsplash.com/search/photos/confused?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)