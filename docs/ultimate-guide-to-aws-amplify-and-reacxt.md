# 如何使用 AWS Amplify 和 React 构建全栈应用

> 原文：<https://www.freecodecamp.org/news/ultimate-guide-to-aws-amplify-and-reacxt/>

AWS Amplify 是由 Amazon Web Services 开发的工具，有助于简化应用程序开发。

它包括许多功能，使您可以快速、轻松地使用其他 AWS 服务。这意味着你可以花更多的时间来构建让你的应用独一无二的功能。

本教程分为四个部分:

*   如何使用登录创建应用程序
*   如何添加数据库并处理数据
*   如何添加文件存储和使用这些文件
*   如何允许用户上传自己的文件和数据

如果你想离线阅读这篇文章，你可以在这里下载。

## 如何创建带有注册、登录和注销的应用程序

在第一部分中，我们将使用 AWS Amplify 建立一个新的 React 应用程序，以尽可能简单的方式添加注册、登录和注销。

[https://www.youtube.com/embed/g4qKydnd0vU?feature=oembed](https://www.youtube.com/embed/g4qKydnd0vU?feature=oembed)

我们需要从使用`create-react-app`创建一个新的 React 应用程序开始。打开终端并运行这些命令。如果你没有安装 create-react 应用程序，那么你可以先运行`npm i -g create-react-app`。

```
npx create-react-app amplify-react-app

cd amplify-react-app
```

有了设置，我们现在可以安装放大器，然后配置它。

```
npm install -g @aws-amplify/cli

amplify configure
```

这将在您的浏览器中打开一个 AWS 控制台选项卡。确保您使用具有管理员权限的用户登录到正确的帐户。

回到终端并按照步骤操作，为用户添加一个地区和名称。这将带您回到浏览器，您可以按照步骤创建新用户。确保停留在你看到密钥和秘密的页面上！

再次回到终端，您可以按照步骤操作，在询问时将访问密钥和密码复制到终端中。当询问您是否要将此添加到个人资料时，请说`Yes`。创建一个类似于`serverless-amplify`的概要文件。

现在我们可以通过运行`amplify init`来初始化放大设置。你可以给项目起个名字，回答所有问题。他们中的大多数应该已经是正确的了。这需要一段时间来改变你的账户。

一旦完成，我们需要添加应用程序的认证。我们用`amplify add auth`来做这件事。选择方法为`default`登录`email`，然后`no, I am done`。我们现在可以通过运行`amplify push`来部署它。这需要一段时间，但是最后，我们的`src/aws-exports.js`文件已经创建好了。

## 如何构建 React 应用程序

现在我们可以开始创建 react 应用程序了。从安装我们需要的 Amplify npm 包开始。

```
npm install --save aws-amplify @aws-amplify/ui-react
```

现在我们可以开始编辑应用程序的代码了。在我们的`src/App.js`文件中，我们可以删除标题中的所有内容，并替换为:

```
<header className="App-header">
    <AmplifySignOut />
    <h2>My App Content</h2>
</header>
```

这是一个非常基本的设置，但是你可以把你网站的主要内容放在这里，然后把`AmplifySignOut`按钮放在你想放的地方。

我们还需要在文件的顶部添加一些额外的导入:

```
import Amplify from 'aws-amplify';
import awsconfig from './aws-exports';
import { AmplifySignOut, withAuthenticator } from '@aws-amplify/ui-react';

Amplify.configure(awsconfig); 
```

现在，我们需要做的最后一件事是改变我们导出应用程序的方式。将最后一行改为`export default withAuthenticator(App);`以将 Amplify 添加到此应用程序中。

现在，当我们运行`npm start`时，我们应该会看到一个登录屏幕。我们没有创造这个，所以它来自于 Amplify 本身。

![Screenshot-2020-08-11-at-06.48.01](img/fbf150df38ce81510e0a324219985c7b.png)

如果我们尝试登录，那么将会失败，因为我们需要先注册。我们可以点击`create account`，然后输入我们的电子邮件和密码来注册。

一旦我们通过提交发送给我们的代码确认了我们的电子邮件，我们就进入我们应用程序的主页。如果我们注销，我们现在可以像预期的那样重新登录。

## 如何向我们的应用程序添加数据库

如果你想在 React 应用程序中添加数据，但不想构建 API，那么这就是适合你的部分。我们将了解如何在 React 应用程序中使用 AWS amplify，以便使用 GraphQL 访问后端数据库。

[https://www.youtube.com/embed/kqi4gPfdVHY?feature=oembed](https://www.youtube.com/embed/kqi4gPfdVHY?feature=oembed)

首先，我们需要进入终端并运行:

```
amplify add api
```

这将启动一组 CLI 选项，询问我们几个配置问题:

我们要使用什么样的 API:****graph QL****
API 名称: ****songAPI****
我们要如何认证 API: ****Amazon Cognito 用户池****
高级设置: ****否，我完成了****
您有模式吗: ****否****
哪种

![Screenshot-2020-08-20-at-07.13.40](img/b4792aa5b0205e76f1c20fddbebe517b.png)

经过一点设置后，系统会询问我们是否要编辑我们的新模式。我们想说是。这将打开 GraphQL 模式，我们将把它更新为这里列出的模式。

```
type Song @model {
    id: ID!
    title: String!
    description: String!
    filePath: String!
    likes: Int!
    owner: String!
}
```

设置好模式后，我们将运行`amplify push`,将我们当前的 amplify 设置与 AWS 帐户上的设置进行比较。由于我们已经添加了一个新的 API，我们将有所改变，所以我们将被询问是否要继续这些改变。

一旦我们选择了**是**，我们就会进入另一组选项。

我们是否要为我们的 GraphQL API 生成代码:**是**
哪种语言: **JavaScript**
新文件的文件模式: **src/graphql/**/*。js**
生成所有操作:**是**
最大语句深度: **2**

这将把所有的更改部署到 AWS，并在 React 应用程序中设置新的请求文件。这确实需要几分钟的时间。

一旦完成，我们可以进入我们的`App.js`文件，并将其重命名为`App.jsx`。这使得编写我们的 JSX 代码更加容易。

我们现在需要在这里写一个函数，从我们的新数据库中获取歌曲列表。这个函数在`listSongs`的操作中调用 GraphQL API 传入。我们还需要向`App`组件添加一个新的状态。

```
const [songs, setSongs] = useState([]);

const fetchSongs = async () => {
    try {
        const songData = await API.graphql(graphqlOperation(listSongs));
        const songList = songData.data.listSongs.items;
        console.log('song list', songList);
        setSongs(songList);
    } catch (error) {
        console.log('error on fetching songs', error);
    }
};
```

我们现在需要添加或更新一些导入到我们的文件中，以使其工作:

```
import React, { useState, useEffect } from 'react';
import { listSongs } from './graphql/queries';
import Amplify, { API, graphqlOperation } from 'aws-amplify';
```

`listSongs`是 amplify 创造的帮助我们访问数据的功能之一。您可以在`./graphql`文件夹中看到其他可用的功能。

现在我们希望这个函数在组件渲染时被调用一次，而不是每次重新渲染时都被调用。为此，我们使用`useEffect`,但要确保添加第二个参数`[]`,这样它只被触发一次。

```
useEffect(() => {
    fetchSongs();
}, []);
```

如果我们现在使用`npm start`启动我们的应用程序，然后转到应用程序，我们可以打开控制台并查看`song list []`的日志。这意味着`useEffect`已经调用了`fetchSongs`，T3 正在控制台记录结果，但是目前数据库中什么也没有。

要纠正这一点，我们需要登录我们的 AWS 帐户并添加 DynamoDB 服务。我们应该找一个叫做`Song-5gq8g8wh64w-dev`的新表。如果你找不到它，确保检查其他地区。

这目前没有数据，所以我们需要添加一些。现在，我们将在这里手动创建新数据。在`Items`下点击`Create item`，然后确保左上方的下拉菜单显示`text`。如果它显示`tree`，那么只需点击它，并将其更改为`text`。然后，我们可以将数据放入该行。

我们从 GraphQL 模式开始，为行的每个属性提供一些数据。但是我们还需要添加`createdAt`和`updatedAt`值。您可以使用浏览器控制台找到它。

键入`new Date().toISOString()`并复制其结果。您最终应该得到这样一个对象:

```
{
  "id": "gr4334t4tog345ht35",
  "title": "My First Song",
  "description": "A test song for our amplify app",
  "owner": "Sam Williams",
  "filePath": "",
  "likes": 4,
  "createdAt": "2020-08-13T07:01:39.176Z",
  "updatedAt": "2020-08-13T07:01:39.176Z"
}
```

如果我们保存新对象，那么我们可以返回到我们的应用程序并刷新页面。我们现在应该能够在 console.log 中看到我们的数据。

![Screenshot-2020-08-20-at-07.46.49](img/9be1914fc7a00d511d570f8527996b59.png)

我们现在可以在我们的应用程序中使用这些数据来显示我们刚刚获得的歌曲列表。用这组 JSX 替换现有的文本`song list`。

```
<div className="songList">
    {songs.map((song, idx) => {
        return (
            <Paper variant="outlined" elevation={2} key={`song${idx}`}>
                <div className="songCard">
                    <IconButton aria-label="play">
                        <PlayArrowIcon />
                    </IconButton>
                    <div>
                        <div className="songTitle">{song.title}</div>
                        <div className="songOwner">{song.owner}</div>
                    </div>
                    <div>
                        <IconButton aria-label="like">
                            <FavoriteIcon />
                        </IconButton>
                        {song.likes}
                    </div>
                    <div className="songDescription">{song.description}</div>
                </div>
            </Paper>
        );
    })}
</div>
```

这段代码映射列表中的每首歌曲，并为它们呈现一个新的`Paper`,包含我们需要的所有细节。

我们使用 MaterialUI 库来帮助我们做得更好，所以我们需要确保运行`npm install --save @material-ui/core @material-ui/icons`来安装这些包，然后将它们添加到文件顶部的导入中:

```
import { Paper, IconButton } from '@material-ui/core';
import PlayArrowIcon from '@material-ui/icons/PlayArrow';
import FavoriteIcon from '@material-ui/icons/Favorite';
```

有了这个，如果我们保存并重新加载我们的应用程序，我们现在得到这个:

![Screenshot-2020-08-20-at-07.53.02](img/0965b315203eab7380fa98e3d7f7f470.png)

虽然这没问题，但我们可以更新 CSS，让它看起来更好。打开您的`App.css`文件，并将其更改为:

```
.App {
    text-align: center;
}

.App-logo {
    height: 10vmin;
    pointer-events: none;
}

.App-header {
    background-color: #282c34;
    min-height: 5vh;
    display: flex;
    align-items: center;
    justify-content: space-around;
    font-size: calc(10px + 2vmin);
    color: white;
}

.App-link {
    color: #61dafb;
}

.songList {
    display: flex;
    flex-direction: column;
}

.songCard {
    display: flex;
    justify-content: space-around;
    padding: 5px;
}

.songTitle {
    font-weight: bold;
}
```

现在我们让它看起来像这样——好多了。

![Screenshot-2020-08-20-at-07.54.40](img/fe947d352c4d82a3203c35cab5b0ea89.png)

现在我们在数据库中有一个项目，所以我们只有一个记录。如果我们回到迪纳摩，创建一个新的项目或复制现有的项目，然后我们可以看到多首歌曲看起来如何。您可以通过点按项目左侧的复选框来复制项目

现在我们可以得到数据，那么更新信息呢？为此，我们将添加喜欢一首歌的能力。为此，我们可以在图标按钮上添加一个`onClick`功能。

```
<IconButton aria-label="like" onClick={() => addLike(idx)}>
    <FavoriteIcon />
</IconButton>
```

你可能已经意识到有一种我们以前从未见过的属性。那是 index 的缩写。

在我们做`songs.map`的地方，我们可以稍微更新它以获得列表中每个条目的位置。我们还可以使用这个 idx 向该映射中的顶层`Paper`添加一个键，以删除 React 中的一个错误。

```
{songs.map((song, idx) => {
    return (
        <Paper variant="outlined" elevation={2} key={`song${idx}`}>
            ...
        </Paper>
    )}
)}
```

有了新的索引和对`onClick`函数的调用，我们现在需要创建`addLike`函数。

这个函数需要获取歌曲的索引来找到正确的歌曲，并更新喜欢的数量。然后，在调用操作之前，它会删除一些不能传递到`updateSong`操作中的字段。

```
const addLike = async idx => {
    try {
        const song = songs[idx];
        song.likes = song.likes + 1;
        delete song.createdAt;
        delete song.updatedAt;

        const songData = await API.graphql(graphqlOperation(updateSong, { input: song }));
        const songList = [...songs];
        songList[idx] = songData.data.updateSong;
        setSongs(songList);
    } catch (error) {
        console.log('error on adding Like to song', error);
    }
};
```

一旦数据库中的歌曲被更新，我们需要将更新返回到我们的状态。我们需要使用`const songList = [...songs]`克隆现有的歌曲。

如果我们只是改变了原来的歌曲列表，那么 React 就不会重新呈现页面。有了新的歌曲列表，我们调用`setSongs`来更新我们的状态，我们就完成了这个函数。

我们只需要在文件的顶部再添加一个导入，它是从 Amplify 创建的 mutators 中获得的:

```
import { updateSong } from './graphql/mutations';
```

现在，当我们点击歌曲上的 like 按钮时，它会在状态和数据库中更新。

![ezgif.com-optimize](img/d58f3d303d069624a745358cf956be8f.png)

# 如何添加文件存储

现在我们已经将歌曲数据存储在 Dynamo 中，我们希望将实际的 MP3 数据存储在某个地方。我们将把歌曲存储在 S3，并使用 Amplify 访问它们。

[https://www.youtube.com/embed/ZpdgHjbnef0?feature=oembed](https://www.youtube.com/embed/ZpdgHjbnef0?feature=oembed)

### 如何添加播放/暂停功能

首先，我们将添加播放和暂停每首歌曲的方法。现在，这只是将播放按钮改为暂停按钮。

稍后，我们将使用 Amplify 存储方法从 S3 获取 MP3 文件，并直接在我们的应用程序中播放。

我们将为组件`App`添加一个名为`toggleSong`的新函数。

```
const toggleSong = async idx => {
    if (songPlaying === idx) {
        setSongPlaying('');
        return;
    }
    setSongPlaying(idx);
    return
}
```

为了让这个工作，我们还需要添加一个新的状态变量到应用程序中。这将用于存储当前正在播放的歌曲的索引。

```
const [songPlaying, setSongPlaying] = useState('');
```

有了这个设置，我们需要对 JSX 代码进行更改，以使用我们的新函数和变量。

找到`songCard` div 中的第一个按钮。我们将添加一个调用我们新函数的`onClick`。我们还将使用一个三元等式来说明，如果正在播放的歌曲是这首歌曲，请显示暂停图标，而不是播放图标。

```
<IconButton aria-label="play" onClick={() => toggleSong(idx)}>
    {songPlaying === idx ? <PauseIcon /> : <PlayArrowIcon />}
</IconButton>
```

我们只需要在文件的顶部导入`PauseIcon`就可以了。

```
import PauseIcon from '@material-ui/icons/Pause';
```

![ezgif.com-video-to-gif--2-](img/8a821a5d3ff31cf649ef21dc02a2593e.png)

### 如何添加要放大的存储

接下来，我们需要使用 Amplify CLI 将存储添加到我们的应用程序中。我们可以从进入终端并运行`amplify add storage`开始。这将打开一个 CLI，我们需要在其中选择以下选项。

请选择您的服务= ****内容(图片、音频、视频等。)****
您的资源的友好名称=****song storage****
Bucket name =****song-storage****
谁应该拥有访问权限=****Auth Users Only****
那些用户拥有什么访问权限= ****Read**** 和 ****创建/更新****
您想要一个= ****否****

配置完成后，我们需要部署它。我们可以从`amplify push`开始，这需要一点时间来弄清楚我们在放大器应用中做了哪些改变。然后，您将看到我们在 Amplify 中的所有资源的一个小显示。

唯一的变化是创建了新的`songStorage`资源。我们可以选择 ****是**** 继续，然后它会为我们创建我们的 S3 桶。

### 如何通过放大存储方法访问 S3 文件

一旦部署完成，我们可以开始使用它来访问我们的歌曲。回到我们的`toggleSong`函数中，我们将添加一些额外的逻辑。

```
const toggleSong = async idx => {
    if (songPlaying === idx) {
        setSongPlaying('');
        return;
    }

    const songFilePath = songs[idx].filePath;
    try {
        const fileAccessURL = await Storage.get(songFilePath, { expires: 60 });
        console.log('access url', fileAccessURL);
        setSongPlaying(idx);
        setAudioURL(fileAccessURL);
        return;
    } catch (error) {
        console.error('error accessing the file from s3', error);
        setAudioURL('');
        setSongPlaying('');
    }
};
```

这是从歌曲数据(存储在迪纳摩的[中)获取文件路径，然后使用`Storage.get(songFilePath, { expires: 60 });`获取文件的访问 URL。](https://completecoding.io/amplify-database/)

最后的``{ expires: 60 }`` 是说返回的网址只去 60 秒。之后你就不能用它来访问文件了。这是一个有用的安全措施，这样人们就不能将 URL 分享给其他不应该访问这些文件的人。

一旦我们有了文件，我们也使用`setAudioURL`将它设置在一个新的状态变量中。在我们的`App`的顶部，我们需要添加这个额外的状态。

```
const [audioURL, setAudioURL] = useState('');
```

保存此信息并返回我们的应用程序。如果我们按下播放按钮并打开控制台，我们会看到网址被注销。这是我们要用来演奏这首歌的。

### 如何上传我们的歌曲

我们现在开始需要一些歌曲来播放。如果我们进入 AWS 帐户，搜索`S3`，然后搜索`song`，我们应该会找到新的 S3 桶。

在那里，我们需要创建一个名为`public`的新文件夹。这是因为这些文件将对所有登录该应用的人公开。还有其他存储数据的方法，这些方法是私有的，只有特定的人才能查看。

在这个文件夹中，我们需要上传两首歌曲。我上传了两首没有版权的歌曲，分别叫做`epic.mp3`和`tomorrow.mp3`。一旦它们被上传，我们需要更新我们的迪纳摩数据指向这些歌曲。

在迪纳摩，我们可以找到我们的`Songs-(lots of random charachters)`桌。在`Items`下，我们应该有两个记录。打开第一个，把`filePath`改成`tomorrow.mp3`，名字改成`Tomorrow`。

保存并打开第二首歌曲，将其更改为`"filePath": "epic.mp3"`和`"name": "Epic"`，同时保存该文件。

如果你用的是自己的歌曲，那么只要确保文件路径与你的歌曲的文件名相匹配就可以了。

我们现在可以返回到我们的应用程序，刷新页面，并按下其中一首歌曲的播放。如果我们在控制台中复制给我们的 URL，我们可以将其粘贴到浏览器中，我们的歌曲应该会开始播放。

### 如何向我们的应用添加媒体播放器

现在，我们希望能够在我们的应用程序中播放我们的歌曲。为此，我们将使用一个名为`react-player`的库。我们需要先用`npm install --save react-player`安装。

在我们的应用程序中，我们可以在文件的顶部导入它。

```
import ReactPlayer from 'react-player';
```

如果我们找到了我们的`<div **className**="songCard">`，我们希望在那个组件之后添加我们的播放器。与我们显示播放/暂停图标的方式相同，我们将根据正在播放的歌曲来显示或隐藏该播放器。

```
<div className="songCard">
    .. ..
</div>
{songPlaying === idx ? (
    <div className="ourAudioPlayer">
        <ReactPlayer
            url={audioURL}
            controls
            playing
            height="50px"
            onPause={() => toggleSong(idx)}
        />
    </div>
) : null}
```

`ReactPlayer`有几个参数。`url`只是要播放的文件 URL，也就是我们从 Amplify 存储中得到的那个。`controls`允许用户改变音量或暂停播放歌曲。`playing`意味着歌曲一加载就开始播放，`onPause`是一个听众，所以我们可以让内置的暂停按钮像暂停按钮一样工作。

完成这些后，我们可以再次保存所有内容并打开我们的应用程序。在那里，如果我们按下其中一首歌曲的 play，我们应该会看到一个播放器出现，歌曲将会播放。

![Screenshot-2020-09-11-at-07.00.57](img/aed08185521b65458f1b98133edbe46e.png)

# 如何启用用户上传

现在我们有一个应用程序，允许用户观看所有的歌曲，并从 S3 播放。在此基础上，我们希望允许我们的用户上传他们自己的歌曲到平台上。

[https://www.youtube.com/embed/vYY3cCvJKv0?feature=oembed](https://www.youtube.com/embed/vYY3cCvJKv0?feature=oembed)

我们首先需要创建一个添加歌曲和输入一些信息的方法。我们首先创建一个`Add`按钮。

```
{
    showAddSong ? <AddSong /> : <IconButton> <AddIcon /> </IconButton>
} 
```

然后，我们还需要将`AddIcon`添加到我们的导入中。

```
import AddIcon from '@material-ui/icons/Add'; 
```

现在我们可以继续创建新的`AddSong`组件。我们可以在我们的`App.jsx`文件的底部创建它。

```
const AddSong = () => {
    return (
        <div className="newSong">
            <TextField label="Title" />
            <TextField label="Artist" />
            <TextField label="Description" />
        </div>
    )
} 
```

我们还需要将`TextField`添加到我们从 material UI 的导入中。

```
import { Paper, IconButton, TextField } from '@material-ui/core'; 
```

接下来要做的是通过控制`showAddSong`变量来添加打开新组件的能力。我们需要创建一个新的州声明。

```
const [showAddSong, setShowAddNewSong] = useState(false); 
```

我们现在可以更新新的`AddIcon`按钮，将`showAddSong`设置为 true。

```
<IconButton onClick={() => setShowAddNewSong(true)}>
    <AddIcon />
</IconButton> 
```

要将它改回来，我们可以向名为`onUpload`的`AddSong`组件添加一个参数。当这个被调用时，我们将把 showAddSong 重置为 false。

```
<AddSong
    onUpload={() => {
        setShowAddNewSong(false);
    }}
/> 
```

然后我们需要更新我们的组件来使用新的参数和一个按钮来“上传”新歌。该按钮调用组件中的一个函数，我们将在其中添加上传数据的功能，但现在我们只调用`onUpload`函数。

```
const AddSong = ({ onUpload }) => {
    const uploadSong = async () => {
        //Upload the song
        onUpload();
    };

    return (
        <div className="newSong">
            <TextField
                label="Title"
            />
            <TextField
                label="Artist"
            />
            <TextField
                label="Description"
            />
            <IconButton onClick={uploadSong}>
                <PublishIcon />
            </IconButton>
        </div>
    );
}; 
```

现在，我们将`PublishIcon`添加到我们的导入中，并准备对此进行测试。

```
import PublishIcon from '@material-ui/icons/Publish'; 
```

当我们启动应用程序并登录时，我们现在会看到一个加号图标。通过点击，我们可以输入歌曲的一些细节，然后点击上传。

![photo1-1](img/8eb22ab6eaad788d3ddd4b8277fe5410.png)![photo2-1](img/1d7b514824b00403ad73aa6f2e35228d.png)

### 如何更新 AddSong

现在我们希望能够存储和访问用户在添加歌曲时输入到字段中的数据。

```
const AddSong = ({ onUpload }) => {
    const [songData, setSongData] = useState({});

    const uploadSong = async () => {
        //Upload the song
        console.log('songData', songData);
        const { title, description, owner } = songData;

        onUpload();
    };

    return (
        <div className="newSong">
            <TextField
                label="Title"
                value={songData.title}
                onChange={e => setSongData({ ...songData, title: e.target.value })}
            />
            <TextField
                label="Artist"
                value={songData.owner}
                onChange={e => setSongData({ ...songData, owner: e.target.value })}
            />
            <TextField
                label="Description"
                value={songData.description}
                onChange={e => setSongData({ ...songData, description: e.target.value })}
            />
            <IconButton onClick={uploadSong}>
                <PublishIcon />
            </IconButton>
        </div>
    );
}; 
```

我们还必须更改所有要控制的文本字段，从 out state 传入一个值并提供一个`onChange`。如果我们保存并在上传之前输入一些细节，我们应该会在 chrome 控制台上看到一个详细的 console.log。

接下来，我们需要添加实际上传歌曲的能力。为此，我们将使用文件类型的默认 html 输入。将此添加到上传图标按钮之前的 JSX 中。

```
<input type="file" accept="audio/mp3" onChange={e => setMp3Data(e.target.files[0])} /> 
```

正如你可能已经注意到的，我们呼吁`setMp3Data`变革。这是 AddSong 组件中的更多状态。

```
const [mp3Data, setMp3Data] = useState(); 
```

现在我们已经有了所有需要的数据，我们可以开始上传歌曲到 S3，然后数据到我们的数据库。

为了上传歌曲，我们将再次使用 Amplify 存储类。文件名将是一个 UUID，所以我们还需要在终端中运行`npm install --save uuid`，然后将其导入到文件`import { v4 as uuid } from 'uuid';`的顶部。然后，我们传入 mp3Data 和 contentType，并返回一个带有键的对象。

```
const { key } = await Storage.put(`${uuid()}.mp3`, mp3Data, { contentType: 'audio/mp3' }); 
```

现在我们有了密钥，我们可以在数据库中创建这首歌曲的记录。由于可能有多首歌曲同名，我们将再次使用 UUID 作为 ID。

```
const createSongInput = {
    id: uuid(),
    title,
    description,
    owner,
    filePath: key,
    like: 0,
};
await API.graphql(graphqlOperation(createSong, { input: createSongInput })); 
```

为了让它工作，我们需要导入`createSong` mutator，它是在我们用 Amplify 创建 dynamo 存储时创建的。

```
import { updateSong, createSong } from './graphql/mutations'; 
```

我们需要做的最后一件事是让应用程序在我们完成上传后从数据库中重新获取数据。我们可以通过添加一个`fetchSongs`调用作为`onUpload`函数的一部分来做到这一点。

```
<AddSong
    onUpload={() => {
        setShowAddNewSong(false);
        fetchSongs();
    }}
/>
```

现在，当我们重新加载页面时，我们可以点击添加一首新歌，输入细节，选择我们的新歌，上传它，然后从应用程序播放它。

## 就这些了，伙计们！

如果你喜欢这篇文章(和附带的视频)并想帮我，那么你能做的最好的事情就是订阅我的 Youtube 频道。我每周在 AWS 和 Serverless 上制作视频，帮助你成为更好的开发者。[订阅这里](https://www.youtube.com/c/completecoding?sub_confirmation=1)

* * *