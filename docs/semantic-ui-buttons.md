# 语义 UI 按钮

> 原文：<https://www.freecodecamp.org/news/semantic-ui-buttons/>

## **什么是语义 UI 按钮？**

按钮指示可能的用户操作。语义 UI 提供了易于使用的语法，不仅简化了按钮的样式，还简化了自然语言语义。

## 如何使用它们

语义 UI 类只是添加到一个按钮元素中。下面给出各种例子来说明如何使用它。

### 类型

*   标准按钮

标准语义 UI 按钮

```
<button class="ui button">Standard Button</button>
```

*   强调按钮

具有不同强调级别的按钮

```
<button class="ui primary button">
```

其他强调类有`secondary`、`positive`和`negative`

*   动画按钮

带有动画的按钮，显示隐藏的内容

```
<div class="ui animated fade button" tabindex="0">
  <div class="visible content">Sign-up for a Pro account</div>
  <div class="hidden content">$12.99 a month</div>
</div>
```

上面的属性`tabindex="0"`使按钮键盘成为焦点，因为没有使用`<button>`标签。

*   标签按钮

标签旁边的按钮

```
<div class="ui labeled button" tabindex="0">
  <div class="ui button"><i class="heart icon"></i> Like</div>
  <a class="ui basic label">2,048</a>
</div>
```

元素`<i>`用于指定一个图标，这里是一个心形图标`<i class="heart icon"></i>`和基本标签`<a class="ui basic label">2,048</a>`

*   图标按钮

语义 UI 按钮可以只是一个图标

```
<button class="ui icon button">
  <i class="camera icon"></i>
</button>
```

上面只是一个相机图标

### 组

语义 UI 按钮可以存在于一个组中

```
<div class="ui buttons">
  <button class="ui button">One</button>
  <button class="ui button">Two</button>
  <button class="ui button">Three</button>
</div>
```

### 内容

语义 UI 按钮可以包含条件

```
<div class="ui buttons">
  <button class="ui positive button">Yes</button>
  <div class="or" data-text="or"></div>
  <button class="ui negative button">No</button>
</div>
```

### 州

语义 UI 按钮可以有不同的状态，`active`、`disabled`、`loading`。只需在属性`of`元素的`class`中添加一个州名。

```
<button class="ui loading button">Loading</button>
```

### 变化

语义 UI 按钮有多种尺寸，`mini`、`tiny`、`small`、`medium`、`large`、`big`、`huge`、`massive`。

```
<button class="ui massive button">Massive Button</button>
```

关于语义 UI 按钮还有很多，请访问更多信息部分提供的链接了解更多信息。

#### ****更多信息:****

*   [语义 UI 按钮文档](https://semantic-ui.com/elements/button.html)