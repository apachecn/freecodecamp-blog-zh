# 使用 TypeScript 和 AWS 构建 API 的完整指南

> 原文：<https://www.freecodecamp.org/news/build-an-api-with-typescript-and-aws/>

在这篇文章中，我们将会看到如何用 TypeScript 和 Serverless 快速简单地构建一个 API。

然后我们将学习如何使用 *aws-sdk* 来访问其他 aws 服务并创建自动翻译 API。

如果你喜欢观看和学习，你可以看看下面的视频:

[https://www.youtube.com/embed/HhgXwKFUzT8?feature=oembed](https://www.youtube.com/embed/HhgXwKFUzT8?feature=oembed)

## 入门指南

要开始整个过程，我们需要确保我们已经安装了无服务器框架，并在我们的计算机上设置了 AWS 配置文件。如果你还没有，那么你可以[看看这个视频](https://youtu.be/D5_FHbdsjRc)如何设置好。

如果你想跟随这个教程，你可以跟随所有的步骤或者[在这里下载代码](https://www.subscribepage.com/awstypescriptapizip)并且跟随完整的代码。

现在我们开始创建我们的无服务器项目和 API。我们需要在终端中启动并运行命令来创建新的 repo。你所需要做的就是切换掉文件夹名称的`{YOUR FOLDER NAME}`。

```
serverless create --template aws-nodejs-typescript --path {YOUR FOLDER NAME}
```

这将用 TypeScript 创建一个非常基本的无服务器项目。如果我们用 VS 代码打开这个新文件夹，那么我们可以看到模板给了我们什么。

![Screenshot-2020-08-21-at-06.54.45](img/0f1edbaeb9060852a121557361a4affa.png)

我们要查看的主要文件是`serverless.ts`文件和`handler.ts`文件。

`serverless.ts`文件是保存部署配置的地方。这个文件告诉无服务器框架项目名称、代码的运行时语言、函数列表和一些其他配置选项。

每当我们想要改变我们项目的架构时，这就是我们将要工作的文件。

下一个文件是`handler.ts`文件。这里我们有模板给我们的 lambda 的示例代码。它非常简单，只返回一个带有消息和输入事件的 API 网关响应。稍后我们将使用它作为我们自己的 API 的开始块。

## 创建您自己的 Lambda

既然我们已经看到了模板带来的好处，是时候添加我们自己的 Lambda 和 API 端点了。

首先，我们将创建一个新文件夹来保存我们所有的 lambda 代码，并将其命名为`lambdas`。这有助于组织它，特别是当你开始在一个项目中得到几个不同的 lambdas。

在这个新文件夹中，我们将创建新的 lambda，并将其命名为`getCityInfo.ts`。如果我们打开这个文件，我们可以开始创建我们的代码。我们可以从复制所有的`handler.ts`代码开始。

我们要做的第一件事是将函数名改为`handler`。这是个人偏好，但是我喜欢命名处理事件处理器的函数。

在这个函数的第一行，我们需要添加一些代码来获取用户请求的城市。我们可以使用`pathParameters`从 URL 路径中得到这个。

```
const city = event.pathparameter?.city;
```

您可能会注意到声明中使用了`?.`。这是可选的链接，是一个非常酷的功能。我

t 表示 **如果 path 参数为 true，则获取 city 参数，否则返回 undefined。** 这意味着如果`pathParameter`不是一个对象 **，** 这不会得到导致节点运行时出错的`cannot read property **city** of undefined`错误。

现在我们有了城市，我们需要检查城市是否有效，以及我们是否有该城市的数据。为此，我们需要一些数据。我们可以使用下面的代码，并将其粘贴在文件的底部。

```
interface CityData {
    name: string;
    state: string;
    description: string;
    mayor: string;
    population: number;
    zipCodes?: string;
}

const cityData: { [key: string]: CityData } = {
    newyork: {
        name: 'New York',
        state: 'New York',
        description:
            'New York City comprises 5 boroughs sitting where the Hudson River meets the Atlantic Ocean. At its core is Manhattan, a densely populated borough that’s among the world’s major commercial, financial and cultural centers. Its iconic sites include skyscrapers such as the Empire State Building and sprawling Central Park. Broadway theater is staged in neon-lit Times Square.',
        mayor: 'Bill de Blasio',
        population: 8399000,
        zipCodes: '100xx–104xx, 11004–05, 111xx–114xx, 116xx',
    },
    washington: {
        name: 'Washington',
        state: 'District of Columbia',
        description: `DescriptionWashington, DC, the U.S. capital, is a compact city on the Potomac River, bordering the states of Maryland and Virginia. It’s defined by imposing neoclassical monuments and buildings – including the iconic ones that house the federal government’s 3 branches: the Capitol, White House and Supreme Court. It's also home to iconic museums and performing-arts venues such as the Kennedy Center.`,
        mayor: 'Muriel Bowser',
        population: 705549,
    },
    seattle: {
        name: 'Seattle',
        state: 'Washington',
        description: `DescriptionSeattle, a city on Puget Sound in the Pacific Northwest, is surrounded by water, mountains and evergreen forests, and contains thousands of acres of parkland. Washington State’s largest city, it’s home to a large tech industry, with Microsoft and Amazon headquartered in its metropolitan area. The futuristic Space Needle, a 1962 World’s Fair legacy, is its most iconic landmark.`,
        mayor: 'Jenny Durkan',
        population: 744955,
    },
};
```

这和 JavaScript 的区别在于，我们可以创建一个 **接口** 来告诉系统数据必须是什么结构。这在开始时感觉像是额外的工作，但将有助于以后使一切变得更容易。

在我们的接口中，我们定义了城市对象的键；一些是字符串，一个数字，然后`zipCodes`是可选属性。这意味着它可能在那里，但不是必须在那里。

如果我们想测试我们的接口，我们可以尝试向我们的城市数据中的任何一个城市添加一个新的属性。

TypeScript 应该立即告诉您，您的新属性在接口上不存在。如果删除一个必需的属性，TypeScript 也会报错。这可以确保您始终拥有正确的数据，并且对象看起来总是与预期的完全一样。

现在我们有了数据，可以检查用户是否发送了正确的城市请求。

```
if (!city || !cityData[city]) {

}
```

如果这个声明是真的，那么用户做错了什么，因此我们需要返回一个 400 响应。

我们可以在这里手动输入代码，但是我们将创建一个新的`apiResponses`对象，其中包含一些可能的 API 响应代码的方法。

```
const apiResponses = {
    _200: (body: { [key: string]: any }) => {
        return {
            statusCode: 200,
            body: JSON.stringify(body, null, 2),
        };
    },
    _400: (body: { [key: string]: any }) => {
        return {
            statusCode: 400,
            body: JSON.stringify(body, null, 2),
        };
    },
};
```

这使得稍后在文件中重用更加容易。您还应该看到我们有一个属性`body: { [key: string]: any }`。这说明这个函数有一个需要成为对象的 body 属性。该对象可以有任何类型值的键。

因为我们知道`body`总是一个字符串，所以我们可以使用`JSON.stringify`来确保我们返回一个字符串体。

如果我们将这个函数添加到我们的处理程序中，我们会得到:

```
export const handler: APIGatewayProxyHandler = async (event, _context) => {
    const city = event.pathParameters?.city;

    if (!city || !cityData[city]) {
        return apiResponses._400({ message: 'missing city or no data for that city' });
    }

    return apiResponses._200(cityData[city]);
};
```

如果用户没有错过一个城市或者错过了一个我们没有数据的城市，我们将返回一个 400 并显示一条错误消息。如果数据确实存在，那么我们返回一个带有数据体的 200。

# 添加新的翻译 API

在上一节中，我们设置了 TypeScript API repo 并创建了一个 lambda，它只使用硬编码数据。

这一部分将教你如何使用 **aws-sdk** 与其他 aws 服务直接交互，以创建一个真正强大的 API。

[https://www.youtube.com/embed/xdWpbr1DZHQ?feature=oembed](https://www.youtube.com/embed/xdWpbr1DZHQ?feature=oembed)

首先，我们需要为我们的翻译 API 添加一个新文件。在`lambdas`文件夹下创建一个名为`translate.ts`的新文件。我们可以从一些基本的样板代码开始这个文件。这是 TypeScript API Lambda 的起始代码。

```
import { APIGatewayProxyHandler } from 'aws-lambda';
import 'source-map-support/register';

export const handler: APIGatewayProxyHandler = async (event) => {

};
```

现在我们需要得到用户想要翻译的文本和他们想要翻译的语言。我们可以从请求体中得到这些。

这里我们要做的一件额外的事情是解析主体。默认情况下，API 网关字符串表示主体中传递的任何 JSON。然后我们可以从身体中解构文本和语言。

```
const body = JSON.parse(event.body);
const { text, language } = body;
```

我们现在需要检查用户是否传递了文本和语言。

```
if (!text) {
    // retrun 400
}
if (!language) {
    // return 400
}
```

在最后一部分中，我们将 400 响应创建为文件中的一个函数。因为我们将在多个文件中使用这些 API 响应，所以最好将它们提取到它们自己的 **通用** 文件中。

在 **下新建一个文件夹** 名为`common`。这是我们将要存储所有常用函数的地方。

在该文件夹中创建一个名为`apiResponses.ts`的新文件。这个文件将要导出带有 **_200 和 _400** 方法的`apiResponses`对象。如果您必须返回其他响应代码，那么您可以将它们添加到该对象中。

```
const apiResponses = {
    _200: (body: { [key: string]: any }) => {
        return {
            statusCode: 200,
            body: JSON.stringify(body, null, 2),
        };
    },
    _400: (body: { [key: string]: any }) => {
        return {
            statusCode: 400,
            body: JSON.stringify(body, null, 2),
        };
    },
};

export default apiResponses;
```

现在，我们可以将该对象导入到代码中，并在代码中使用这些常用方法。在我们的 **translate.ts** 文件的顶部我们现在可以添加这一行:

```
import apiResponses from './common/apiResponses';
```

并更新我们的文本和语言检查以调用该对象上的 **_400** 方法:

```
if (!text) {
    return apiResponses._400({ message: 'missing text fom the body' });
}
if (!language) {
    return apiResponses._400({ message: 'missing language from the body' });
}
```

完成后，我们知道我们有要翻译的文本和要翻译成的语言，所以我们可以开始翻译过程。

使用 aws-sdk 几乎总是一个异步任务，所以我们将把它包装在一个 **try/catch** 中，这样我们的错误处理就更容易了。

```
try {

} catch (error) {

}
```

我们需要做的第一件事是导入 aws-sdk 并创建翻译服务的新实例。

为此，我们需要安装 aws-sdk，然后导入它。首先运行`npm install --save aws-sdk`，然后将这段代码添加到翻译文件的顶部:

```
import * as AWS from 'aws-sdk';

const translate = new AWS.Translate();
```

有了这个，我们就可以开始写翻译代码了。我们先从翻译的那一行开始。将此添加到 **尝试** 部分。

```
const translatedMessage = await translate.translateText(translateParams).promise();
```

你们中的一些人可能已经注意到的一件事是，我们还没有定义它就传入了`translateParams`。那是因为我们还不确定它是什么类型。

为了找出这一点，我们可以使用 VS 代码中的一个工具`go to definition`。这允许我们跳到函数被定义的地方，这样我们就可以找出参数的类型。你可以右击选择`go to definition`或者按住 **Ctrl** 点击该功能。

![Screenshot-2020-08-23-at-08.14.03](img/1471699bcc4eb5eb08d20aa6439607af.png)

正如你所看到的，`translateText`函数接受了一个参数`Translate.Types.TranslateTextRequest`。

找出这一点的另一种方法是通过将鼠标悬停在`translateText`函数上来使用 **智能** 。你应该看到这个，在这里你可以看到那个`params: AWS.Translate.TranslateTextRequest`:

![Screenshot-2020-08-23-at-08.15.30](img/d339adc493b69e748c179b27437b5a94.png)

有了这个，我们就可以在前面提出的翻译请求之上创建我们的翻译参数。然后我们可以根据我们设置的类型来填充它。这确保我们传递了正确的字段。

```
const translateParams: AWS.Translate.Types.TranslateTextRequest = {
    Text: text,
    SourceLanguageCode: 'en',
    TargetLanguageCode: language,
};
```

现在我们有了参数，并把它们传递给了`translate.translateText`函数，我们可以开始创建我们的响应了。这只是一个 200 的回复和翻译后的信息。

```
return apiResponses._200({ translatedMessage });
```

完成所有工作后，我们可以进入 **捕捉** 部分。在这里，我们只想记录错误，然后从公共文件返回一个 400 响应。

```
console.log('error in the translation', error);
return apiResponses._400({ message: 'unable to translate the message' });
```

至此，我们完成了 lambda 代码，所以需要进入我们的`severless.ts`文件来添加这个新的 API 端点，并赋予它所需的权限。

在`serverless.ts`文件中，我们可以向下滚动到`functions`部分。在这里我们需要给对象添加一个新的函数。

```
translate: {
    handler: 'lambdas/translate.handler',
    events: [
        {
            http: {
                path: 'translate',
                method: 'POST',
                cors: true,
            },
        },
    ],
},
```

这与以前的端点的主要区别在于，现在的端点是一个 **POST** 方法。这意味着如果你尝试对这个 URL 路径做一个 **GET** 请求，你会得到一个错误响应。

最后要做的事情是允许 lambdas 使用翻译服务。对于几乎所有的 AWS 服务，您需要添加额外的权限才能在 lambda 中使用。

为此，我们在名为`iamRoleStatements`的`provider`部分添加了一个新字段。这是一组 **允许** 或 **拒绝** 的语句，用于不同的服务和资源。

```
iamRoleStatements: [
    {
        Effect: 'Allow',
        Action: ['translate:*'],
        Resource: '*',
    },
],
```

有了这个插件，我们已经设置好了所有需要的东西，所以我们可以运行`sls deploy`来部署我们的新 API。

一旦部署完成，我们就可以获得 API URL，并使用 postman 或 [postwoman.io](https://postwoman.io/) 之类的工具向该 URL 发出请求。我们只需要忽略一部分:

```
{
    "text": "This is a test message for translation",
    "language": "fr"
}
```

然后我们应该得到 200 的回应:

```
{
  "translatedMessage": {
    "TranslatedText": "Ceci est un message de test pour la traduction",
    "SourceLanguageCode": "en",
    "TargetLanguageCode": "fr"
  }
}
```

# 摘要

在本文中，我们学习了如何:

*   用`severless create --template aws-nodejs-typescript`设置一个新的类型脚本 repo
*   添加我们自己的 Lambda，它返回硬编码数据的选择
*   添加了 Lambda 作为 API 端点
*   添加了另一个 Lambda，它会自动翻译传递给它的任何文本
*   添加了一个 API 端点，并给予 Lambda 工作所需的权限

如果你喜欢这篇文章，并想了解更多关于无服务器和 AWS 的知识，那么我有一个 Youtube 频道，上面有 50 多个关于这方面的视频。我推荐观看我的[无服务器和 AWS 播放列表](https://www.youtube.com/playlist?list=PLmexTtcbIn_gP8bpsUsHfv-58KsKPsGEo)中你觉得最有趣的视频。