# 如何在 React 中用 Props 构建可重用组件

> 原文：<https://www.freecodecamp.org/news/how-to-build-reusable-components-with-props-in-react/>

Props 是 React.js 最重要的构建模块之一。当您需要将数据从父组件向下传递到子组件时，props 使这成为可能。

在本文中，我们将学习如何在 React 中使用 props 和构建可重用组件。

## React 里的道具是什么？

Props 是 properties 的简称。它们表示您希望传递给特定组件的信息，以便组件可以重用这些信息。

可以作为道具传递的信息范围从类名到高度、宽度等等。这使得创建可重用的项目变得非常容易，比如按钮、卡片、输入元素等等。

## 如何在 React 中使用道具

现在你已经知道了什么是道具，以及它们在组件中的主要工作，是时候演示在 React 中创建可重用组件时如何使用道具了。

假设您有一个新的或现有的 React 应用程序，创建两个新文件。我们将第一个文件称为 **Author.jsx** ，第二个文件称为***author profile . jsx***。**

*现在打开“Author.jsx”文件，添加以下代码:*

```
*`export default function Author({name, imageUrl}){
	return (
    	<div className="author">
          <h2>{name}</h2>
          <img src={imageUrl} alt={name}/>
        </div>
    )
}`*
```

*在上面的代码中我们有什么？*

*在 React 中我们有常规的功能组件，然后我们有道具–`name`和`imageUrl`。它们被析构并直接传递到我们的函数中，因此我们可以在应用程序的任何部分重用这个 Author 组件。*

*现在，要在 AuthorProfile.jsx 中重用 Author.jsx 组件，我们可以这样做:*

```
*`import Author from "./Author"

export default function AuthorProfile(){
	return (
      <div className="author-profile">
      	<Author name="Alvin"  
        imageUrl="https://avatars.githubusercontent.com/u/43749581?v=4" />
      </div>
    )
}`*
```

*首先，我们必须将组件导入到我们想要重用它的地方，正如您在上面代码的第一行中看到的，在那里我们有 import 语句。*

*第二，因为我们之前在 author 组件中把`name`和`imageUrl`作为道具传入，所以 author 组件首先期望相同的数据传入。*

*我们可以在道具的帮助下，在一个大的代码库中，用这种方式让很多组件重用。这有助于我们正确地构建代码。*

*太棒了。*

## *道具示例*

*让我们用最后一个例子来演示道具是如何有帮助的——但是这一次，我们将在道具的帮助下创建一个可重复使用的按钮。*

*创建一个名为 PrimaryButton.jsx 的新文件。在 PrimaryButton 文件中，我们这样做:*

```
*`export default function PrimaryButton({width,
  height,
  backgroundColor,
  color,
  border,
  borderColor,
  fontSize,
  buttonText,}) {
	return (
    	<button style={{width, height, backgroundColor, color, border, borderColor, fontSize}}>
           {buttonText}
        </button>
    )
}`*
```

*太棒了，现在我们有了一个可以在应用程序的任何地方使用的按钮。我们可以简单地导入可重用的按钮，传入所需的数据，而不是一遍又一遍地重复或创建按钮，这样就万事大吉了。*

## *包扎*

*现在，您已经学习了如何在 React 中借助 props 构建可重用的组件。尝试更多你自己的例子，祝你快乐！*