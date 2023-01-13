# CSS 特异性以及何时使用 CSS 重要标签

> 原文：<https://www.freecodecamp.org/news/what-is-css-specificity/>

如果你想在 CSS 方面做得更好，CSS 特异性是一个需要理解的重要话题。正是应用于 CSS 选择器的一组规则决定了将哪种样式应用于元素。

为了更好地理解这一点，我们必须理解一个相关的主题 CSS 中的级联。

## CSS 的级联性质

CSS 意味着层叠样式表。“级联”意味着 CSS 规则应用于元素**的顺序**实际上很重要。

理想情况下，如果两个规则应用于同一个元素，那么最后出现的规则将被使用。我们用一个例子来理解这一点。

我们将对一个元素应用两个类，并给每个类一个不同的`background-color`。

```
<p class="style1 style2"> This is a test paragraph</p>
```

这是 CSS:

```
.style2 {
  background-color: red;
}

.style1 {
  background-color: yellow;
}
```

这是结果: