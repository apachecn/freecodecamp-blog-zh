# HTML 中的 DOCTYPE 声明是什么？

> 原文：<https://www.freecodecamp.org/news/what-is-the-doctype-declaration-in-html/>

HTML 文档类型声明，也称为`DOCTYPE`，是每个 HTML 或 XHTML 文档所需的第一行代码。`DOCTYPE`声明是对 web 浏览器的一个指令，指示页面是用什么版本的 HTML 编写的。这确保了不同的 web 浏览器以相同的方式解析网页。

在 HTML 4.01 中，`DOCTYPE`声明指的是文档类型定义(DTD)。DTD 定义了 XML 文档的结构和合法元素。因为 HTML 4.01 是基于标准通用标记语言(SGML)的，所以有必要在`DOCTYPE`声明中引用 DTD。

此外，HTML 4.01 的文档类型需要声明`strict`、`transitional`或`frameset` DTD，每种都有不同的用例，如下所述。

*   ****Strict DTD**** :用于那些*排除了*属性和元素的网页，W3C 预计这些属性和元素会随着 CSS 支持的增长而逐步淘汰
*   ****过渡性 DTD**** :用于包含属性和元素的*网页，W3C 预计这些属性和元素将随着 CSS 支持的增长而逐步淘汰*
*   ****框架集 DTD**** :用于有框架的网页

相比之下，HTML5 `DOCTYPE`的声明要简单得多:它不再需要引用 dtd，因为它不再基于 SGML。关于 HTML 4.01 和 HTML5 `DOCTYPE` s 的比较，请参见下面的示例。

### **例题**

HTML5 及更高版本的 Doctype 语法:

```
<!DOCTYPE html>
```

严格 HTML 4.01 的 Doctype 语法:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

过渡 HTML 4.01 的 Doctype 语法:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
```

框架集 HTML 4.01 的 Doctype 语法:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Frameset//EN" "http://www.w3.org/TR/html4/frameset.dtd">
```

## **历史**

在 HTML 形成的那几年，web 标准还没有达成一致。浏览器厂商会以他们想要的任何方式构建新功能。很少有人担心竞争的浏览器。

结果是网络开发者不得不选择一种浏览器来开发他们的网站。这意味着网站在不受支持的浏览器中表现不好。这种情况不能再继续下去了。

W3C(万维网联盟)编写了一套 Web 标准来处理这种情况。所有浏览器供应商和 web 开发人员都应该遵守这些标准。这将确保网站能够很好地跨浏览器呈现。

这些标准所要求的改变与一些现有的做法大相径庭。遵守这些标准会破坏现有的不符合标准的网站。

为了解决这个问题，供应商开始在他们的浏览器中编程渲染模式。Web 开发人员需要在 HTML 文档的顶部添加 doctype 声明。doctype 声明将告诉浏览器对该文档使用哪种呈现模式。

三种不同的呈现模式通常可以在不同的浏览器中使用。

*   ****全标准模式**** 根据 W3C web 标准呈现页面。
*   ****怪癖模式**** 以不符合标准的方式呈现页面。
*   ****几乎标准模式**** 接近全标准模式，但功能支持少量怪癖。

在 HTML5 的现代时代，web 标准在所有主流浏览器中都得到了充分的实现。网站通常是以符合标准的方式开发的。因此，HTML5 doctype 声明的存在只是为了告诉浏览器以完全标准模式呈现文档。

## **用途**

Doctype 声明必须是 HTML 文档中除注释之外的第一行代码，如果需要，注释可以放在它的前面。对于现代 HTML5 文档，doctype 声明应该如下所示:

`<!DOCTYPE html>`

#### **更多信息:**

虽然不再被普遍使用，但以前版本的 HTML 中还有其他几种 doctype 声明类型。XML 文档也有特定的版本。要阅读更多关于它们的内容，并查看每一个的代码示例，请看一下维基百科文章。

[W3 的一张纸条](https://www.w3.org/QA/Tips/Doctype)

[MDN 术语表条目](https://developer.mozilla.org/en-US/docs/Glossary/Doctype)

[w3 学校](https://www.w3schools.com/tags/tag_doctype.asp)

[快速解释“怪癖模式”和“标准模式”](https://developer.mozilla.org/en-US/docs/Quirks_Mode_and_Standards_Mode)