# 如何用 Typescript 自动化你的博客文章发布过程

> 原文：<https://www.freecodecamp.org/news/automating-my-blog-posts-publishing-process-with-typescript/>

因为我在努力养成写作习惯，嗯，我写的越来越多了。尽管我使用像 [Medium](https://medium.com/@leandrotk_) 、 [dev.to](https://dev.to/teekay) 和 [Hashnode](https://hashnode.com/@teekay) 这样的发布博客，我也喜欢在[我自己的博客](http://leandrotk.github.io/tk)上发布我的内容。

因为我想建立一个简单的网站，这个博客基本上是 HTML 和 CSS，只有很少的 JavaScript。但问题是，我需要改进出版流程。

那么现在是怎么运作的呢？

我在观念上管理博客路线图。看起来是这样的:

![blog-roadmap](img/c0f97047df6b4b0d9b94bb8f6f1e0115.png)

这是一个简单的看板类型的板。我喜欢这个板，因为我可以把我所有的想法变成实体(或数字？)表示。我也用它来建立一个草稿，润色那个草稿，让它越来越好，然后发布到博客上。

所以我用概念来写我的博客。在我完成它之后，我复制这个想法并粘贴到一个在线工具中，将 markdown 转换成 HTML。然后我可以使用这个 HTML 来创建实际的帖子。

但这只是正文，页面的内容。我总是需要创建一个包含标题内容、正文和页脚的 HTML。

这个过程繁琐又无聊。但好消息是，它可以自动化。这篇文章就是关于自动化的。我想向你们展示我创造的这个新工具的幕后以及我在这个过程中学到的东西。

## 特征

我的主要想法是准备发布一整篇 HTML 文章。正如我之前提到的，`<head>`和`<footer>`部分变化不大。所以我可以用这些作为“模板”。

有了这个模板，我就有了可以为我撰写和发表的每篇文章修改的数据。该数据是模板中的一个变量，表示为`{{ variableName }}`。一个例子:

```
<h1>{{ title }}</h1> 
```

现在我可以使用模板并用真实数据替换变量——每篇文章的特定信息。

第二部分是正文，实帖。在模板中，用`{{ article }}`表示。这个变量将被 markdown 生成的 HTML 替换。

当我们从概念中复制和粘贴笔记时，我们得到了一种降价风格。这个项目将把这个 markdown 转换成一个 HTML，并把它作为模板中的`article`变量。

为了创建理想的模板，我查看了需要创建的所有变量:

*   `title`
*   `description`
*   `date`
*   `tags`
*   `imageAlt`
*   `imageCover`
*   `photographerUrl`
*   `photographerName`
*   `article`
*   `keywords`

有了这些变量，我创建了[模板](https://github.com/leandrotk/publisher/blob/master/examples/template.html)。

为了传递这些信息来构建 HTML，我创建了一个`json`文件作为文章配置:`article.config.json`。我有这样的东西:

```
{
  "title": "React Hooks, Context API, and Pokemons",
  "description": "Understanding how hooks and the context api work",
  "date": "2020-04-21",
  "tags": [
    "javascript",
    "react"
  ],
  "imageAlt": "The Ash from Pokemon",
  "photographerUrl": "<https://www.instagram.com/kazuh.illust>",
  "photographerName": "kazuh.yasiro",
  "articleFile": "article.md",
  "keywords": "javascript,react"
} 
```

第一步是项目应该知道如何打开和阅读模板和文章配置。我使用这些数据来填充模板。

模板优先:

```
const templateContent: string = await getTemplateContent(); 
```

所以我们基本上需要实现`getTemplateContent`函数。

```
import fs, { promises } from 'fs';
import { resolve } from 'path';

const { readFile } = promises;

const getTemplateContent = async (): Promise<string> => {
  const contentTemplatePath = resolve(__dirname, '../examples/template.html');
  return await readFile(contentTemplatePath, 'utf8');
}; 
```

带有`__dirname`的`resolve`将从正在运行的源文件中获取目录的绝对路径。然后转到`examples/template.html`文件。`readFile`将异步读取并返回模板路径中的内容。

现在我们有了模板内容。我们需要为文章配置做同样的事情。

```
const getArticleConfig = async (): Promise<ArticleConfig> => {
  const articleConfigPath = resolve(__dirname, '../examples/article.config.json');
  const articleConfigContent = await readFile(articleConfigPath, 'utf8');
  return JSON.parse(articleConfigContent);
}; 
```

这里发生了两件不同的事情:

*   由于`article.config.json`有一个 json 格式，我们需要在读取文件后将这个 json 字符串转换成一个 JavaScript 对象
*   文章配置内容的返回将是一个我在函数返回类型中定义的`ArticleConfig`。让我们建造它。

```
type ArticleConfig = {
  title: string;
  description: string;
  date: string;
  tags: string[];
  imageCover: string;
  imageAlt: string;
  photographerUrl: string;
  photographerName: string;
  articleFile: string;
  keywords: string;
}; 
```

当我们得到这个内容时，我们也使用这个新类型。

```
const articleConfig: ArticleConfig = await getArticleConfig(); 
```

现在我们可以使用`replace`方法在模板内容中填充配置数据。为了说明这个想法，它看起来像这样:

```
templateContent.replace('title', articleConfig.title) 
```

但是有些变量在模板中出现不止一次。Regex 来拯救。有了这个:

```
new RegExp('\\{\\{(?:\\\\s+)?(title)(?:\\\\s+)?\\}\\}', 'g'); 
```

...我得到所有匹配`{{ title }}`的字符串。所以我可以构建一个函数来接收要查找的参数，并在标题位置使用它。

```
const getPattern = (find: string): RegExp =>
  new RegExp('\\{\\{(?:\\\\s+)?(' + find + ')(?:\\\\s+)?\\}\\}', 'g'); 
```

现在我们可以替换所有匹配。标题变量的示例:

```
templateContent.replace(getPattern('title'), articleConfig.title) 
```

但是我们不想只替换标题变量，而是替换文章配置中的所有变量。全部替换！

```
const buildArticle = (templateContent: string) => ({
  with: (articleConfig: ArticleAttributes) =>
    templateContent
      .replace(getPattern('title'), articleConfig.title)
      .replace(getPattern('description'), articleConfig.description)
      .replace(getPattern('date'), articleConfig.date)
      .replace(getPattern('tags'), articleConfig.articleTags)
      .replace(getPattern('imageCover'), articleConfig.imageCover)
      .replace(getPattern('imageAlt'), articleConfig.imageAlt)
      .replace(getPattern('photographerUrl'), articleConfig.photographerUrl)
      .replace(getPattern('photographerName'), articleConfig.photographerName)
      .replace(getPattern('article'), articleConfig.articleBody)
      .replace(getPattern('keywords'), articleConfig.keywords)
}); 
```

现在我全部换掉！我们这样使用它:

```
const article: string = buildArticle(templateContent).with(articleConfig); 
```

但是我们遗漏了两个部分:

*   `tags`
*   `article`

在 config json 文件中，`tags`是一个列表。所以，对于这个列表:

```
['javascript', 'react']; 
```

最终的 HTML 将是:

```
<a class="tag-link" href="../../../tags/javascript.html">javascript</a>
<a class="tag-link" href="../../../tags/react.html">react</a> 
```

所以我创建了另一个模板:`tag_template.html`和`{{ tag }}`变量。我们只需要映射`tags`列表并创建每个 HTML 标签模板。

```
const getArticleTags = async ({ tags }: { tags: string[] }): Promise<string> => {
  const tagTemplatePath = resolve(__dirname, '../examples/tag_template.html');
  const tagContent = await readFile(tagTemplatePath, 'utf8');
  return tags.map(buildTag(tagContent)).join('');
}; 
```

在这里我们:

*   获取标记模板路径
*   获取标记模板内容
*   通过`tags`映射，并基于标签模板构建最终的标签 HTML

`buildTag`是一个返回另一个函数的函数。

```
const buildTag = (tagContent: string) => (tag: string): string =>
  tagContent.replace(getPattern('tag'), tag); 
```

它接收`tagContent`——它是标签模板内容——并返回一个接收标签并构建最终标签 HTML 的函数。现在我们调用它来获取文章标签。

```
const articleTags: string = await getArticleTags(articleConfig); 
```

关于这篇文章。看起来是这样的:

```
const getArticleBody = async ({ articleFile }: { articleFile: string }): Promise<string> => {
  const articleMarkdownPath = resolve(__dirname, `../examples/${articleFile}`);
  const articleMarkdown = await readFile(articleMarkdownPath, 'utf8');
  return fromMarkdownToHTML(articleMarkdown);
}; 
```

它接收到`articleFile`，我们尝试获取路径，读取文件，并获取 markdown 内容。然后将这些内容传递给`fromMarkdownToHTML`函数，将 markdown 转换成 HTML。

对于这一部分，我使用一个名为`showdown`的外部库。它处理将 markdown 转换成 HTML 的每一个小细节。

```
import showdown from 'showdown';

const fromMarkdownToHTML = (articleMarkdown: string): string => {
  const converter = new showdown.Converter()
  return converter.makeHtml(articleMarkdown);
}; 
```

现在我有了标签和文章 HTML:

```
const templateContent: string = await getTemplateContent();
const articleConfig: ArticleConfig = await getArticleConfig();
const articleTags: string = await getArticleTags(articleConfig);
const articleBody: string = await getArticleBody(articleConfig);

const article: string = buildArticle(templateContent).with({
  ...articleConfig,
  articleTags,
  articleBody
}); 
```

我又错过了一件事！以前，我认为我总是需要将图像封面路径添加到文章配置文件中。大概是这样的:

```
{
  "imageCover": "an-image.png",
} 
```

但是我们可以假设图像名称是`cover`。挑战在于延伸。可以是`.png`、`.jpg`、`.jpeg`或`.gif`。

所以我构建了一个函数来获得正确的图像扩展。想法是在文件夹中搜索图像。如果它存在于文件夹中，则返回扩展名。

我从“现有的”部分开始。

```
fs.existsSync(`${folder}/${fileName}.${extension}`); 
```

这里我使用`existsSync`函数来查找文件。如果它存在于文件夹中，则返回 true。否则为假。

我将这段代码添加到一个函数中:

```
const existsFile = (folder: string, fileName: string) => (extension: string): boolean =>
  fs.existsSync(`${folder}/${fileName}.${extension}`); 
```

我为什么这样做？

使用这个函数，我需要通过`folder`、`filename`和`extension`。`folder`和`filename`总是一样的。不同的是`extension`。

所以我可以用 curry 创建一个函数。这样，我可以为同一个`folder`和`filename`构建不同的函数。像这样:

```
const hasFileWithExtension = existsFile(examplesFolder, imageName);

hasFileWithExtension('jpeg'); // true or false
hasFileWithExtension('jpg'); // true or false
hasFileWithExtension('png'); // true or false
hasFileWithExtension('gif'); // true or false 
```

整个函数如下所示:

```
const getImageExtension = (): string => {
  const examplesFolder: string = resolve(__dirname, `../examples`);
  const imageName: string = 'cover';
  const hasFileWithExtension = existsFile(examplesFolder, imageName);

  if (hasFileWithExtension('jpeg')) {
    return 'jpeg';
  }

  if (hasFileWithExtension('jpg')) {
    return 'jpg';
  }

  if (hasFileWithExtension('png')) {
    return 'png';
  }

  return 'gif';
}; 
```

但是我不喜欢用这个硬编码的字符串来表示图像扩展。`enum`真酷！

```
enum ImageExtension {
  JPEG = 'jpeg',
  JPG = 'jpg',
  PNG = 'png',
  GIF = 'gif'
}; 
```

这个函数现在使用我们新的枚举`ImageExtension`:

```
const getImageExtension = (): string => {
  const examplesFolder: string = resolve(__dirname, `../examples`);
  const imageName: string = 'cover';
  const hasFileWithExtension = existsFile(examplesFolder, imageName);

  if (hasFileWithExtension(ImageExtension.JPEG)) {
    return ImageExtension.JPEG;
  }

  if (hasFileWithExtension(ImageExtension.JPG)) {
    return ImageExtension.JPG;
  }

  if (hasFileWithExtension(ImageExtension.PNG)) {
    return ImageExtension.PNG;
  }

  return ImageExtension.GIF;
}; 
```

现在我已经有了填充模板的所有数据。太好了！

HTML 完成后，我想用这些数据创建真正的 HTML 文件。我基本上需要获得正确的路径，HTML，并使用`writeFile`函数来创建这个文件。

为了找到路径，我需要了解我的博客的模式。它用年、月、标题组织文件夹，文件命名为`index.html`。

一个例子是:

```
2020/04/publisher-a-tooling-to-blog-post-publishing/index.html 
```

起初，我考虑将这些数据添加到文章配置文件中。所以每次我需要从文章配置中更新这个属性来获得正确的路径。

但是另一个有趣的想法是通过文章配置文件中已经有的一些数据来推断路径。我们有`date`(例如`"2020-04-21"`)和`title`(例如`"Publisher: tooling to automate blog post publishing"`)。

从日期，我可以得到年和月。根据标题，我可以生成文章文件夹。`index.html`文件总是不变的。

该字符串如下所示:

```
`${year}/${month}/${slugifiedTitle}` 
```

对于约会来说，真的很简单。我可以通过`-`分裂和解构:

```
const [year, month]: string[] = date.split('-'); 
```

对于`slugifiedTitle`，我构建了一个函数:

```
const slugify = (title: string): string =>
  title
    .trim()
    .toLowerCase()
    .replace(/[^\\w\\s]/gi, '')
    .replace(/[\\s]/g, '-'); 
```

它删除字符串开头和结尾的空格。然后向下转换字符串。然后删除所有特殊字符(只保留单词和空白字符)。最后，用一个`-`替换所有的空白。

整个函数看起来像这样:

```
const buildNewArticleFolderPath = ({ title, date }: { title: string, date: string }): string => {
  const [year, month]: string[] = date.split('-');
  const slugifiedTitle: string = slugify(title);

  return resolve(__dirname, `../../${year}/${month}/${slugifiedTitle}`);
}; 
```

这个函数试图获取文章文件夹。它不会生成新文件。这就是为什么我没有把`/index.html`加到最后一个字符串的末尾。

为什么会这样？因为，在写新文件之前，我们总是需要创建文件夹。我用`mkdir`和这个文件夹路径来创建它。

```
const newArticleFolderPath: string = buildNewArticleFolderPath(articleConfig);
await mkdir(newArticleFolderPath, { recursive: true }); 
```

现在，我可以使用该文件夹在其中创建新的文章文件。

```
const newArticlePath: string = `${newArticleFolderPath}/index.html`;
await writeFile(newArticlePath, article); 
```

这里我们遗漏了一件事:当我在文章配置文件夹中添加图像封面时，我需要复制它并粘贴到正确的位置。

对于`2020/04/publisher-a-tooling-to-blog-post-publishing/index.html`示例，图像封面将位于资产文件夹中:

```
2020/04/publisher-a-tooling-to-blog-post-publishing/assets/cover.png 
```

为此，我需要两样东西:

*   用`mkdir`创建一个新的`assets`文件夹
*   用`copyFile`复制图像文件并粘贴到新文件夹中

要创建新文件夹，我只需要文件夹路径。要复制和粘贴图像文件，我需要当前图像路径和文章图像路径。

对于文件夹，因为我有`newArticleFolderPath`，我只需要将这个路径连接到 assets 文件夹。

```
const assetsFolder: string = `${newArticleFolderPath}/assets`; 
```

对于当前的图像路径，我有带有正确扩展名的`imageCoverFileName`。我只需要得到图像覆盖路径:

```
const imageCoverExamplePath: string = resolve(__dirname, `../examples/${imageCoverFileName}`); 
```

为了获得未来的图像路径，我需要连接图像封面路径和图像文件名:

```
const imageCoverPath: string = `${assetsFolder}/${imageCoverFileName}`; 
```

有了这些数据，我就可以创建新文件夹了:

```
await mkdir(assetsFolder, { recursive: true }); 
```

复制并粘贴图像封面文件:

```
await copyFile(imageCoverExamplePath, imageCoverPath); 
```

当我实现这个`paths`部分的时候，我发现我可以把它们组合成一个函数`buildPaths`。

```
const buildPaths = (newArticleFolderPath: string): ArticlePaths => {
  const imageExtension: string = getImageExtension();
  const imageCoverFileName: string = `cover.${imageExtension}`;
  const newArticlePath: string = `${newArticleFolderPath}/index.html`;
  const imageCoverExamplePath: string = resolve(__dirname, `../examples/${imageCoverFileName}`);
  const assetsFolder: string = `${newArticleFolderPath}/assets`;
  const imageCoverPath: string = `${assetsFolder}/${imageCoverFileName}`;

  return {
    newArticlePath,
    imageCoverExamplePath,
    imageCoverPath,
    assetsFolder,
    imageCoverFileName
  };
}; 
```

我还创建了`ArticlePaths`类型:

```
type ArticlePaths = {
  newArticlePath: string;
  imageCoverExamplePath: string;
  imageCoverPath: string;
  assetsFolder: string;
  imageCoverFileName: string;
}; 
```

我可以使用该函数获得我需要的所有路径数据:

```
const {
  newArticlePath,
  imageCoverExamplePath,
  imageCoverPath,
  assetsFolder,
  imageCoverFileName
}: ArticlePaths = buildPaths(newArticleFolderPath); 
```

现在是算法的最后一部分！我想快速验证创建的帖子。那么，如果我可以在浏览器选项卡中打开创建的帖子会怎么样呢？那太棒了！

所以我做到了:

```
await open(newArticlePath); 
```

这里我使用`open`库来模拟终端打开命令。

就这样了！

## 我学到了什么

这个项目非常有趣！通过这个过程我学到了一些很酷的东西。我想把它们列在这里:

*   当我在[学习 Typescript](https://leandrotk.github.io/tk/2020/04/typescript-learnings/index.html) 时，我想快速验证我写的代码。所以我配置`nodemon`在每次文件保存时编译并运行代码。将开发过程变得如此动态是很酷的。
*   我尝试使用新节点`fs`的`promises` : `readFile`、`mkdir`、`writeFile`和`copyFile`。它在`Stability: 2`上。
*   我对一些函数做了大量的[处理](https://leandrotk.github.io/tk/2020/03/closure-currying-and-cool-abstractions/index.html),使它们可以重用。
*   枚举和[类型](https://leandrotk.github.io/tk/2020/04/typescript-learnings-002-type-system/index.html)是在 Typescript 中保持状态一致的好方法，也是所有项目数据的好表示和文档。[数据契约](https://leandrotk.github.io/tk/2020/04/thinking-in-data-contracts/index.html)是一个非常好的东西。
*   工具思维。这是我真正喜欢编程的原因之一。构建工具来自动化重复的任务，让生活更轻松。

我希望这是一本好书！继续学习，继续编码！

这篇文章最初发表在我的博客上。

我的 [Twitter](https://twitter.com/leandrotk_) 和 [Github](https://github.com/leandrotk/) 。

## 资源

*   [发布者工具:源代码](https://github.com/leandrotk/publisher)
*   [在数据契约中思考](https://leandrotk.github.io/tk/2020/04/thinking-in-data-contracts/index.html)
*   [打字稿学习](https://leandrotk.github.io/tk/2020/04/typescript-learnings/index.html)
*   [闭包、奉承和酷抽象](https://leandrotk.github.io/tk/2020/03/closure-currying-and-cool-abstractions/index.html)
*   [通过构建 App 学习 React](https://alterclass.io/?ref=5ec57f513c1321001703dcd2)