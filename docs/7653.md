# 如何将 AngularJS 1.x 应用程序转换为 React 应用程序—一次一个组件。

> 原文：<https://www.freecodecamp.org/news/how-to-convert-an-angular-app-to-a-react-app-one-component-at-a-time-ba985eaae66e/>

Angular 和 React 都是很棒的框架/库。Angular 提供了 MVC(模型、视图、控制器)的定义结构。React 提供了基于状态变化的轻量级渲染机制。通常，开发人员会用 AngularJS 编写一个带有遗留代码的应用程序，但他们会希望在 ReactJS 中构建新的特性。

尽管可以淘汰 AngularJS 应用程序并从头构建 ReactJS 应用程序，但对于大规模应用程序来说，这不是一个可行的解决方案。在这种情况下，单独构建 React 组件并将其导入 Angular 更容易。

在这篇文章中，我将帮助你使用`react2angular`在 Angular 应用程序中创建一个 React 组件。

### 规划应用程序

这是我们将要做的——

**Given** :一个有角度的应用程序，呈现一个城市的名字和它的主要景点。

**目标**:给 Angular app 添加一个 React 组件。React 组件将显示一个景点的特色图像。

**计划**:我们将创建一个 React 组件，通过`imageUrl`到`props`传递，并将图像显示为 React 组件。

我们开始吧！

### 步骤 0:拥有一个有角度的应用

出于本文的目的，让我们保持 Angular 应用程序的复杂性简单。我计划在 2018 年去欧洲旅行，因此我的 Angular 应用程序基本上是我想去的地方的清单。

这是我们的数据集`bucketlist`的样子:

```
const bucketlist = [{
  city: 'Venice',
  position: 3,
  sites: ['Grand Canal', 'Bridge of Sighs', 'Piazza San Marco'],
  img: 'https://unsplash.com/photos/ryC3SVUeRgY',
}, {
  city: 'Paris',
  position: 2,
  sites: ['Eiffel Tower', 'The Louvre', 'Notre-Dame de Paris'],
  img: 'https://unsplash.com/photos/R5scocnOOdM',
}, {
  city: 'Santorini',
  position: 1,
  sites: ['Imerovigli', 'Akrotiri', 'Santorini Arts Factory'],
  img: 'https://unsplash.com/photos/hmXtDtmM5r0',
}];
```

这是`angularComponent.js`的样子:

```
function AngularComponentCtrl() {
  var ctrl = this;
  ctrl.bucketlist = bucketlist; 
};

angular.module(’demoApp’).component(’angularComponent’, {
  templateUrl: 'angularComponent.html’,
  controller: AngularComponentCtrl
});
```

这是`angularComponent.html`:

```
<div ng-repeat="item in $ctrl.bucketlist" ng-sort="item.position">
  <h2>{{item.city}}</h2>
  <p> I want to see <span ng-repeat="sight in item.sights">{{sight}}                 </p></span>
</div>
```

超级简单！虽然现在可以去圣托里尼岛…

![p83cdbYPyyvGn1IrTNnzC-XDpcWuSm7-1VBu](img/e50c2e13d97b55ea4fa1f864af277f9e.png)

That moment when you come back from vacation and cannot wait for next vacation. Photo by [Alexandre Chambon](https://unsplash.com/photos/aapSemzfsOk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/santorini?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 步骤 1:安装依赖项

如果你的编辑器没有配置，从 Angular 到 React world 会是一件非常痛苦的事情。让我们先做那件事。我们将首先安装安装程序林挺。

```
npm install --save eslint babel-eslint
```

接下来，我们来安装`react2angular`。如果你从未安装过 React，你还需要安装`react`、`react-dom`和`prop-types`。

```
npm install --save react2angular react react-dom prop-types
```

### 步骤 2:创建一个 React 组件

现在，我们已经有了一个呈现城市名称的角度组件。接下来，我们需要渲染特色图像。现在让我们假设图像是通过`props`提供给我们的(稍后我们将了解`props`是如何工作的)。我们的 React 组件如下所示:

```
import {Component} from 'react';

class RenderImage extends Component {

  render() {
    const imageUrl = this.props.imageUrl;
    return (
      <div>
        <img src={imageUrl} alt=""/>
      </div>
      );
  }
}
```

### 第三步:传递道具

记得在`Step 2`中，我们假设通过`props`我们有一个可用的图像。我们现在要填充`props`。您可以使用`props`将依赖关系传递给 React 组件。请记住，React 组件无法使用任何角度从属关系。可以这样想 React 组件就像一个连接到 Angular 应用程序的容器。如果您需要容器从父容器继承信息，您将需要通过`props`显式地连接它。

因此，为了传递依赖关系，我们将在 angular 中添加一个组件`renderImage`,并作为参数传递`imageUrl`:

```
 angular.module(’demoApp’, [])
.component(’renderImage’, react2angular(RenderImage,[’imageUrl’]));
```

### 步骤 4:包含在角度模板中

现在，您可以像导入任何其他组件一样，在 Angular 应用程序中导入此组件:

```
<div ng-repeat="item in $ctrl.bucketlist">
  <h2>{{item.city}}</h2>
  <p> I want to see <span ng-repeat="site in item.sites">{{site}}</span>
  <render-image image-url={{item.img}}></render-image>
</div>
```

哒哒！太神奇了！不完全是。是努力和汗水。还有咖啡。很多。

![-L8XanmrOrxeb9YzUIjafJ21x2KRbuSbmaCy](img/4c441e56ffeec5d96cd9df6ab04b9c08.png)

Coffee Coffee Coffee. Photo by [Christiana Rivers](https://unsplash.com/photos/02MLReRp3I8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/new-york?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 现在去制造一些反应元件吧，勇敢的战士！

特别要感谢[大卫·吉](https://twitter.com/dvdgee)把我介绍给`react2angular`并在我深陷棱角分明的世界时帮助我看到隧道尽头的光明。

资源:

1.  [这篇文章](https://medium.com/@panagiotisvrs/angularjs-migration-to-react-redux-2d3bb3a7cc84)帮了我大忙。
2.  react2angular 的官方文件

**如果这篇文章对你有帮助，请点击？按钮，以便其他开发人员也能看到。**