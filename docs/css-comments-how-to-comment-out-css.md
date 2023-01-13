# CSS 注释–如何注释掉 CSS

> 原文：<https://www.freecodecamp.org/news/css-comments-how-to-comment-out-css/>

注释是任何编程语言不可或缺的一部分，CSS 也不例外。

如果你有一个非常大的项目或者你在一个团队中工作，那么你需要通过添加注释来帮助其他人更好地理解你的 CSS 样式表。

由于样式表随着时间的推移会变得复杂和冗长，所以在 CSS 代码中添加注释是一个有用的约定，您应该遵守。

这篇文章将向你展示如何在 CSS 中添加行内和多行注释。

## 如何注释掉 CSS

正斜杠(`/`)和星号(`*`)是注释掉一行或多行 CSS 所需要的。但是你是怎么做到的呢？

要在 CSS 中添加行内和多行注释，可以用正斜杠和星号(`/*`)开始，用星号和正斜杠(`*/`)结束注释。

CSS 中的行内注释是这样的:

```
/* This is an inline comment in CSS */ 
```

这是多行注释的样子:

```
/* 
This 
is
a 
multi-line
comment
in 
CSS
*/ 
```

您可以注释掉不想运行的一行或多行 CSS:

```
/* .email-sub {
  padding: 0.2rem;
  border: 1px solid var(--primary-color);
  border-radius: 4px;
}

.email-sub:focus {
  border: 1px solid var(--secondary-color);
  outline: none;
} */ 
```

您可以用注释为网页的某一部分指定样式的开始和结束:

```
/* Hero section starts */
.hero {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.9rem;
  max-width: 1100px;
  margin: 2rem auto -6rem;
}
/* Hero section ends */ 
```

您还可以使用注释向 CSS 添加注释:

```
/* 
Don't override this style if you don't know what you are doing. Otherwise, CSS might give you a kick in the butt
*/ 
```

## 结论

从长远来看，在 CSS 中添加注释可以帮助您记住编写代码时正在做什么。

此外，当您以正确的方式添加注释时，如果您已经很长时间没有查看代码了，那么开始一个项目会更容易。

要了解如何用注释来组织你的 CSS，你应该阅读这篇文章。