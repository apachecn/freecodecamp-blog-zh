# react cheat sheet–你应该知道的 9 个常见 HTML 渲染案例

> 原文：<https://www.freecodecamp.org/news/react-cheatsheet-html-rendering/>

每当我开始使用一个新的框架，或者已经有一段时间没有使用它了，我总是会寻找同样简单的东西。

我将搜索如何呈现原始 HTML，如何基于条件显示组件，如何为元素分配类，等等。

这就是为什么我用 React [和 JSX](https://reactjs.org/docs/introducing-jsx.html) 创建了这个备忘单，上面有九个你会经常执行的最常见的任务。

我按照构建应用程序时偶然发现它们的方式对它们进行了排序。在下面的例子中，第一个代码片段将显示语法，第二个代码片段将显示如何在真实数据中使用它。

## 如何将数据输出到 HTML

最简单的用例——将变量值呈现在 HTML 标记中。这通常是第一个测试，向您显示应用程序已经处理了数据，并可以呈现它们:

```
{ variable } 
```

```
{ metadata.subtitle.value } 
```

## 如何添加标准的类属性

虽然许多框架保持 HTML 标记不变，但 React 不允许保留字“class”用于样式。我们需要用`className`来代替，就像这样:

```
<... className="classname"> 
```

```
<div className="sidebar__inner"> 
```

## 如何将数据输出到 HTML 属性中

这个用例与构建链接相关。但是在很多情况下，你还需要填充一些属性，比如标题、数据，甚至是简单的图片标签:

```
< ... name={variable}> 
```

```
<a href={`https://twitter.com/${author.twitter.value}`}> 
```

## 如何输出原始 HTML

有些内容是在另一个系统中构建的，例如，在无头 CMS 中编写的富文本。

在这些情况下，您通常会使用像 SDK 这样的工具来构建 HTML。这是将它添加到标记中的方法:

```
< ... dangerouslySetInnerHTML={{__html: variable}}></...> 
```

```
<div dangerouslySetInnerHTML={{__html: article.teaser}}></div> 
```

## 如何迭代数据集

在索引页面、网站地图、搜索页面或任何需要显示集合数据的地方，React 和 JSX 允许您将(几乎)全能的`map` 功能与 HTML 标记结合起来:

```
let components = collectionVariable.map((item) =>
  <Component data={item} key={item.uniqueKey} />);
...
<div>{components}</div> 
```

```
let articleComponents = articles.map((article) =>
  <Article data={article} key={article.id} ... />);
...
<div>{articleComponents}</div> 
```

## 如何使用索引迭代数据集

这是相同的用例，但是它允许您访问每个迭代项的索引。在某些情况下，索引也可以用来替换唯一键:

```
let components = collectionVariable.map((item, index) =>
  <Component data={item} index={index} key={uniqueKey} />);
...
<div>{components}</div> 
```

```
let articleComponents = articles.map((article) =>
  <Article data={article} index={index} key={article.id} ... />);
...
<div>{articleComponents}</div> 
```

## 如何呈现条件标记

这是 JSX 典型的 *if* 情况。它通常在数据加载期间使用，以显示预加载器或根据数据决定使用标记的哪一部分:

```
{variable !== null && <... >} 
```

```
{data.length > 0 && <div> ... </div>} 
```

## 如何呈现包含 else 分支的条件标记

else 分支通过颠倒条件来实现:

```
{variable !== null && <... >}
{variable == null && <... >} 
```

```
{data.length > 0 && <div> ... </div>}
{data.length == 0 && <div>Loading...</div>} 
```

## 如何将数据传递给子组件

最后，当你开始使用组件时，这就是你如何通过 props 向孩子们提供数据:

```
<component componentVariable={variable}> 
```

```
<links author={author}> 
```

我希望这能帮你省去一些谷歌搜索:-)

如果你正在寻找一个可打印的(PDF)版本，它可以在我的[个人网站](https://ondrabus.com/react-vue-angular-cheatsheet)上找到，在那里你也可以找到类似的 Vue 和 Angular 的备忘单。