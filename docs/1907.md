# 如何用 React 建立作品集网站

> 原文：<https://www.freecodecamp.org/news/build-portfolio-website-react/>

今天，你将创建一个最重要的应用程序:你的开发者组合。

一般来说，每个 React 开发人员或 web 开发人员都需要能够向任何潜在的客户或雇主展示他们能做什么。

这正是我们现在要做的，借助于许多行业标准工具，包括 React、Tailwind CSS 和 Netlify。

我们开始吧！

## 投资组合会是什么样的？

![portfolio-1-min](img/5c1977aa1c0f761699ef3529f599c70d.png)

这是你将建立的投资组合的最终版本。

它将包括关于你自己的信息，你做了什么项目，你在做这些项目中使用了什么技能，还将包括一份联系客户或雇主的表格。

## 我们将使用什么工具？

*   我们将使用 React 来创建应用程序的用户界面。它将允许我们通过可重用的组件组成我们的着陆页面的每个部分，并且如果我们想添加额外的功能，如博客，可以扩展我们的应用程序。
*   为了设计我们的应用程序，我们将使用 Tailwind CSS。为了让我们的应用程序具有专业的外观，Tailwind 将允许我们通过在 React 元素上组合类名来轻松应用多种样式。
*   为了将我们的应用程序推向网络，我们将使用免费服务 Netlify。它将在 CDN(内容交付网络)的帮助下，非常快速地在自定义域上为用户提供我们的项目。

## 如何开始

您可以在这里下载我们项目的启动文件。

当您获取代码时，您所要做的就是将您的(解压缩的)项目文件夹拖到您的代码编辑器中，并运行命令:

```
npm install
```

你可以走了！

## 我需要什么工具来建立我的投资组合？

要完成从开始到部署的整个应用创建流程，您需要具备以下条件:

1.  Node.js 安装在您的计算机上。你可以在 nodejs.org 下载。
2.  Git 安装在你的电脑上。你可以在 git-scm.com 下载。
3.  我推荐使用 VS 代码作为你的代码编辑器。你可以在 code.visualstudio.com 下载。
4.  netlify.com 的免费网络账户。
5.  github.com 的免费 GitHub 账户。

## 如何构建投资组合结构

使用 React 的好处是，我们可以非常简单地将我们的应用程序扩展到任意多的页面，并添加许多额外的内容。

然而，由于我们只处理一个页面，我们可以在我们的应用程序组件中很快找出我们需要的不同组件。我们将在顶部有一个导航栏，所有的链接跳转到我们的投资组合的不同部分。

之后，我们将包括一个关于部分，我们的项目，证明，最后是我们的联系方式部分。

这种快速的计划使我们能够确定我们的组件应该以什么样的顺序命名。我们可以将它们全部添加到我们的 App.js 文件中(在 src 中):

```
// src/App.js

import React from "react";

export default function App() {
  return (
    <main>
      <Navbar />
      <About />
      <Projects />
      <Skills />
      <Testimonials />
      <Contact />
    </main>
  );
}
```

## 如何创建我们的组件

现在我们已经列出了所有这些组件，我们需要继续创建它们。

在 source (src)文件夹中，我们将创建一个名为 components 的文件夹，其中包含我们需要的所有文件:

```
my-portfolio
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.js
    ├── data.js
    ├── index.css
    ├── index.js
    └── components
        ├── About.js
        ├── Contact.js
        ├── Navbar.js
        ├── Projects.js
        ├── Skills.js
        └── Testimonials.js
```

然后我们将创建每个 React 组件的基本结构，并使用`export default`从该文件中导出它:

```
// src/components/About.js

export default function About() {}

// repeat the same basic structure for all 6 components
```

最后，确保将其导入 App.js:

```
// src/App.js

import React from "react";
import About from "./components/About";
import Contact from "./components/Contact";
import Navbar from "./components/Navbar";
import Projects from "./components/Projects";
import Skills from "./components/Skills";
import Testimonials from "./components/Testimonials";

export default function App() {
  return (
    <main>
      <Navbar />
      <About />
      <Projects />
      <Skills />
      <Testimonials />
      <Contact />
    </main>
  );
}
```

