# CSS 下拉指南:如何在 CSS 中制作下拉菜单

> 原文：<https://www.freecodecamp.org/news/css-dropdown-guide-how-to-make-a-dropdown-menu-in-css/>

## **什么是下拉菜单？**

CSS 中使用下拉菜单来隐藏按钮中的预定义列表。

示例:

```
<div class="dropdown">
  <button class="dropbtn">Name</button>
  <div class="dropdownContent">
    <a href="#">One</a>
    <a href="#">Two</a>
    <a href="#">Three</a>
  </div>
</div>
```

然后你应该像这样定制 CSS 中的类:

```
.dropdown {
  position: relative;
  display: inline-block;
}

.dropbtn {
  background-color: red;
  padding: 10px;
}

.dropdown-content {
  display: none;
  position: absolute;
}

.dropdown:hover .dropdown-content {
  display:block;
}
```

您需要单独的 div 类来创建按钮，并需要另一个 div 来分隔按钮包含的内容列表。

### 一个例子

```
<div id="container">

        <div id="myNav1" class="overlay">

            <div class="overlay-content" id="myNav1-content">

                <div>
                    <a href="#" id="list1_obj1" class="list1" >Content 1</a>
                </div>
                <div>
                    <a href="#" id="list1_obj2" class="list1" >Content 2</a>
                </div>

            </div>

         </div>

         <div id="myNav2" class="overlay">

             <a href="javascript:void(10)" class="closebtn" onclick="closeNav()">&times</a>
            <div class="overlay-content" id="myNav2-content">

                <div>
                    <a href="#" id="list2_obj1" class="list2" >Content 3</a>
                </div>
                <div>
                   <a href="#" id="list2_obj2" class="list2" >Content 4</a>
                </div>
                <div>
                  <a href="#" id="list2_obj3" class="list2" >Content 5</a>
                </div>

            </div>

         </div>

 </div>
```

```
#myNav1 {
    height: 0;
    width: 50%;
    position: fixed;
    z-index: 6;
    top: 0;
    left: 0;
    background-color: #ffff;
    overflow: hidden;
    transition: 0.3s;
    opacity: 0.85;
}

#myNav2 {
    height: 0;
    width: 50%;
    position: fixed;
    z-index: 6;
    bottom: 0;
    right: 0;
    background-color: #ffff;
    overflow: hidden;
    transition: 0.3s;
    opacity: 0.85;
}

.overlay-content {
    position: relative;
    width: 100%;
    text-align: center;
    margin-top: 30px;
}

#myNav1-content{
    top: 12%;
    left: 5%;
    display: none;
}

#myNav2-content{
    top: 12%;
    right: 10%;
    display: none;   
}
```

## 关于 CSS 下拉菜单的更多信息:

*   [如何用 CSS 和 JavaScript 创建下拉菜单](https://www.freecodecamp.org/news/how-to-create-a-dropdown-menu-with-css-and-javascript/)