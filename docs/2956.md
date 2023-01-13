# 如何自动化您的 GitHub 档案自述文件

> 原文：<https://www.freecodecamp.org/news/go-automate-your-github-profile-readme/>

GitHub 的新个人资料页面 README 特性给开发者互联网的 Myspace 页面带来了一些个性。

尽管 Markdown 最适合标准的静态文本内容，但这并没有阻止有创造力的人们努力创建下一级自述文件。你可以包括 gif 和图片来增加一些动感和活力(它们在 [GitHub Flavor Markdown](https://github.github.com/gfm/) 中有所介绍)，但我在考虑一些更有活力的东西。

由于它位于您的 GitHub 个人资料的前端和中心，您的自述文件是一个很好的机会，让人们了解您的情况、您认为重要的内容，并展示您工作的一些亮点。

你可能想炫耀你最新的知识库、推文或博客文章。由于 GitHub Actions 等持续交付工具，保持更新也不一定是一件痛苦的事情。

我当前的自述每天都会更新，提供我最新博客文章的链接。下面是我如何用 Go 和 GitHub 动作构建一个自我更新的`README.md`。

## 用 Go 读写文件

我最近写了很多 Python，但是对于一些事情我真的很喜欢用 Go。你可以说它是我在 just-for-to 项目中的常用语言。抱歉。我控制不了自己。

为了创建我的 README.md，我将从现有文件中获取一些静态内容，将其与我们将使用 Go 生成的一些新的动态内容混合在一起，然后在 400 度的温度下烘烤整个文件，直到出现一些令人惊叹的内容。

下面是我们如何读入一个名为`static.md`的文件，并将其放入`string`格式:

```
// Unwrap Markdown content
content, err := ioutil.ReadFile("static.md")
if err != nil {
    log.Fatalf("cannot read file: %v", err)
    return err
}

// Make it a string
stringyContent := string(content) 
```

您的动态内容的可能性只受到您的想象力的限制！在这里，我将使用 [`github.com/mmcdole/gofeed`](https://github.com/mmcdole/gofeed) [包](https://github.com/mmcdole/gofeed)从我的博客中读取 RSS 提要并获取最新的帖子。

```
fp := gofeed.NewParser()
feed, err := fp.ParseURL("https://victoria.dev/index.xml")
if err != nil {
    log.Fatalf("error getting feed: %v", err)
}
// Get the freshest item
rssItem := feed.Items[0] 
```

为了将这些位连接在一起并产生字符串，我们使用 [`fmt.Sprintf()`](https://golang.org/pkg/fmt/#Sprintf) 来创建一个格式化的字符串。

```
// Whisk together static and dynamic content until stiff peaks form
blog := "Read my latest blog post: **[" + rssItem.Title + "](" + rssItem.Link + ")**"
data := fmt.Sprintf("%s\n%s\n", stringyContent, blog) 
```

然后从这个混合文件中创建一个新文件，我们使用 [`os.Create()`](https://golang.org/pkg/os/#Create) 。关于推迟`file.Close()` 还有[更多的事情需要了解，但是我们不需要在这里深入那些细节。我们将添加`file.Sync()`，以确保我们的自述文件被写入。](https://www.joeshaw.org/dont-defer-close-on-writable-files/)

```
// Prepare file with a light coating of os
file, err := os.Create("README.md")
if err != nil {
    return err
}
defer file.Close()

// Bake at n bytes per second until golden brown
_, err = io.WriteString(file, data)
if err != nil {
    return err
}
return file.Sync() 
```

在我的自述文件库中查看完整代码[。](https://github.com/victoriadrake/victoriadrake/blob/master/update/main.go)

嗯，闻起来不错吧？？让我们通过一个 GitHub 操作让这一点在日常生活中发生。

## 按照行动计划运行您的围棋程序

您可以创建一个 GitHub 动作工作流，该工作流由[触发](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)，既可以推送到您的`master`分支，也可以按每天的时间表触发。下面是定义这一点的`.github/workflows/update.yaml`片段:

```
on:
  push:
    branches:
      - master
  schedule:
    - cron: '0 11 * * *' 
```

要运行重建自述文件的 Go 程序，我们首先需要文件的副本。我们用`actions/checkout`来表示:

```
steps:
    - name: ?️ Get working copy
      uses: actions/checkout@master
      with:
        fetch-depth: 1 
```

这一步运行我们的 Go 程序:

```
- name: ? Shake & bake README
  run: |
    cd ${GITHUB_WORKSPACE}/update/
    go run main.go 
```

最后，我们将更新的文件推回我们的存储库。使用工作流中的变量和秘密了解更多关于[中显示的变量的信息。](https://docs.github.com/en/actions/configuring-and-managing-workflows/using-variables-and-secrets-in-a-workflow)

```
- name: ? Deploy
  run: |
    git config user.name "${GITHUB_ACTOR}"
    git config user.email "${GITHUB_ACTOR}@users.noreply.github.com"
    git add .
    git commit -am "Update dynamic content"
    git push --all -f https://${{ secrets.GITHUB_TOKEN }}@github.com/${GITHUB_REPOSITORY}.git 
```

在我的自述文件库中查看这个动作工作流[的完整代码。](https://github.com/victoriadrake/victoriadrake/blob/master/.github/workflows/update.yaml)

## 前进并自动更新您的自述文件

恭喜你，欢迎来到酷孩子俱乐部！您现在知道如何构建自动更新的 GitHub 概要文件自述文件了。你现在可以前进，并添加各种各样的整洁的动态元素到你的页面上——只是要放松 gif，好吗？