*注意，总共应该有六个组件。*

## 顺风 CSS 简介

一旦完成，我们就可以开始使用 Tailwind CSS，以便给我们的应用程序一个基本的外观。

使用 Tailwind CSS 的好处是，我们不必在 CSS 样式表中手动编写任何样式。我们所要做的就是组合多个类来创建我们想要的外观。

例如，要给我们的投资组合一个深色背景，灰色文本应用于我们所有的子组件，您可以将下面的类添加到我们的`main`元素中:

```
// src/App.js

import React from "react";
import About from "./components/About";
import Contact from "./components/Contact";
import Navbar from "./components/Navbar";
import Projects from "./components/Projects";
import Skills from "./components/Skills";
import Testimonials from "./components/Testimonials";

export default function App() {
  return (
    <main className="text-gray-400 bg-gray-900 body-font">
      <Navbar />
      <About />
      <Projects />
      <Skills />
      <Testimonials />
      <Contact />
    </main>
  );
}
```

## 如何构建 About 组件

让我们开始我们的第一部分，关于部分。这将包括对我们自己的基本介绍和我们擅长的技能。

它还将包括一些联系方式的链接，以及我们过去的项目。由于这些链接将指向同一个页面的不同部分，我们可以使用哈希值:“/#projects”和“/#contact”。

为了使这些链接起作用并能够跳转到每个部分，我们将把项目部分的`id`属性设置为“projects ”,把联系人部分的属性设置为“contact”。

```
// src/components/About.js

import React from "react";

export default function About() {
  return (
    <section id="about">
      <div className="container mx-auto flex px-10 py-20 md:flex-row flex-col items-center">
        <div className="lg:flex-grow md:w-1/2 lg:pr-24 md:pr-16 flex flex-col md:items-start md:text-left mb-16 md:mb-0 items-center text-center">
          <h1 className="title-font sm:text-4xl text-3xl mb-4 font-medium text-white">
            Hi, I'm Reed.
            <br className="hidden lg:inline-block" />I love to build amazing
            apps.
          </h1>
          <p className="mb-8 leading-relaxed">
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Qui
            laborum quasi, incidunt dolore iste nostrum cupiditate voluptas?
            Laborum, voluptas natus?
          </p>
          <div className="flex justify-center">
            <a
              href="#contact"
              className="inline-flex text-white bg-green-500 border-0 py-2 px-6 focus:outline-none hover:bg-green-600 rounded text-lg">
              Work With Me
            </a>
            <a
              href="#projects"
              className="ml-4 inline-flex text-gray-400 bg-gray-800 border-0 py-2 px-6 focus:outline-none hover:bg-gray-700 hover:text-white rounded text-lg">
              See My Past Work
            </a>
          </div>
        </div>
        <div className="lg:max-w-lg lg:w-full md:w-1/2 w-5/6">
          <img
            className="object-cover object-center rounded"
            alt="hero"
            src="./coding.svg"
          />
        </div>
      </div>
    </section>
  );
}
```

对于该部分右侧的图像，我使用的是来自`public`文件夹的 svg 文件 coding.svg。

该图像仅用作临时占位符。我强烈建议使用你自己的真实形象。

## 如何构建项目组件

我们的项目部分将由一个带有“项目”的`id`的`section`元素组成。这将具有我们已经建立的所有项目的画廊，其中将包括图像。

它将有项目的标题，以及我们用来制作它的技术，还有一个链接(如果它被部署的话)。

