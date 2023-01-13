# 注释掉 HTML–代码示例

> 原文：<https://www.freecodecamp.org/news/comment-out-html-code-example/>

在代码中添加注释是一个很好的做法，因为它使代码更具可读性和可理解性。

注释不会运行，因为它们被编译器和解释器忽略了。

在本文中，我将向您展示如何注释掉您的 HTML 代码，这样您就可以帮助自己和团队成员理解您在用它做什么。

## 如何注释掉 HTML 代码

不像其他编程语言有不同的符号来做多行和单行注释，你可以在 HTML 中用相同的符号做多行和单行注释。

要注释掉 HTML 代码，请使用小于符号(`<`)，后跟一个感叹号(`!`)、两个连字符(`--`)、注释、两个连字符(`--`，以及大于号(`>`)。

注释掉 HTML 的语法如下所示:

```
<!-- comments here --> 
```

您可以将它用于单行注释:

```
 <!-- This is a single line comment -->
    <section class="about" id="about">
      <h3>Watch the Jabs</h3>
      <p>
        Our primary objective is to bring live boxing matches to fans all around
        the world
      </p>

      <h3>Its not About the Fights Alone!</h3>
      <p>
        We also air documentaries specially made for the greats, lifestyle of
        boxers, news, and more.
      </p>
    </section> 
```

您也可以将它用于多行注释:

```
 <section class="about" id="about">
      <h3>Watch the Jabs</h3>
      <p>
        Our primary objective is to bring live boxing matches to fans all around
        the world
      </p>
      <!-- This is a 
        multi-line 
        comment -->
      <h3>Its not About the Fights Alone!</h3>
      <p>
        We also air documentaries specially made for the greats, lifestyle of
        boxers, news, and more.
      </p>
    </section> 
```

您也可以在任何想要的位置插入注释，只要它不在标签内:

```
 <section class="about" id="about">
      <h3>Watch the Jabs</h3>
      <p>
        Our primary objective is to bring live boxing matches to fans all around
        the world
      </p>
      <h3>Its not About the Fights Alone!</h3>
      <p>
        We also air
        <!-- This is a comment within some text -->
        documentaries specially made for the greats, lifestyle of boxers, news,
        and more.
      </p>
    </section> 
```

现在你可以正确安全地注释你的 HTML 代码了。

感谢您的阅读。