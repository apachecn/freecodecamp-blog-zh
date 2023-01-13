# CSS 字体系列和网页安全字体解释

> 原文：<https://www.freecodecamp.org/news/web-safe-fonts/>

## **网络安全字体**

Web 安全字体是包含在大多数操作系统中的字体，这种高可用性的含义是，设计者可以期望包含 web 安全字体的排版对大多数用户来说完全符合预期。下面是一些在写作时被认为是 web 安全的字体的非详尽列表，按 CSS 通用字体家族分类。

Web 安全衬线字体:

*   格鲁吉亚
*   时代新罗马

Web 安全无衬线字体:

*   天线
*   前面有突出的护架
*   投石机 MS
*   韦尔达纳

Web 安全等宽字体:

*   信使新闻

值得注意的是，即使您的设计只使用 web 安全字体，也应该使用带有后备选项的字体堆栈，包括通用字体系列。例如:

```
p {
  font-family: Tahoma, Arial, sans-serif;
}
```

#### **关于网络字体的说明**

仅仅因为一些字体比其他字体更安全并不意味着你应该限制你的设计只使用网页安全字体。使用 CSS 的现代设计也可以利用 web 字体来确保跨操作系统的一致排版。

## 关于字体的更多信息:

*   [如何正确加载网页字体](https://www.freecodecamp.org/news/web-fonts-in-2018-f191a48367e8/)
*   [谷歌字体超级家族](https://www.freecodecamp.org/news/low-hanging-design-fruit-why-you-should-use-google-font-superfamilies-1dae04b2fc50/)
*   [如何在你的下一个设计项目中使用谷歌字体](https://www.freecodecamp.org/news/how-to-use-google-fonts-in-your-next-web-design-project-e1ad48f1adfa/)