```
// src/components/Projects.js

import { CodeIcon } from "@heroicons/react/solid";
import React from "react";
import { projects } from "../data";

export default function Projects() {
  return (
    <section id="projects" className="text-gray-400 bg-gray-900 body-font">
      <div className="container px-5 py-10 mx-auto text-center lg:px-40">
        <div className="flex flex-col w-full mb-20">
          <CodeIcon className="mx-auto inline-block w-10 mb-4" />
          <h1 className="sm:text-4xl text-3xl font-medium title-font mb-4 text-white">
            Apps I've Built
          </h1>
          <p className="lg:w-2/3 mx-auto leading-relaxed text-base">
            Lorem ipsum, dolor sit amet consectetur adipisicing elit. Explicabo
            facilis repellat ab cupiditate alias vero aliquid obcaecati quisquam
            fuga dolore.
          </p>
        </div>
        <div className="flex flex-wrap -m-4">
          {projects.map((project) => (
            <a
              href={project.link}
              key={project.image}
              className="sm:w-1/2 w-100 p-4">
              <div className="flex relative">
                <img
                  alt="gallery"
                  className="absolute inset-0 w-full h-full object-cover object-center"
                  src={project.image}
                />
                <div className="px-8 py-10 relative z-10 w-full border-4 border-gray-800 bg-gray-900 opacity-0 hover:opacity-100">
                  <h2 className="tracking-widest text-sm title-font font-medium text-green-400 mb-1">
                    {project.subtitle}
                  </h2>
                  <h1 className="title-font text-lg font-medium text-white mb-3">
                    {project.title}
                  </h1>
                  <p className="leading-relaxed">{project.description}</p>
                </div>
              </div>
            </a>
          ))}
        </div>
      </div>
    </section>
  );
}
```

注意，我们还将使用库`@heroicons/react`来编写一些 SVG 图标作为 React 组件。

我们将从同一文件夹中的 data.js 文件导入一系列项目。在这里，我们导出一个对象数组，每个对象都包含一个项目的数据:

```
// src/data.js

export const projects = [
  {
    title: "React Reserve",
    subtitle: "MERN Stack",
    description:
      "Lorem ipsum dolor sit amet consectetur adipisicing elit. Praesentium dolore rerum laborum iure enim sint nemo omnis voluptate exercitationem eius?",
    image: "./project-1.gif",
    link: "https://reactbootcamp.com",
  },
  {
    title: "React Tracks",
    subtitle: "React and Python",
    description:
      "Lorem ipsum dolor sit amet consectetur adipisicing elit. Praesentium dolore rerum laborum iure enim sint nemo omnis voluptate exercitationem eius?",
    image: "./project-2.gif",
    link: "https://reedbarger.com",
  },
  {
    title: "DevChat",
    subtitle: "React and Firebase",
    description:
      "Lorem ipsum dolor sit amet consectetur adipisicing elit. Praesentium dolore rerum laborum iure enim sint nemo omnis voluptate exercitationem eius?",
    image: "./project-3.gif",
    link: "https://jsbootcamp.com",
  },
  {
    title: "Epic Todo App",
    subtitle: "React Hooks",
    description:
      "Lorem ipsum dolor sit amet consectetur adipisicing elit. Praesentium dolore rerum laborum iure enim sint nemo omnis voluptate exercitationem eius?",
    image: "./project-4.gif",
    link: "https://pythonbootcamp.com",
  },
];
```

## 如何构建技能组件

让我们填写我们所知道的所有技能和技术。

这将包括一个简单的主要工具的列表，我们都很熟悉，可以在我们的雇主或客户的项目中使用。

我们将再次从数据文件夹中导入一个数组。但是这个数组由许多字符串组成，这些字符串代表了我们所知道的每一种技能，比如 JavaScript、React 和 Node:

