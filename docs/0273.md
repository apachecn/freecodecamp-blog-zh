# HTML 下拉菜单–如何添加带有 Select 元素的下拉列表

> 原文：<https://www.freecodecamp.org/news/html-drop-down-menu-how-to-add-a-drop-down-list-with-the-select-element/>

许多网站、应用程序和网页使用下拉菜单来帮助显示信息列表。您可以使用它们来创建导航菜单、表单选项等。

如果您正在查看这些菜单或列表，您可能会想象创建它们会有多复杂。是的，在某些情况下，事情会变得有点复杂。

下拉菜单是一个选项列表，当用户通过点击它或者将光标悬停在它上面来与菜单交互时，它会垂直显示出来。

当用户通过再次点击或将光标从菜单上移开而停止与菜单的交互时，该菜单也消失。

在本文中，您将学习如何在网页的`select`元素中添加一个下拉列表。您还将学习各种可用的选项，以及如何创建可悬停的下拉列表/菜单。

## 如何创建一个 HTML 下拉列表

在 HTML 中，默认情况下，你可以创建一个下拉列表，在选项标签旁边加上`select`标签。这主要用于在表单中从许多选项列表中选择一个值。

select 标记有两个主要属性:`name`和`id`属性。

```
// Syntax
<select name="" id="">
  <option value="">...</option>
  // ...
</select> 
```

在表单中提交选择时，使用`name`属性来标识下拉列表。您可以将`id`属性连接到与其`for`属性具有相似值的`label`。

```
// Syntax
<label for="languages">List of Languages:</label>
<select name="" id="languages">
  <option value="">...</option>
  // ...
</select> 
```

`select`标签还有一些可选的布尔属性，如`disabled`(禁用选择字段)、`required`(使字段成为表单中的必填字段)，等等。

```
<select name="" id="languages" required>
  // ...
</select>

// Or

<select name="" id="languages" disabled>
  // ...
</select> 
```

在`select`标签中，您可以在单独的`option`标签中添加许多选项。`option`标签有一个属性`value`，它指定了当一个选项被选中时从表单提交的一个值。

```
<select name="language" id="language">
  <option value="javascript">JavaScript</option>
  <option value="python">Python</option>
  <option value="c++">C++</option>
  <option value="java">Java</option>
</select> 
```

还有其他布尔属性，如`disabled`(禁用菜单中的选项)和`selected`(用于在页面加载时将特定选项设置为默认选择的选项，而不是第一个选项)。

```
<select name="language" id="language">
  <option value="javascript" disabled>JavaScript</option>
  <option value="python">Python</option>
  <option value="c++">C++</option>
  <option value="java" selected>Java</option>
</select> 
```

在上面的代码中，第一个选项有一个属性`disabled`，这意味着您将不能选择该选项。第四个选项有一个属性`selected`，这意味着将选择 Java，而不是默认选择 JavaScript 作为选择值。

[https://codepen.io/olawanlejoel/embed/YzaNgmw?default-tab=html%2Cresult](https://codepen.io/olawanlejoel/embed/YzaNgmw?default-tab=html%2Cresult)

See the Pen [Dropdown List](https://codepen.io/olawanlejoel/pen/YzaNgmw) by Olawanle Joel ([@olawanlejoel](https://codepen.io/olawanlejoel)) on [CodePen](https://codepen.io).

## 如何创建可悬停的下拉菜单

当你浏览或访问许多先进和现代的网页时，你会注意到它们有下拉菜单。

这些菜单用于导航，有助于将相似的链接放在一起。大多数情况下，当您将鼠标悬停在父菜单上时，会出现下拉列表。

![s_B4C6D2ADDF91C398F7D0077C06A79A5494062ED47759B85768844AD11A4B757E_1664053790313_image](img/6a4f6b896ebf8d9dafcbbee54e1214d2.png)

您可以用各种方式创建这些类型的菜单，因为没有直接的语法来构建它们。

您可以使用 CSS 样式来创建它，以便在用户悬停在菜单上时显示和隐藏下拉列表。一个非常好的方法是创建一个保存菜单和下拉菜单的`div`。

```
<div class="dropdown">
  <button>Profile</button>
  <div class="dropdown-options">
    <a href="#">Dashboard</a>
    <a href="#">Setting</a>
    <a href="#">Logout</a>
  </div>
</div> 
```

这个`div`作为一个容器，你可以把它设计成`relative`的`position`和`inline-block`的`display`，这样下拉选项就会出现在菜单下面。

```
.dropdown {
  display: inline-block;
  position: relative;
} 
```

您可以随意设计按钮和`dropdown-options`的样式。但是默认情况下，控制悬停效果的主要样式将`dropdown-options`设置为不显示。然后当鼠标悬停在菜单上时，显示没有设置为`block`，所以选项是可见的。您还可以将`position`设置为`absolute`，这样选项就会出现在菜单下方，并将`overflow`设置为`auto`以启用小屏幕上的滚动。

```
.dropdown-options {
  display: none;
  position: absolute;
  overflow: auto;
}

.dropdown:hover .dropdown-options {
  display: block;
} 
```

在下面的演示中，我们添加了更多的样式，使下拉菜单更有吸引力和美观:

[https://codepen.io/olawanlejoel/embed/ZExZGdK?default-tab=html%2Cresult](https://codepen.io/olawanlejoel/embed/ZExZGdK?default-tab=html%2Cresult)

See the Pen [Hoverable dropdown menu](https://codepen.io/olawanlejoel/pen/ZExZGdK) by Olawanle Joel ([@olawanlejoel](https://codepen.io/olawanlejoel)) on [CodePen](https://codepen.io).

## 包扎

在本文中，您已经学习了如何使用 select 标记创建下拉列表。您还学习了如何用 CSS 创建可悬停的下拉菜单来处理悬停效果。

你可以在 **HTML 选择标签-如何制作下拉菜单或组合列表**T3[kola de](https://www.freecodecamp.org/news/author/kolade/)的[这篇文章中了解更多关于选择标签的信息。](https://www.freecodecamp.org/news/html-select-tag-how-to-make-a-dropdown-menu-or-combo-list/)