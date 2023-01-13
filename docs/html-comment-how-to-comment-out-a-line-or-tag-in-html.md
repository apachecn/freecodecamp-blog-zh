# HTML 注释–如何在 HTML 中注释掉一行或一个标签

> 原文：<https://www.freecodecamp.org/news/html-comment-how-to-comment-out-a-line-or-tag-in-html/>

在本文中，您将学习如何在 HTML 文档中添加单行和多行注释。

您还将看到为什么在编写 HTML 代码时注释被认为是一种好的实践。

我们开始吧！

## HTML 注释标签

HTML 注释的一般语法如下所示:

```
<!-- I am a comment! --> 
```

HTML 中的注释以`<!--`开始，以`-->`结束。

不要忘记标签开头的感叹号！但是不需要加在最后。

标签包围任何您想要注释掉的文本或其他 HTML 标签。

## 何时使用 HTML 注释

HTML 注释不会显示在浏览器中。这意味着当文档在 web 浏览器中呈现时，您添加到 HTML 源代码中的任何注释都不会显示出来。

也就是说，请记住，任何人都可以通过访问`View -> Developer -> View Source`查看互联网上发布的几乎所有网站的源代码，这也包括所有的评论！

因此，如果您将 HTML 文档公开，其他人就可以看到您的注释，并且他们会选择查看源代码。

写注释是有帮助的，并且在编写源代码时，这是一个很好的实践。注释有助于您记录代码和思考过程，并与自己(和他人)交流。它还会提醒你，当你几个月没做一个项目后回到这个项目时，你在想什么/做什么。

注释还有助于您与和您一起工作的其他开发人员交流。你的注释可以清楚地向他们解释为什么你添加了某些代码行。

## 如何在 HTML 中编写单行注释

单行注释只跨越一行。如前所述，这一行不会显示在浏览器中。

当您想要解释和阐明代码背后的目的，或者当您想要像这样为自己添加提醒时，请使用单行注释:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- Add the navbar here -->
    <h2>About me</h2>
    <p>I am learning to code with freeCodeCamp.</p>
    <p>I am going through each and every one of their awesome and super helpful  certifications. </p>
    <p>I am on my way to becoming a fullstack web developer!</p>
    <h3>Work Experience</h3>
</body>
</html> 
```

当你想弄清楚标签的结束位置时，单行注释也很有帮助。这在一个长而复杂的 HTML 文档中非常方便，在这个文档中有很多内容，您可能会对结束标记的位置感到困惑。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <section class="contact">
    </section> <!--closing tag of contact section is here-->
</body>
</html> 
```

## 如何在 HTML 中编写内联注释

您还可以在一句话或一行代码中间添加注释。

只有`<!-- -->`中的文本会被注释掉，标签中的其他文本不会受到影响。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>I am <!--going to be--> a web developer</p>
</body>
</html> 
```

## 如何在 HTML 中编写多行注释

注释也可以跨多行，使用的语法与您目前看到的完全相同。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <p>I am  a web developer</p>
    <!--This is going to be my portfolio.
    It will show off the projects that I'm most proud of.
    I could go on and on about what I want to add
    because I am writing a multi-line comment here-->
</body>
</html> 
```

## 如何在 HTML 中注释掉标签

那么如果你想在 HTML 中注释掉一个标签呢？

您在`<!-- -->`中包装您选择的标签，就像这样:

```
 !DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h2>My portfolio page</h2>
    <h3>freeCodeCamp certification projects</h3>
    <!-- <section class="hero">
    </section>  -->
    <h2>About Me</h2>
</body>
</html> 
```

注释掉标记有助于调试。

当事情没有按照预期的方式或你希望的方式运行时，开始一个接一个地注释掉单独的标签。这使您可以测试它们，并查看是哪一个导致了问题。

## 用于添加 HTML 注释的键盘快捷键

你可以使用一些快捷方式来添加评论——你可能会经常使用它们。Mac 用户的快捷方式是`Command /`，Windows 和 Linux 用户的快捷方式是`Control /`。

要添加单行注释，只需在代码编辑器中按住上面显示的组合键。那么你所在的整行都会被注释掉。请记住，因为所有内容都将在那一行被注释掉，所以这只适用于单行注释。您需要手动添加内嵌注释。

要添加多行注释，请选择并突出显示您想要注释掉的所有文本或标签，并按住前面显示的两个键。您选择的每一行现在都有了注释。

## 结论

好了，现在你知道如何以及为什么在 HTML 中使用注释了！

通过观看 freeCodeCamp 的 YouTube 频道上的以下视频，了解更多关于 HTML 的知识:

*   [HTML 教程-网站初学者速成班](https://www.youtube.com/watch?v=916GWv2Qs08)
*   [HTML 全教程——搭建网站教程](https://www.youtube.com/watch?v=pQN-pnXPaVg)

freeCodeCamp 还提供免费的基于项目的认证，认证内容是关于[响应式网页设计](https://www.freecodecamp.org/learn/responsive-web-design/)。

这是完全初学者的理想选择，不需要任何先前的知识。你将从绝对必要的基础开始，并随着你的进步建立你的技能。最后，你将完成五个项目。

感谢阅读，快乐学习:)