```
// src/components/Skills.js

import { BadgeCheckIcon, ChipIcon } from "@heroicons/react/solid";
import React from "react";
import { skills } from "../data";

export default function Skills() {
  return (
    <section id="skills">
      <div className="container px-5 py-10 mx-auto">
        <div className="text-center mb-20">
          <ChipIcon className="w-10 inline-block mb-4" />
          <h1 className="sm:text-4xl text-3xl font-medium title-font text-white mb-4">
            Skills &amp; Technologies
          </h1>
          <p className="text-base leading-relaxed xl:w-2/4 lg:w-3/4 mx-auto">
            Lorem ipsum dolor sit amet consectetur, adipisicing elit. Nisi sit
            ipsa delectus eum quo voluptas aspernatur accusantium distinctio
            possimus est.
          </p>
        </div>
        <div className="flex flex-wrap lg:w-4/5 sm:mx-auto sm:mb-2 -mx-2">
          {skills.map((skill) => (
            <div key={skill} className="p-2 sm:w-1/2 w-full">
              <div className="bg-gray-800 rounded flex p-4 h-full items-center">
                <BadgeCheckIcon className="text-green-400 w-6 h-6 flex-shrink-0 mr-4" />
                <span className="title-font font-medium text-white">
                  {skill}
                </span>
              </div>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

## 如何构建推荐组件

在推荐书部分，我们将列出一些推荐书，可能来自以前的客户或熟悉我们工作的人。

这些将由几张卡片组成，卡片上有客户评价本身，以及它来自谁和这个人来自哪个公司。

我们还导入了一个客户评价数组，其中包含许多对象，包括报价、图片、姓名和公司。

```
// src/components/Testimonials

import React from "react";
import { TerminalIcon, UsersIcon } from "@heroicons/react/solid";
import { testimonials } from "../data";

