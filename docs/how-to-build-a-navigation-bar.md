# 如何构建一个导航栏

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-navigation-bar/>

## **导航栏**

导航栏对任何网站来说都是非常重要的元素。它们通过向用户提供主要的链接列表来提供主要的导航方法。创建导航栏的方法有很多。创建导航栏最简单的方法是使用一个无序列表，并用 CSS 对其进行样式化。

导航栏大多是由水平排列的`<ul>`列表组成，并设置样式。

在设计导航栏的样式时，通常会删除由`<ul>`和`<li>`标签创建的额外间距以及自动插入的项目符号:

```
 list-style-type: none;
   margin: 0px;
   padding: 0px;
```

****例如:****

任何导航都有两个部分:HTML 和 CSS。这只是一个简单的例子。

```
<nav class="myNav">                                 <!-- Any element can be used here -->
    <ul>
        <li><a href="index.html">Home</a></li>
        <li><a href="about.html">About</a></li>
        <li><a href="contact.html">Contact</a></li>
    </ul>
</nav>
```

```
/* Define the main Navigation block */
.myNav {
    display: block;
    height: 50px;
    line-height: 50px;
    background-color: #333;
}
/* Remove bullets, margin and padding */
.myNav ul {
    list-style: none;
    padding: 0;
    margin: 0;
}
.myNav li {
    float: left;
    /* Or you can use display: inline; */
}
/* Define the block styling for the links */
.myNav li a {
    display: inline-block;
    text-align: center;
    padding: 14px 16px;
}
/* This is optional, however if you want to display the active link differently apply a background to it */
.myNav li a.active {
    background-color: #3786E1;
}
```