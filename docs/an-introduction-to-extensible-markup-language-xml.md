# 可扩展标记语言(XML)简介

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-extensible-markup-language-xml/>

XML 代表可扩展标记语言。它是可扩展的，因为与 HTML 不同，它不使用一组预定义的标签来标识结构组件。相反，它为用户自己定义标签提供了一种机制。

XML 旨在简化数据共享和数据传输，并侧重于以逻辑方式组织信息。

## **XML 的语法**

XML 的语法非常简单，很容易学习。

XML 文档必须包含一个根元素，它是所有其他元素的父元素:

```
<root>
  <child>
    <subchild>.....</subchild>
  </child>
</root>
```

上面的语法显示了创建 XML 代码时必需的根元素。

但是根元素可以被称为任何东西。例如:

```
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>
```

在上面的代码中,`<note>`是根元素。

使用 XML 的优势:

*   简单性——XML 文档是普通的文本文件，可以用任何文本编辑器创建和编辑。
*   供应商独立性
*   独立于平台
*   庞大的基础设施

使用 XML 的缺点:

*   冗长而繁琐的语法
*   非常低效的存储