# 目标 HTML 属性解释

> 原文：<https://www.freecodecamp.org/news/the-a-target-html-attribute-explained/>

`<a target>`属性指定在`a`(锚)标签中打开链接文档的位置。

值为“_blank”的目标属性会在新窗口或选项卡中打开链接的文档。

```
 <a href="https://www.freecodecamp.org" target="_blank">freeCodeCamp</a>
```

值为“_self”的目标属性会在单击链接的文档时在同一框架中打开该文档(这是默认设置，通常不需要指定)。

```
 <a href="https://www.freecodecamp.org" target="_self">freeCodeCamp</a>
```

```
 <a href="https://www.freecodecamp.org">freeCodeCamp</a>
```

值为“_parent”的目标属性在父框架中打开链接的文档。

```
 <a href="https://www.freecodecamp.org" target="_parent">freeCodeCamp</a>
```

值为“_top”的目标属性在整个窗口体中打开链接的文档。

```
 <a href="https://www.freecodecamp.org" target="_top">freeCodeCamp</a>
```

值为 *"* framename *"* 的目标属性在指定的命名框架中打开链接的文档。

```
 <a href="https://www.freecodecamp.org" target="framename">freeCodeCamp</a>
```

## 相关链接:

*   [<a href>属性](https://guide.freecodecamp.org/html/attributes/a-href-attribute)
*   [关于 HTML 属性的一般信息](https://guide.freecodecamp.org/html/attributes)