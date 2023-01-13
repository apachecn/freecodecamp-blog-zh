# 如何强制使用纱线或 NPM

> 原文：<https://www.freecodecamp.org/news/how-to-force-use-yarn-or-npm/>

在这篇短文中，我将向你展示如何根据你的需要防止使用**【NPM】**或 **【纱】** 。

当您的团队或者组织偏爱一个特定的包管理器时，这就很方便了。

使用这种方法，您可以确保每个人都使用同一个包管理器。

我们开始吧！

想看视频了解如何做到这一点吗？看看这个:

[https://www.youtube.com/embed/RmpVlaocd0M?feature=oembed](https://www.youtube.com/embed/RmpVlaocd0M?feature=oembed)

## 编辑(Edit)。npmrc

您的代码库中可能没有这个文件。如果是这种情况，请在应用程序的根文件夹中创建该文件。

它允许我们指定包管理器的配置，并由 **npm** 和 **yarn** 使用。

您的`.npmrc`文件应该将`engine-strict`属性标记为`true`。

```
//.npmrc file

engine-strict = true
```

这个选项告诉包管理器使用我们在`package.json`文件中指定的引擎版本。

## 编辑包. json

如果您当前没有`package.json`文件，您应该在里面添加`engines`部分。

```
 //package.json
{ 
  ...
  "engines": {
    "npm": "please-use-yarn",
    "yarn": ">= 1.19.1"
  },
  ...
}
```

在上面的代码中，`package.json`文件使用的是`yarn` 1.19.1 或更高版本。
但是对于`npm`，我们指定了一个不存在的版本。

这样我们可以确保当有人试图使用`npm`而不是`yarn`时，他们会收到一个输出“`please-use-yarn`”的错误。

## 运行 npm 安装

一旦你完成了上面的修改，试着运行`npm install`。

您将收到一条错误消息，阻止您使用`npm`。

```
 npm ERR! code ENOTSUP
npm ERR! notsup Unsupported engine for my-app@0.1.0: wanted: {"npm":"please-use-yarn","yarn":">= 1.19.1"} (current:
 {"node":"12.16.3","npm":"6.14.4"})
npm ERR! notsup Not compatible with your version of node/npm: my-app@0.1.0
npm ERR! notsup Not compatible with your version of node/npm: my-app@0.1.0
npm ERR! notsup Required: {"npm":"please-use-yarn","yarn":">= 1.19.1"}
npm ERR! notsup Actual:   {"npm":"6.14.4","node":"12.16.3"}

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\YourUser\AppData\Roaming\npm-cache\_logs\2020-05-21T10_21_04_676Z-debug.log 
```

当然，如果你想阻止使用`yarn`，可以反过来做。

## 结论

确保在您的项目中只使用一个包管理器是非常简单和容易的。这将减少由使用不同包管理器的开发人员引起的错误的机会，并且这是一个标准化项目编码规则和管理的好实践。

你可以在[推特](https://twitter.com/pelu_carol)、[脸书](https://www.facebook.com/neutrondevcom)和我的[网站](https://neutrondev.com/)上向我咨询任何问题。