# CSS 悬停选择器解释(用例子)

> 原文：<https://www.freecodecamp.org/news/hover-selector/>

CSS `:hover`选择器是许多用于样式化元素的伪类之一。`:hover`用于选择用户将光标或鼠标悬停在其上的元素。它可以用于所有元素，而不仅仅是链接。

当用于设计链接样式时，`:hover`通常与`:link`、`:visited`和`:active`选择器成对出现，它们分别设计未访问、已访问和活动链接的样式。

如果`:link`和`:visited`规则在 CSS 定义中，`:hover`应该在它们之后。否则，`:hover`规则中的样式不会应用到所选元素。

**语法:**

```
a:hover {
  /* CSS declarations */
}
```

当元素进入悬停状态时，悬停选择器仅应用规则中提供的样式。这通常是当用户将鼠标悬停在元素上时。

```
button {
  color: white;
  background-color: green;
}

button:hover {
  background-color: white;
  border: 2px solid green;
  color: green;
}
```

在上面的例子中，按钮的正常样式是绿色按钮上的白色文本。当用户用鼠标悬停在按钮上时，带有`:hover`选择器的规则将被激活，按钮的样式将改变。

请注意`:hover`在触摸屏上可能会有问题——不同的硬件和移动浏览器实现可能会导致伪类在某些情况下被触发，而在其他情况下则不会。确保在尽可能多的不同移动浏览器和设备中使用`:hover`彻底测试元素。