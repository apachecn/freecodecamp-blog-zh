# 如何使用 HTML、CSS、JavaScript 和 Bootstrap 5 创建作品集网站

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-portfolio-website-using-html-css-javascript-and-bootstrap/>

如果你是一个网页开发者或者网页设计师，拥有一个作品集网站对你来说是必不可少的。它让你提供关于你自己的信息，展示你的相关技能和经验。

在这篇博文中，我将讨论创建作品集网站的一些好处。然后我会告诉你如何使用 HTML，CSS，JavaScript 和 Bootstrap 第 5 版为自己创建一个漂亮的响应式作品集网站。

## 目录

*   [拥有投资组合网站的好处](#benefits-of-having-a-portfolio-website)
*   [什么是自举](#what-is-bootstrap)？
*   [文件夹结构](#folder-structure)
*   [如何向您的文件夹添加导航菜单](#how-to-add-a-navigation-menu-to-your-portfolio)
*   [如何将英雄头像添加到作品集](#how-to-add-a-hero-header-to-the-portfolio)
*   [如何制作“关于”部分](#how-to-make-the-about-section)
*   [如何制作服务部分](#how-to-make-the-services-section)
*   [如何在页面滚动条上添加深色背景色](#how-to-add-dark-background-color-to-navbar-on-page-scroll)
*   [如何建立投资组合部分](#how-to-build-the-portfolio-section)
*   [如何构建联系人部分](#how-to-build-the-contact-section)
*   [如何构建页脚部分](#how-to-build-the-footer-section)
*   [添加最后的润色](#final-touches)
*   [结论](#conclusion)

## 拥有投资组合网站的好处

拥有一个投资组合网站有几个好处，包括:

*   它提供了一个展示你相关技能和经验的平台
*   它显示了你的个性
*   它让招聘经理找到你，而不是你去找他们
*   你很容易在谷歌这样的搜索引擎上被搜索到

## 什么是自举？

Bootstrap 是一个流行的前端 CSS 框架，用于开发响应性和移动友好的网站。Bootstrap 的最新版本是版本 5。你可以在这里找到 Bootstrap 5 [的官方文档。](https://getbootstrap.com/docs/5.0/getting-started/introduction/)

## 文件夹结构

我们现在将开始创建作品集网站。

首先，让我们创建文件夹结构。你可以在 [GitHub](https://github.com/SampurnaC/portfolio_website_fcc/tree/portfolio-starter-files) 上获得项目启动文件。此外，你可以访问[这里](https://brad-portfolio.netlify.app/)看到这个项目的现场演示。

![Screenshot-from-2022-01-22-19-10-25](img/5ed2da90c3c565f5aa683f6831b66ea5.png)

Project Folder Structure

文件夹结构由 index.html、style.css 和 script.js 文件以及一个 images 文件夹组成。我们将在 style.css 文件中编写所有 CSS，在 script.js 文件中编写 JavaScript。

在 index.html 文件中，您可以看到带有引导 CDN、 [font awesome kit](https://fontawesome.com/) 的 HTML 样板代码，以及到外部样式表和 JavaScript 的链接。

这里，script.js 文件是在加载完所有 HTML 代码后加载的。

## 如何将导航菜单添加到您的文件夹中

现在，让我们在项目中添加一个导航菜单。它将帮助访问者找到他们正在寻找的相关信息。

我们将在 nav 元素中使用 Bootstrap 的`fixed-top`类来保持导航栏在页面顶部。navbar 也有一个`navbar-brand`类，我们将这个人的名字作为一个品牌。

```
<nav class="navbar navbar-expand-lg fixed-top navbarScroll">
        <div class="container">
            <a class="navbar-brand" href="#">Brad</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item active">
                        <a class="nav-link" href="#home">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#about">About</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#services">Services</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#portfolio">Portfolio</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#contact">Contact</a>
                    </li>
                </ul>

            </div>
        </div>
    </nav>
```

导航栏具有以下功能:

*   它有六个链接:主页、关于、服务、投资组合、联系人和页脚
*   它有一个透明的背景。稍后我们将在页面滚动上添加一个黑色背景。
*   它能打开更小的设备

你可以在这里找到关于 Bootstrap 5 导航条功能[的更多细节。](https://getbootstrap.com/docs/5.0/components/navbar/)

但是，导航条在滚动时有一个问题。它在整个页面中是完全透明的，这导致了可读性问题。我们将在完成[服务部分](#how-to-make-the-services-section)后修复此问题，以使您正确理解此问题。

## 如何在文件夹中添加英雄头像

现在，我们将添加一个英雄图像，并在中间添加一些文字。英雄形象是一个网页设计术语，指的是展示公司或个人主要目标的高质量全幅图像、代表性图像、照片或其他引人注目的元素。它有助于吸引用户到您的网站。

```
 <!-- main banner -->
    <section class="bgimage" id="home">
        <div class="container-fluid">
            <div class="row">
            <div class="col-lg-12 col-md-12 col-sm-12 col-xs-12 hero-text">
                <h2 class="hero_title">Hi, it's me Brad</h2>
                <p class="hero_desc">I am a professional freelancer in New York City</p>
            </div>
            </div>
        </div>
    </section>
```

此外，让我们在 style.css 文件中添加上述代码的 CSS:

```
/* hero background image */
.bgimage {
    height:100vh;
    background: url('images/heroImage.jpeg');
    background-size:cover;
    position:relative;
}
/* text css above hero image*/
.hero_title {
    font-size: 4.5rem;
}
.hero_desc {
    font-size: 2rem;
}
.hero-text {
    text-align: center;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
}
```

这里我们可以看到该部分有一个名为`bgimage`的 id，它负责显示背景英雄全幅图像。在上面的 CSS 的帮助下，它还在背景图像上方的中心显示一些文本。

这是该网站目前为止的样子，包括导航条和英雄部分:

![Screenshot-from-2022-01-25-10-13-25](img/59041d43f4ff1c3302a1494c99699ecf.png)

Hero Image with Navbar

## 如何制作“关于”部分

“关于”页面包含关于您和您的背景的重要信息。您的作品集网站的访问者可以通过您在本页提供的信息来了解您。

我们将在这一行的左侧添加一个图像，在右侧添加我们在这一部分的快速介绍。让我们用下面的代码来演示一下:

```
<!-- about section-->
    <section id="about">
        <div class="container mt-4 pt-4">
            <h1 class="text-center">About Me</h1>
            <div class="row mt-4">
                <div class="col-lg-4">
                    <img src="images/about.jpeg" class= "imageAboutPage" alt="">
                </div>

                <div class="col-lg-8">
                    <p> Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged

                    </p>
                    <div class="row mt-3">
                        <div class="col-md-6">
                            <ul>
                                <li>Name: David Parker</li>
                                <li>Age: 28</li>
                                <li>Occupation: Web Developer</li>

                            </ul>
                        </div>
                        <div class="col-md-6">
                            <ul>
                                <li>Name: David Parker</li>
                                <li>Age: 28</li>
                                <li>Occupation: Web Developer</li>

                            </ul>
                        </div>
                    </div>
                    <div class="row mt-3">
                        <p> Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged.
                            Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged.
                        </p>
                    </div>
                </div>
            </div>
    </section> 
```

让我们为左侧图像添加一些 CSS:

```
/* about section image css */
.imageAboutPage {
    width: 100%;
}
```

这将创建一个“关于”部分。您可以根据您的用例修改内容。我们用容器类添加了名为`mt-4`和`pt-4`的类，这两个类会将边距顶部和填充顶部设置为 1.5 雷姆。

该行有两列。一个是用于显示图像的`col-lg-4`类，它将占据左边的一列，有一个大屏幕的 4 部分网格。

下一列被分配了一个`col-lg-8`类，它将占据右边的一列，有一个 8 部分网格，用于更大的屏幕。对于中小型屏幕，它们会相互重叠，我们可以在下面的 GIF 文件中看到:

![about](img/b87309177d6aa7d3a9cb737be21b13cf.png)

About Section

## 如何制作服务部分

这个部分有助于将网站访问者转化为潜在客户。在这里，你可以解释你提供的具体服务，以及你提供的服务的定位。

让我们为这一部分添加代码，并在下面进行描述:

```
<!-- services section-->
    <section id="services">
        <div class="container">
            <h1 class="text-center">Services</h1>
            <div class="row">
                <div class="col-lg-4 mt-4">
                    <div class="card servicesText">
                        <div class="card-body">
                            <i class="fas servicesIcon fa-clock"></i>
                            <h4 class="card-title mt-3">Website Development</h4>
                            <p class="card-text mt-3">Some quick example text to build on the card title and make up the bulk of the card's content.
                                Some quick example text to build on the card title and make up the bulk of the card's content.
                            </p>
                        </div>
                    </div>  
                </div>
                <div class="col-lg-4 mt-4">
                    <div class="card servicesText">
                        <div class="card-body">
                            <i class='fas servicesIcon fa-layer-group'></i>
                            <h4 class="card-title mt-3">Website Design</h4>
                            <p class="card-text mt-3">Some quick example text to build on the card title and make up the bulk of the card's content.
                                Some quick example text to build on the card title and make up the bulk of the card's content.
                            </p>
                        </div>
                    </div>  
                </div>

                <div class="col-lg-4 mt-4">
                    <div class="card servicesText">
                        <div class="card-body">
                            <i class='far servicesIcon fa-check-circle'></i>
                            <h4 class="card-title mt-3">Website Deployment</h4>
                            <p class="card-text mt-3">Some quick example text to build on the card title and make up the bulk of the card's content.
                                Some quick example text to build on the card title and make up the bulk of the card's content.
                            </p>
                        </div>
                    </div>  
                </div>
            </div>

            <div class="row">
                <div class="col-lg-4 mt-4">
                    <div class="card servicesText">
                        <div class="card-body">
                            <i class='fas servicesIcon fa-search'></i>
                            <h4 class="card-title mt-3">SEO</h4>
                            <p class="card-text mt-3">Some quick example text to build on the card title and make up the bulk of the card's content.
                                Some quick example text to build on the card title and make up the bulk of the card's content.
                            </p>
                        </div>
                    </div>  
                </div>

                <div class="col-lg-4 mt-4">
                    <div class="card servicesText">
                        <div class="card-body">
                            <i class='fas servicesIcon fa-shield-alt'></i>
                            <h4 class="card-title mt-3">DevOps</h4>
                            <p class="card-text mt-3">Some quick example text to build on the card title and make up the bulk of the card's content.
                                Some quick example text to build on the card title and make up the bulk of the card's content.
                            </p>
                        </div>
                    </div>  
                </div>

                <div class="col-lg-4 mt-4">
                    <div class="card servicesText">
                        <div class="card-body">
                            <i class='fas servicesIcon fa-wrench'></i>
                            <h4 class="card-title mt-3">QA</h4>
                            <p class="card-text mt-3">Some quick example text to build on the card title and make up the bulk of the card's content.
                                Some quick example text to build on the card title and make up the bulk of the card's content.
                            </p>
                        </div>
                    </div>  
                </div>
            </div>
        </div>
    </section>
```

因为这个网站是面向网页开发者和设计者的，我已经包括了一些网页开发者或设计者可能提供的服务。

我们已经使用引导卡来显示服务。我们的服务部分每行有 2 行 3 列。对于宽度大于或等于 992px 的大屏幕，一行显示三张卡片。对于宽度小于 992 像素的屏幕，一行中只显示一张卡片。

你可以在这里找到更多关于引导断点的信息。

还有，每张卡里都加了字体，让它们看起来更好看。

如果没有 CSS，服务部分将如下所示:

![Screenshot-from-2022-01-23-14-01-00](img/0584d1b49e72f35281d2bd52bde1fcf0.png)

因此，让我们添加一些 CSS 来增加字体图标字体大小和卡片高度，并在用户悬停在卡片上时添加一些额外的颜色。

```
/* services section css */
.servicesText.card {
    height: 280px;
    cursor: pointer;
  }
.servicesIcon {
    font-size: 36px;
    text-align: center;
    width: 100%;
}
.card-title {
    text-align: center;
}
.card:hover .servicesIcon {
    color: #008000;
}
.servicesText:hover {
    border: 1px solid #008000;
}
```

这是我们的服务部分现在的样子:

![services](img/cd27a8c1006432b11b005d14087846dc.png)

Services 

## 如何在页面滚动时给导航栏添加暗背景色

如果你仔细观察上面的 gif，你会发现导航条在整个页面上都是透明的，这导致了可读性问题。所以让我们努力解决这个问题。

我们将编写一些 JavaScript 和 CSS 来解决这个问题。我们将添加一个`navbarDark`类，以便在页面滚动时显示导航条的深色背景。

为此，我们需要转到 script.js 文件并添加以下代码:

```
// add class navbarDark on navbar scroll
const header = document.querySelector('.navbar');

window.onscroll = function() {
    var top = window.scrollY;
    if(top >=100) {
        header.classList.add('navbarDark');
    }
    else {
        header.classList.remove('navbarDark');
    }
}
```

现在，让我们分解上面的代码:

*   头保存 nav 元素的值，因为 querySelector 方法返回第一个匹配 CSS 选择器的元素(在本例中是`.navbar`)。
*   当滚动事件发生时触发。
*   `window.scrollY`返回文档垂直滚动的像素数，其值赋给一个名为`top`的变量。
*   如果`top`的值大于或等于 100，它会在头中添加一个类`navbarDark`。

让我们快速为`navbarDark`类添加 CSS。为此，转到 style.css 文件并添加以下代码:

```
/* display background color black on navbar scroll */
.navbarScroll.navbarDark {
    background-color: black;
}
```

这是导航条现在的样子:

![navbar](img/5d192311a9f5448df3b16f1476fd58fe.png)

Dark background color on Navbar on page scroll

## 如何建立投资组合部分

这部分包括你最好的作品。人们可以看到你有能力做什么，展示过去的工作肯定会吸引更多的潜在客户或招聘人员。所以这部分只加你最好的作品。

我们将使用自举卡来展示投资组合项目。将有 2 行，每行将有 3 列卡片。

这将是投资组合部分的代码:

```
<!-- portfolio section-->
    <section id="portfolio">
        <div class="container mt-3">
            <h1 class="text-center">Portfolio</h1>
            <div class="row">
                <div class="col-lg-4 mt-4">
                    <div class="card">
                        <img class="card-img-top" src="images/portfolioImage1.jpg" alt="Card image" style="width:100%">
                        <div class="card-body">
                            <h4 class="card-title">YouTube Clone</h4>
                            <p class="card-text">Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
                            <div class="text-center">
                                <a href="#" class="btn btn-success">Link</a>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="col-lg-4 mt-4">
                    <div class="card portfolioContent">
                        <img class="card-img-top" src="images/portfolioImage4.jpg" alt="Card image" style="width:100%">
                        <div class="card-body">
                            <h4 class="card-title">Quiz App</h4>
                            <p class="card-text">Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
                            <div class="text-center">
                                <a href="#" class="btn btn-success">Link</a>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="col-lg-4 mt-4">
                    <div class="card portfolioContent">
                        <img class="card-img-top" src="images/portfolioImage3.jpg" alt="Card image" style="width:100%">
                        <div class="card-body">
                            <h4 class="card-title">Product Landing Page</h4>
                            <p class="card-text">Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
                            <div class="text-center">
                                <a href="#" class="btn btn-success">Link</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <br>
            <div class="row">
                <div class="col-lg-4 mt-4">
                    <div class="card portfolioContent">
                        <img class="card-img-top" src="images/portfolioImage4.jpg" alt="Card image" style="width:100%">
                        <div class="card-body">
                            <h4 class="card-title">Messaging Service</h4>
                            <p class="card-text">Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
                            <div class="text-center">
                                <a href="#" class="btn btn-success">Link</a>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="col-lg-4 mt-4">
                    <div class="card portfolioContent">
                        <img class="card-img-top" src="images/portfolioImage1.jpg" alt="Card image" style="width:100%">
                        <div class="card-body">
                            <h4 class="card-title">Twitter Clone</h4>
                            <p class="card-text">Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
                            <div class="text-center">
                                <a href="#" class="btn btn-success">Link</a>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="col-lg-4 mt-4">
                    <div class="card portfolioContent">
                        <img class="card-img-top" src="images/portfolioImage4.jpg" alt="Card image" style="width:100%">
                        <div class="card-body">
                            <h4 class="card-title">Blog App</h4>
                            <p class="card-text">Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p>
                            <div class="text-center">
                                <a href="#" class="btn btn-success">Link</a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
    </section>
```

每张卡片都有图片、标题、描述和项目链接。对于断点宽度≥992 像素的大屏幕，一行显示三张卡，但对于宽度< 992 像素的屏幕，一行仅显示一张卡。

下面的 GIF 显示了投资组合部分现在的样子:

![portfolio](img/76de0c90919fa220d63a2e7a98052d07.png)

Portfolio

## 如何构建联系人部分

你应该在这一部分包括你的联系信息，这样如果访问者想雇佣你，他们可以联系你。

我们的联系人部分将在一行中包括 2 列:谷歌地图的位置和联系方式。

为了嵌入谷歌地图，你需要遵循以下步骤:

*   前往[https://www.embed-map.com](https://www.embed-map.com/)
*   输入您的位置
*   点击**生成 HTML 代码**按钮，这将提供您的谷歌地图 HTML 代码

我们的代码将如下所示，包括联系表单:

```
<!-- contact section-->
    <section id="contact">
        <div class="container mt-3 contactContent">
            <h1 class="text-center">Contact Me</h1>

            <div class="row mt-4">
                <div class="col-lg-6">
                    <!-- to edit google map goto https://www.embed-map.com type your location, generate html code and copy the html  -->
                    <div style="max-width:100%;overflow:hidden;color:red;width:500px;height:500px;">
                        <div id="embedmap-canvas" style="height:100%; width:100%;max-width:100%;">
                            <iframe style="height:100%;width:100%;border:0;" frameborder="0" src="https://www.google.com/maps/embed/v1/place?q=new+york&key=AIzaSyBFw0Qbyq9zTFTd-tUY6dZWTgaQzuU17R8">
                            </iframe>
                        </div>
                        <a class="googlemaps-html" href="https://www.embed-map.com" id="get-data-forembedmap">https://www.embed-map.com</a>
                        <style>#embedmap-canvas img{max-width:none!important;background:none!important;font-size: inherit;font-weight:inherit;}
                        </style>
                    </div>
                </div>

                <div class="col-lg-6">
                    <!-- form fields -->
                    <form>
                        <input type="text" class="form-control form-control-lg" placeholder="Name">
                        <input type="email" class="form-control mt-3" placeholder="Email">
                        <input type="text" class="form-control mt-3" placeholder="Subject">
                        <div class="mb-3 mt-3">
                            <textarea class="form-control" rows="5" id="comment" name="text" placeholder="Project Details"></textarea>
                        </div>
                    </form>
                    <button type="button" class="btn btn-success mt-3">Contact Me</button>

                </div>

            </div>
        </div>
    </section>
```

第一列将显示谷歌地图，下一列将显示联系方式。

该表单有四个不同的表单字段:名称、电子邮件、主题和项目详细信息。表单本身不会提交请求。你需要把它和任何后端语言连接起来。或者，您可以简单地使用 [Netlify 表单](https://www.netlify.com/products/forms/)或[表单 spree 表单](https://formspree.io/) 来实现这一点。

联系人部分将显示如下:

![Screenshot-from-2022-01-25-11-31-56](img/0a72f905d3172751870f83f9f40e0f65.png)

Contact Section

## 如何构建页脚部分

现在我们来到这篇文章的最后一部分，也就是页脚部分。我们已经在 index.html 文件中添加了一个链接到字体 awesome CDN。

在页脚，我们将添加链接到我们的社交媒体通过字体真棒图标。

```
 <!-- footer section-->
    <footer id="footer">
        <div class="container-fluid">
            <!-- social media icons -->
            <div class="social-icons mt-4">
                <a href="https://www.facebook.com/" target="_blank"><i class="fab fa-facebook"></i></a>
                <a href="https://www.instagram.com/" target="_blank"><i class="fab fa-instagram"></i></a>
                <a href="https://www.twitter.com/" target="_blank"><i class="fab fa-twitter"></i></a>
                <a href="https://www.linkedin.com/" target="_blank"><i class="fab fa-linkedin"></i></a>
                <a href="https://www.twitch.tv/" target="_blank"><i class="fab fa-twitch"></i></a>
            </div>
        </div>
    </footer>
```

如果没有 CSS，我们的页脚将如下所示:

![Screenshot-from-2022-01-23-17-56-37](img/bd02b7bebb048e837a2630b757f8c7a9.png)

footer without styling

所以让我们用下面的代码给页脚添加一些样式:

```
/* social media icons styling */
.social-icons {
    font-size: 36px;
    cursor: pointer;
}
.fa-facebook:hover,.fa-instagram:hover,.fa-twitter:hover,.fa-linkedin:hover, .fa-twitch:hover {
    color: #008000;
}
.fab {
    color: #000000;
}
/* footer styling */
#footer {
    background-color: #808080;
    text-align: center;
}
```

图标现在以悬停效果显示在中间，我们可以在下面的 GIF 文件中看到。

![footer](img/40db4aca811d0cb964f813f0abfd8dd2.png)

Footer

## 最后润色

为了在所有部分之间增加一些间距，让我们添加一些更多的样式:

```
/* spacing on all sections */
#about, #services, #portfolio, #contact {
    margin-top: 4rem;
    padding-top: 4rem;
}
#contact {
    padding-bottom: 4rem;
}
```

现在我们完成了我们完整的投资组合网站。

你可以在这里找到这个项目[的完整源代码。](https://github.com/SampurnaC/portfolio_website_fcc/tree/master)

## 结论

这就是如何使用 HTML、CSS、JavaScript 和 Bootstrap 5 创建一个完整的响应式作品集网站。

在这篇博文中，我们看到了为网站开发者和设计者创建作品集网站的一些好处。我们将整个网站分成不同的部分，并在构建时单独讨论每个部分。

您可以根据自己的使用案例定制这个网站。

我希望这篇文章对你有用。

编码快乐！

你可以在 Twitter 上找到我，了解关于网络开发的每日内容。