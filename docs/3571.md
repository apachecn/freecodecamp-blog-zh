# 如何写“你好，世界！”离子的

> 原文：<https://www.freecodecamp.org/news/how-to-write-hello-world-in-ionic/>

本指南将教你如何用 Ionic 编写一个简单的 Hello World 程序。

## 步骤 1:安装

如果没有安装，安装 ionic、npm、angular 和 cordova。【更多信息请参见[第一篇](https://guide.freecodecamp.org/ionic)简介。]

```
sudo apt-get install nodejs
sudo apt-get install npm 
sudo npm install -g ionic cordova
```

## 步骤 2:创建一个名为 helloworld 的文件夹

```
ionic start helloworld blank
```

注意:由于这是一个小项目，在程序执行过程中出现提示时，请输入 No 或 N。

## 步骤 3:将目录更改为 hello world[这是您的 ionic 目录]

```
cd helloworld
```

## 步骤 4:在文本编辑器中打开文件夹。

您将看到各种文件和子文件夹。

不要惊慌，这些文件是由 npm 自动为您生成的。就去`src`->-`page`->-`home`->-`home.html`。

### 基本文件格式

```
`home.html` is the html page where you can write html syntax.<br/>

`home.scss` is the css page to write css syntax.<br/>

`home.ts` is the typescript page to write typescript code.
```

## 步骤 6:删除模板并添加 html 语法，如下所示

```
 <ion-header>
  <ion-navbar>
    <ion-title>
      Ionic Project
    </ion-title>
   </ion-navbar>
  </ion-header>

  <ion-content padding>
   <h2>Hello World </h2>
  </ion-content>
```

## 步骤 7:保存代码并运行

```
ionic serve
```

要查看您的代码运行，请转到浏览器并在 URL 中打开 localhost:8100。就是这样！

## 关于爱奥尼亚的更多信息:

*   [离子全程(视频)](https://www.freecodecamp.org/news/ionic-full-course/)
*   [用 Ionic 创建一个 CRUD todo 应用](https://www.freecodecamp.org/news/creating-a-crud-to-do-app-using-ionic-4/)