export default function Testimonials() {
  return (
    <section id="testimonials">
      <div className="container px-5 py-10 mx-auto text-center">
        <UsersIcon className="w-10 inline-block mb-4" />
        <h1 className="sm:text-4xl text-3xl font-medium title-font text-white mb-12">
          Client Testimonials
        </h1>
        <div className="flex flex-wrap m-4">
          {testimonials.map((testimonial) => (
            <div className="p-4 md:w-1/2 w-full">
              <div className="h-full bg-gray-800 bg-opacity-40 p-8 rounded">
                <TerminalIcon className="block w-8 text-gray-500 mb-4" />
                <p className="leading-relaxed mb-6">{testimonial.quote}</p>
                <div className="inline-flex items-center">
                  <img
                    alt="testimonial"
                    src={testimonial.image}
                    className="w-12 rounded-full flex-shrink-0 object-cover object-center"
                  />
                  <span className="flex-grow flex flex-col pl-4">
                    <span className="title-font font-medium text-white">
                      {testimonial.name}
                    </span>
                    <span className="text-gray-500 text-sm uppercase">
                      {testimonial.company}
                    </span>
                  </span>
                </div>
              </div>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

## 如何构建联系人组件

在我们登录页面的最后，我们将附上我们的联系表格，以便潜在雇主联系我们。

这个表单将有 3 个输入:一个名字，电子邮件和消息输入。

为了接收这些表单提交，我们将使用 Netlify Forms 工具来轻松保存这些消息。

```
// src/components/Contact.js

import React from "react";

export default function Contact() {
  return (
    <section id="contact" className="relative">
      <div className="container px-5 py-10 mx-auto flex sm:flex-nowrap flex-wrap">
        <div className="lg:w-2/3 md:w-1/2 bg-gray-900 rounded-lg overflow-hidden sm:mr-10 p-10 flex items-end justify-start relative">
          <iframe
            width="100%"
            height="100%"
            title="map"
            className="absolute inset-0"
            frameBorder={0}
            marginHeight={0}
            marginWidth={0}
            style={{ filter: "opacity(0.7)" }}
            src="https://www.google.com/maps/embed/v1/place?q=97+warren+st+new+york+city&key=AIzaSyBFw0Qbyq9zTFTd-tUY6dZWTgaQzuU17R8"
          />
          <div className="bg-gray-900 relative flex flex-wrap py-6 rounded shadow-md">
            <div className="lg:w-1/2 px-6">
              <h2 className="title-font font-semibold text-white tracking-widest text-xs">
                ADDRESS
              </h2>
              <p className="mt-1">
                97 Warren St. <br />
                New York, NY 10007
              </p>
            </div>
            <div className="lg:w-1/2 px-6 mt-4 lg:mt-0">
              <h2 className="title-font font-semibold text-white tracking-widest text-xs">
                EMAIL
              </h2>
              <a className="text-indigo-400 leading-relaxed">
                reedbarger@email.com
              </a>
              <h2 className="title-font font-semibold text-white tracking-widest text-xs mt-4">
                PHONE
              </h2>
              <p className="leading-relaxed">123-456-7890</p>
            </div>
          </div>
        </div>
        <form
          netlify
          name="contact"
          className="lg:w-1/3 md:w-1/2 flex flex-col md:ml-auto w-full md:py-8 mt-8 md:mt-0">
          <h2 className="text-white sm:text-4xl text-3xl mb-1 font-medium title-font">
            Hire Me
          </h2>
          <p className="leading-relaxed mb-5">
            Lorem ipsum dolor sit amet consectetur, adipisicing elit. Illum
            suscipit officia aspernatur veritatis. Asperiores, aliquid?
          </p>
          <div className="relative mb-4">
            <label htmlFor="name" className="leading-7 text-sm text-gray-400">
              Name
            </label>
            <input
              type="text"
              id="name"
              name="name"
              className="w-full bg-gray-800 rounded border border-gray-700 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-900 text-base outline-none text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
            />
          </div>
          <div className="relative mb-4">
            <label htmlFor="email" className="leading-7 text-sm text-gray-400">
              Email
            </label>
            <input
              type="email"
              id="email"
              name="email"
              className="w-full bg-gray-800 rounded border border-gray-700 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-900 text-base outline-none text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
            />
          </div>
          <div className="relative mb-4">
            <label
              htmlFor="message"
              className="leading-7 text-sm text-gray-400">
              Message
            </label>
            <textarea
              id="message"
              name="message"
              className="w-full bg-gray-800 rounded border border-gray-700 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-900 h-32 text-base outline-none text-gray-100 py-1 px-3 resize-none leading-6 transition-colors duration-200 ease-in-out"
            />
          </div>
          <button
            type="submit"
            className="text-white bg-indigo-500 border-0 py-2 px-6 focus:outline-none hover:bg-indigo-600 rounded text-lg">
            Submit
          </button>
        </form>
      </div>
    </section>
  );
}
```

## 如何嵌入谷歌地图位置

在表单的左侧，我们将包含一个嵌入了我们所在位置的谷歌地图的谷歌地图。

我们可以在一个在线工具的帮助下做到这一点:embed-map.com。你所要做的就是输入你的位置，然后点击“生成 HTML 代码”。

在给出的代码中，不要复制所有的代码，只复制 iframe 元素中的`src`属性。我们将用 iframe 的缺省值`src`替换这个值。

![portfolio-2](img/a6ed0bc0de38d6d1b920de8bf04786dc.png)

要将任何提交的表单数据发送到 Netlify，Netlify Forms 需要将表单识别为静态 HTML。因为我们的 React 应用程序是由 JavaScript 控制的，不包含普通的 HTML，所以我们需要在 public 文件夹中的 index.html 文件中添加一个隐藏表单。

```
<!-- public/index.html -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- head content skipped -->
  </head>
  <body>

  <form name="contact" netlify netlify-honeypot="bot-field" hidden>
    <input type="text" name="name" />
    <input type="email" name="email" />
    <textarea name="message"></textarea>
  </form>

    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

我们需要隐藏这个表单，因为它不需要被用户看到，只是 Netlify。

我们将赋予它属性`hidden`以及一个与 Contact.js 中的 JSX 表单匹配的属性`name`。我们还需要赋予它属性`netlify`,以便 Netlify 表单识别它。最后，我们需要包括所有与 JSX 表单相同的输入:姓名、电子邮件、消息。

## 如何提交联系表格

完成后，我们将返回 Contact.js。我们将使用 JavaScript 来提交该表单。

首先，我们将为姓名、电子邮件和消息表单中键入的每个值创建一些专用状态:

```
const [name, setName] = React.useState("");
const [email, setEmail] = React.useState("");
const [message, setMessage] = React.useState("");
```

我们将在`onChange`处理程序的帮助下，将用户输入的内容存储到每个输入状态中。

为了处理表单的提交，我们将向它添加`onSubmit`道具。将被调用的函数`handleSubmit`，将用我们所有的表单数据向端点“/”发出 post 请求。

我们将设置请求的头部，以表明我们正在发送表单数据。对于请求体，我们将包括表单名称以及来自`name`、`email`和`message`状态变量的所有表单数据。

```
// src/components/Contact.js

import React from "react";

export default function Contact() {
  const [name, setName] = React.useState("");
  const [email, setEmail] = React.useState("");
  const [message, setMessage] = React.useState("");

  function encode(data) {
    return Object.keys(data)
      .map(
        (key) => encodeURIComponent(key) + "=" + encodeURIComponent(data[key])
      )
      .join("&");
  }

  function handleSubmit(e) {
    e.preventDefault();
    fetch("/", {
      method: "POST",
      headers: { "Content-Type": "application/x-www-form-urlencoded" },
      body: encode({ "form-name": "contact", name, email, message }),
    })
      .then(() => alert("Message sent!"))
      .catch((error) => alert(error));
  }

  return (
    <section id="contact" className="relative">
      <div className="container px-5 py-10 mx-auto flex sm:flex-nowrap flex-wrap">
        <div className="lg:w-2/3 md:w-1/2 bg-gray-900 rounded-lg overflow-hidden sm:mr-10 p-10 flex items-end justify-start relative">
          <iframe
            width="100%"
            height="100%"
            title="map"
            className="absolute inset-0"
            frameBorder={0}
            marginHeight={0}
            marginWidth={0}
            style={{ filter: "opacity(0.7)" }}
            src="https://www.google.com/maps/embed/v1/place?q=97+warren+st+new+york+city&key=AIzaSyBFw0Qbyq9zTFTd-tUY6dZWTgaQzuU17R8"
          />
          <div className="bg-gray-900 relative flex flex-wrap py-6 rounded shadow-md">
            <div className="lg:w-1/2 px-6">
              <h2 className="title-font font-semibold text-white tracking-widest text-xs">
                ADDRESS
              </h2>
              <p className="mt-1">
                97 Warren St. <br />
                New York, NY 10007
              </p>
            </div>
            <div className="lg:w-1/2 px-6 mt-4 lg:mt-0">
              <h2 className="title-font font-semibold text-white tracking-widest text-xs">
                EMAIL
              </h2>
              <a className="text-indigo-400 leading-relaxed">
                reedbarger@email.com
              </a>
              <h2 className="title-font font-semibold text-white tracking-widest text-xs mt-4">
                PHONE
              </h2>
              <p className="leading-relaxed">123-456-7890</p>
            </div>
          </div>
        </div>
        <form
          netlify
          name="contact"
          onSubmit={handleSubmit}
          className="lg:w-1/3 md:w-1/2 flex flex-col md:ml-auto w-full md:py-8 mt-8 md:mt-0">
          <h2 className="text-white sm:text-4xl text-3xl mb-1 font-medium title-font">
            Hire Me
          </h2>
          <p className="leading-relaxed mb-5">
            Lorem ipsum dolor sit amet consectetur, adipisicing elit. Illum
            suscipit officia aspernatur veritatis. Asperiores, aliquid?
          </p>
          <div className="relative mb-4">
            <label htmlFor="name" className="leading-7 text-sm text-gray-400">
              Name
            </label>
            <input
              type="text"
              id="name"
              name="name"
              className="w-full bg-gray-800 rounded border border-gray-700 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-900 text-base outline-none text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
              onChange={(e) => setName(e.target.value)}
            />
          </div>
          <div className="relative mb-4">
            <label htmlFor="email" className="leading-7 text-sm text-gray-400">
              Email
            </label>
            <input
              type="email"
              id="email"
              name="email"
              className="w-full bg-gray-800 rounded border border-gray-700 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-900 text-base outline-none text-gray-100 py-1 px-3 leading-8 transition-colors duration-200 ease-in-out"
              onChange={(e) => setEmail(e.target.value)}
            />
          </div>
          <div className="relative mb-4">
            <label
              htmlFor="message"
              className="leading-7 text-sm text-gray-400">
              Message
            </label>
            <textarea
              id="message"
              name="message"
              className="w-full bg-gray-800 rounded border border-gray-700 focus:border-indigo-500 focus:ring-2 focus:ring-indigo-900 h-32 text-base outline-none text-gray-100 py-1 px-3 resize-none leading-6 transition-colors duration-200 ease-in-out"
              onChange={(e) => setMessage(e.target.value)}
            />
          </div>
          <button
            type="submit"
            className="text-white bg-indigo-500 border-0 py-2 px-6 focus:outline-none hover:bg-indigo-600 rounded text-lg">
            Submit
          </button>
        </form>
      </div>
    </section>
  );
}
```

正如你在上面看到的，我们用一个特殊的`encode`函数对表单数据进行编码。

如果消息被正确发送，我们将显示一个提示“消息已发送”。否则，如果出现错误，我们将提醒用户该错误。

## 如何构建导航栏组件

最后一步是构建我们的 Navbar 组件。

我们希望这个导航条在大型设备上贴在我们应用的顶部，而不是在移动设备上粘着。

此外，我们还想在我们的项目技能证明和联系表格中包含每个相关部分的链接:

```
// src/components/Navbar.js

import { ArrowRightIcon } from "@heroicons/react/solid";
import React from "react";

export default function Navbar() {
  return (
    <header className="bg-gray-800 md:sticky top-0 z-10">
      <div className="container mx-auto flex flex-wrap p-5 flex-col md:flex-row items-center">
        <a className="title-font font-medium text-white mb-4 md:mb-0">
          <a href="#about" className="ml-3 text-xl">
            Reed Barger
          </a>
        </a>
        <nav className="md:mr-auto md:ml-4 md:py-1 md:pl-4 md:border-l md:border-gray-700	flex flex-wrap items-center text-base justify-center">
          <a href="#projects" className="mr-5 hover:text-white">
            Past Work
          </a>
          <a href="#skills" className="mr-5 hover:text-white">
            Skills
          </a>
          <a href="#testimonials" className="mr-5 hover:text-white">
            Testimonials
          </a>
        </nav>
        <a
          href="#contact"
          className="inline-flex items-center bg-gray-800 border-0 py-1 px-3 focus:outline-none hover:bg-gray-700 rounded text-base mt-4 md:mt-0">
          Hire Me
          <ArrowRightIcon className="w-4 h-4 ml-1" />
        </a>
      </div>
    </header>
  );
}
```

在一个更大的设备上，这是如何粘在页面顶部的呢？在我们的`header`元素上的类`md:sticky`的帮助下。

这个类意味着它将从一个中等大小的断点(768px)开始应用样式规则`position: sticky;`。

## 如何部署您的投资组合

现在，为了让我们的投资组合活跃起来，我们需要将我们的应用程序推送到 GitHub。

如果你不熟悉 Git 和 GitHub，我会花一点时间来学习如何第一次将你的代码推送到你的 GitHub 账户。这是任何开发人员都应该知道的基本技能。

一旦你熟悉了这个过程，我们可以首先创建一个新的 Github 库。之后，我们将运行`git add .`、`git commit -m "Deploy"`，创建我们的 git remote 和`git push -u origin master`。

一旦我们的项目在 GitHub 上，我们可以前往 Netlify 并选择“从 Git 选择站点”选项。然后，我们将选择 GitHub 进行持续部署，并选择我们刚刚将代码推入的 GitHub 存储库。

![portfolio-3-min](img/558224064eb8755ef009f74d0eb2a427.png)

之后，我们的项目将自动部署到 web 上！

## 下一步是什么

恭喜你！你现在有一个在线的作品集应用程序，可以向潜在雇主展示你所有的项目和技能。

下一步是建立一个自定义域名，最好是用你的名字(如[reedbarger.com](https://reedbarger.com/))。由于 Netlify 包括一个 DNS，你可以很容易地用它们建立一个自定义域。

考虑在你的 React 应用中添加一个博客，向潜在雇主展示你更多的开发者知识。

作为一名开发者，让你的个人作品集成为你自己和你所热爱的东西的表达，你会成功的！

## 想要学习 React 的第一方法吗？

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获取开发人员已经掌握的内幕信息**100 条【React，找到他们梦想的工作，掌控他们的未来:**

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*