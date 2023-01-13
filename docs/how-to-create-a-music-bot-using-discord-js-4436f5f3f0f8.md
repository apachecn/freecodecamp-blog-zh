# 如何使用 Discord.js 创建音乐机器人

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-music-bot-using-discord-js-4436f5f3f0f8/>

discord API 为您提供了一个创建和使用自己的机器人和工具的简单工具。

今天，我们将看看如何创建一个基本的音乐机器人，并将其添加到我们的服务器。该机器人将能够播放、跳过和停止音乐，还将支持排队功能。

## 先决条件

在我们开始创建机器人之前，请确保您已经安装了所有需要的工具。

*   [节点](https://nodejs.org/en/)
*   [NPM](https://www.npmjs.com/)
*   [FFMPEG](https://www.ffmpeg.org/)

安装完成后，我们可以继续设置我们的不和谐机器人。

## 设置一个不和谐机器人

首先，我们需要在 discord 开发门户上创建一个新的应用程序。

我们可以通过访问[门户](https://discordapp.com/developers/applications/)并点击新应用程序来实现。

![Creating a new application](img/1162683d6fbf7ace7d314e3d9a573e20.png)

Creating a new application

之后，我们需要为我们的应用程序命名，并单击 create 按钮。

![Creating a discord bot](img/0c96343f36f65227461445736b159a73.png)

Creating a discord bot

之后，我们需要选择 bot 选项卡并单击 add bot。

![Discord bot information page](img/5605313ed79d583f612f887a4f8fcc30.png)

Discord bot information page

现在我们的机器人已经创建好了，我们可以继续邀请它加入我们的服务器。

### 将 bot 添加到服务器

创建我们的 bot 后，我们可以使用 OAuth2 URL 生成器邀请它。

为此，我们需要导航到 OAuth2 页面，并在 scope tap 中选择 bot。

![OAuth2 tab](img/14237e08b4de1c265c8207a9d87dd77c.png)

OAuth2 tab

之后，我们需要选择播放音乐和阅读消息所需的权限。

![Giving the discord bot the needed permissions](img/62db71449106becd3cc44506ac401042.png)

Giving the discord bot the needed permissions

然后，我们可以复制生成的 URL，并将其粘贴到浏览器中。

![Discord bot invite link](img/4f42cd7841fb5ec789ec6ec08730533f.png)

Discord bot invite link

粘贴后，我们通过选择服务器并单击 authorize 按钮将它添加到我们的服务器。

![Invite the bot to the server](img/21acb6dea9e89d28de79fe6c8f2b4358.png)

Invite the bot to the server

### 创建我们的项目

现在我们可以开始使用我们的终端创建我们的项目。

首先，我们创建一个目录并移入其中。我们可以通过使用这两个命令来实现。

```
mkdir musicbot && cd musicbot
```

之后，我们可以使用 npm init 命令创建我们的项目模块。输入命令后，您会被问及一些问题，请回答并继续。

然后，我们只需要创建我们将要工作的两个文件。

```
touch index.js && touch config.json
```

现在我们只需要在文本编辑器中打开我们的项目。我个人使用 VS 代码，可以用下面的命令打开。

```
code .
```

### 不和谐 js 基础

现在我们只需要在开始之前安装一些依赖项。

```
npm install discord.js ffmpeg fluent-ffmpeg @discordjs/opus ytdl-core --save
```

安装完成后，我们可以继续编写 config.json 文件。这里我们保存了我们的机器人的令牌和他应该监听的前缀。

```
{
"prefix": "!",
"token": "your-token"
}
```

要获得您的令牌，您需要再次访问 discord 开发者门户，并从 bot 部分复制它。

![Copy token](img/8f5bd27fb71c39447d3828324bf4eb72.png)

Copy token

这是我们在 config.json 文件中唯一需要做的事情。所以让我们开始编写 javascript 代码。

首先，我们需要导入所有的依赖项。

```
const Discord = require('discord.js');
const {
	prefix,
	token,
} = require('./config.json');
const ytdl = require('ytdl-core');
```

之后，我们可以创建我们的客户端并使用我们的令牌登录。

```
const client = new Discord.Client();
client.login(token);
```

现在让我们添加一些基本的侦听器，它们在执行时会生成 console.log。

```
client.once('ready', () => {
 console.log('Ready!');
});
client.once('reconnecting', () => {
 console.log('Reconnecting!');
});
client.once('disconnect', () => {
 console.log('Disconnect!');
});
```

之后，我们可以使用 node 命令启动我们的机器人，他应该在 discord 上联机并打印“Ready！”在控制台里。

```
node index.js
```

### 阅读邮件

现在我们的机器人已经在我们的服务器上了，可以上网了，我们可以开始阅读聊天信息并回复它们了。

要读取消息，我们只需要编写一个简单的函数。

```
client.on('message', async message => {

}
```

在这里，我们为消息事件创建一个监听器，如果消息被触发，就获取消息并保存到消息对象中。

现在我们需要检查消息是否来自我们自己的机器人，如果是，就忽略它。

```
if (message.author.bot) return;
```

在这一行中，我们检查消息的作者是否是我们的 bot，如果是，则返回。

之后，我们检查消息是否以我们之前定义的前缀开头，如果不是，则返回。

```
if (!message.content.startsWith(prefix)) return;
```

之后，我们可以检查我们需要执行哪个命令。我们可以使用一些简单的 if 语句来做到这一点。

```
const serverQueue = queue.get(message.guild.id);

if (message.content.startsWith(`${prefix}play`)) {
    execute(message, serverQueue);
    return;
} else if (message.content.startsWith(`${prefix}skip`)) {
    skip(message, serverQueue);
    return;
} else if (message.content.startsWith(`${prefix}stop`)) {
    stop(message, serverQueue);
    return;
} else {
    message.channel.send("You need to enter a valid command!");
}
```

在这个代码块中，我们检查要执行哪个命令并调用该命令。如果输入命令无效，我们使用****send()****功能将错误消息写入聊天。

现在我们知道了需要执行哪个命令，我们可以开始实现这些命令了。

### 添加歌曲

让我们从添加 play 命令开始。为此，我们需要一首歌和一个公会(一个公会代表一个孤立的用户和频道集合，通常被称为服务器)。我们还需要之前安装的 ytdl 库。

首先，我们需要创建一个带有队列名称的地图，在这里保存我们在聊天中输入的所有歌曲。

```
const queue = new Map();
```

之后，我们创建一个名为 execute 的异步函数，检查用户是否在进行语音聊天，以及机器人是否有正确的权限。如果没有，我们写一个错误信息并返回。

```
async function execute(message, serverQueue) {
  const args = message.content.split(" ");

  const voiceChannel = message.member.voice.channel;
  if (!voiceChannel)
    return message.channel.send(
      "You need to be in a voice channel to play music!"
    );
  const permissions = voiceChannel.permissionsFor(message.client.user);
  if (!permissions.has("CONNECT") || !permissions.has("SPEAK")) {
    return message.channel.send(
      "I need the permissions to join and speak in your voice channel!"
    );
  }
}
```

现在我们可以继续获取歌曲信息，并将其保存到一个歌曲对象中。为此，我们使用我们的 ytdl 库，它从 youtube 链接中获取歌曲信息。

```
const songInfo = await ytdl.getInfo(args[1]);
const song = {
 title: songInfo.title,
 url: songInfo.video_url,
};
```

这将使用我们之前安装的****ytdl****库来获取歌曲的信息。然后，我们将需要的信息保存到一个 song 对象中。

保存歌曲信息后，我们只需要创建一个可以添加到队列中的合同。为此，我们首先需要检查我们的 serverQueue 是否已经定义，这意味着音乐已经在播放。如果是这样，我们只需要将歌曲添加到现有的 serverQueue 中，并发送一条成功消息。如果没有，我们需要创建它，并尝试加入语音通道，开始播放音乐。

```
if (!serverQueue) {

}else {
 serverQueue.songs.push(song);
 console.log(serverQueue.songs);
 return message.channel.send(`${song.title} has been added to the queue!`);
}
```

在这里，我们检查 ****服务器队列**** 是否为空，如果不是，就将歌曲添加到其中。现在，如果****server queue****为空，我们只需要创建我们的合同。

```
// Creating the contract for our queue
const queueContruct = {
 textChannel: message.channel,
 voiceChannel: voiceChannel,
 connection: null,
 songs: [],
 volume: 5,
 playing: true,
};
// Setting the queue using our contract
queue.set(message.guild.id, queueContruct);
// Pushing the song to our songs array
queueContruct.songs.push(song);

try {
 // Here we try to join the voicechat and save our connection into our object.
 var connection = await voiceChannel.join();
 queueContruct.connection = connection;
 // Calling the play function to start a song
 play(message.guild, queueContruct.songs[0]);
} catch (err) {
 // Printing the error message if the bot fails to join the voicechat
 console.log(err);
 queue.delete(message.guild.id);
 return message.channel.send(err);
}
```

在这个代码块中，我们创建了一个契约并将我们的歌曲添加到 songs 数组中。之后，我们尝试加入用户的语音聊天，调用我们的****play()****功能，我们将在之后实现。

### 播放歌曲

现在，我们可以将歌曲添加到队列中，并创建一个合同(如果还没有合同的话),我们可以开始实现播放功能了。

首先，我们将创建一个名为 play 的函数，它接受两个参数(公会和我们想要播放的歌曲)并检查歌曲是否为空。如果是这样，我们将离开语音频道并删除队列。

```
function play(guild, song) {
  const serverQueue = queue.get(guild.id);
  if (!song) {
    serverQueue.voiceChannel.leave();
    queue.delete(guild.id);
    return;
  }
}
```

之后，我们将使用连接的 play()函数开始播放我们的歌曲，并传递我们歌曲的 URL。

```
const dispatcher = serverQueue.connection
    .play(ytdl(song.url))
    .on("finish", () => {
        serverQueue.songs.shift();
        play(guild, serverQueue.songs[0]);
    })
    .on("error", error => console.error(error));
dispatcher.setVolumeLogarithmic(serverQueue.volume / 5);
serverQueue.textChannel.send(`Start playing: **${song.title}**`);
```

这里我们创建了一个流，并把我们歌曲的 URL 传递给它。我们还添加了两个侦听器来处理结束和错误事件。

********注意:******** 这是一个递归函数，意思是它反复调用自己。我们使用递归，所以当歌曲结束时，它播放下一首歌曲。

现在我们只需输入就可以播放一首歌曲了！在聊天中播放网址。

### 跳过歌曲

现在我们可以开始实现跳过功能。为此，我们只需结束我们在****play()****函数中创建的调度程序，它就会开始播放下一首歌曲。

```
function skip(message, serverQueue) {
  if (!message.member.voice.channel)
    return message.channel.send(
      "You have to be in a voice channel to stop the music!"
    );
  if (!serverQueue)
    return message.channel.send("There is no song that I could skip!");
  serverQueue.connection.dispatcher.end();
} 
```

在这里，我们检查键入命令的用户是否在语音频道中，以及是否有要跳过的歌曲。

### 停止歌曲

****stop()****函数与****skip()****函数几乎相同，只是我们清除了歌曲数组，这将使我们的 bot 删除队列并离开语音聊天。

```
function stop(message, serverQueue) {
  if (!message.member.voice.channel)
    return message.channel.send(
      "You have to be in a voice channel to stop the music!"
    );
  serverQueue.songs = [];
  serverQueue.connection.dispatcher.end();
}
```

### index.js 的完整源代码:

在这里，您可以获得我们音乐机器人的完整源代码:

```
const Discord = require("discord.js");
const { prefix, token } = require("./config.json");
const ytdl = require("ytdl-core");

const client = new Discord.Client();

const queue = new Map();

client.once("ready", () => {
  console.log("Ready!");
});

client.once("reconnecting", () => {
  console.log("Reconnecting!");
});

client.once("disconnect", () => {
  console.log("Disconnect!");
});

client.on("message", async message => {
  if (message.author.bot) return;
  if (!message.content.startsWith(prefix)) return;

  const serverQueue = queue.get(message.guild.id);

  if (message.content.startsWith(`${prefix}play`)) {
    execute(message, serverQueue);
    return;
  } else if (message.content.startsWith(`${prefix}skip`)) {
    skip(message, serverQueue);
    return;
  } else if (message.content.startsWith(`${prefix}stop`)) {
    stop(message, serverQueue);
    return;
  } else {
    message.channel.send("You need to enter a valid command!");
  }
});

async function execute(message, serverQueue) {
  const args = message.content.split(" ");

  const voiceChannel = message.member.voice.channel;
  if (!voiceChannel)
    return message.channel.send(
      "You need to be in a voice channel to play music!"
    );
  const permissions = voiceChannel.permissionsFor(message.client.user);
  if (!permissions.has("CONNECT") || !permissions.has("SPEAK")) {
    return message.channel.send(
      "I need the permissions to join and speak in your voice channel!"
    );
  }

  const songInfo = await ytdl.getInfo(args[1]);
  const song = {
    title: songInfo.title,
    url: songInfo.video_url
  };

  if (!serverQueue) {
    const queueContruct = {
      textChannel: message.channel,
      voiceChannel: voiceChannel,
      connection: null,
      songs: [],
      volume: 5,
      playing: true
    };

    queue.set(message.guild.id, queueContruct);

    queueContruct.songs.push(song);

    try {
      var connection = await voiceChannel.join();
      queueContruct.connection = connection;
      play(message.guild, queueContruct.songs[0]);
    } catch (err) {
      console.log(err);
      queue.delete(message.guild.id);
      return message.channel.send(err);
    }
  } else {
    serverQueue.songs.push(song);
    return message.channel.send(`${song.title} has been added to the queue!`);
  }
}

function skip(message, serverQueue) {
  if (!message.member.voice.channel)
    return message.channel.send(
      "You have to be in a voice channel to stop the music!"
    );
  if (!serverQueue)
    return message.channel.send("There is no song that I could skip!");
  serverQueue.connection.dispatcher.end();
}

function stop(message, serverQueue) {
  if (!message.member.voice.channel)
    return message.channel.send(
      "You have to be in a voice channel to stop the music!"
    );
  serverQueue.songs = [];
  serverQueue.connection.dispatcher.end();
}

function play(guild, song) {
  const serverQueue = queue.get(guild.id);
  if (!song) {
    serverQueue.voiceChannel.leave();
    queue.delete(guild.id);
    return;
  }

  const dispatcher = serverQueue.connection
    .play(ytdl(song.url))
    .on("finish", () => {
      serverQueue.songs.shift();
      play(guild, serverQueue.songs[0]);
    })
    .on("error", error => console.error(error));
  dispatcher.setVolumeLogarithmic(serverQueue.volume / 5);
  serverQueue.textChannel.send(`Start playing: **${song.title}**`);
}

client.login(token); 
```

## 结论

你一直坚持到最后！希望这篇文章能帮助您理解 Discord API 以及如何使用它来创建一个简单的 bot。如果你想看一个更先进的不和谐机器人的例子，你可以访问我的 Github 库。

如果您发现这很有用，请考虑推荐并与其他开发人员分享。

如果你有任何问题或反馈，请在下面的评论中告诉我。