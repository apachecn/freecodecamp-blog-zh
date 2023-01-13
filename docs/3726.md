# HTML 注释:如何注释掉你的 HTML 代码

> 原文：<https://www.freecodecamp.org/news/html-comments-how-to-comment-out-your-html-code/>

## **HTML 中的注释**

评论标签是用来留下注释的元素，大多与项目或网站相关。这个标签经常被用来解释代码中的一些东西或者留下一些关于项目的建议。comment 标签也使得开发人员在以后的阶段更容易回来理解他所写的代码。注释也可以用于注释掉代码行，以便进行调试。

向代码中添加注释是一种很好的做法，尤其是在团队或公司工作时。

注释以`<!--`开头，以`-->`结尾，可以跨多行。它们可以包含代码或文本，当用户访问页面时，它们不会出现在网站的前端。您可以通过检查器控制台或查看页面源代码来查看注释。

### **例子**

```
<!-- You can comment out a large number of lines like this.
Author: xyz
Date: xx/xx/xxxx
Purpose: abc
-->
Read more: https://html.com/tags/comment-tag/#ixzz4vtZHu5uR
<!DOCTYPE html>
<html>
	<body>
		<h1>FreeCodeCamp web</h1>
		<!-- Leave some space between the h1 and the p in order to understand what are we talking about-->
		<p>FreeCodeCamp is an open-source project that needs your help</p>
	        <!-- For readability of code use proper indentation -->
	</body>
</html>
```

## **条件注释**

条件注释定义了当满足某个条件时执行的 HTML 标签。

只有 Internet Explorer 版本 5 到版本 9 - IE5 - IE9 才能识别条件注释。

### **例子**

```
<!DOCTYPE html>
<html>
	<body>
		<!--[if IE 9]>
    			<h1>FreeCodeCamp web</h1>
			<p>FreeCodeCamp is an open-source project that needs your help</p>	
		<![endif]-->
	</body>
</html>
```

### **IE 条件注释**

这些注释仅在 Internet Explorer 中可用，最高可用于 IE9。在目前的时代，有一个好的变化，你将永远看不到他们，但知道他们的存在是很好的。条件注释是为不同的客户端浏览器提供不同体验的一种方式。例如:

```
<!--[if lt IE 9]> <p>Your browser is lower then IE9</p> <![endif]-->     
<!--[if IE 9]> <p>Your browser is IE9</p> <![endif]-->
<!--[if gt IE 9]> <p>Your browser is greater then IE9</p> <![endif]-->
```

## 关于 HTML 的更多信息:

*   HTML 初学者指南
*   [最佳 HTML 和 HTML5 教程](https://www.freecodecamp.org/news/best-html-html5-tutorial/)
*   [HTML 手册](https://www.freecodecamp.org/news/the-html-handbook/)