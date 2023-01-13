# 如何使用带插座的 Laravel？超正析象管(Image Orthicon)

> 原文：<https://www.freecodecamp.org/news/how-to-use-laravel-with-socket-io-e7c7565cc19d/>

阿德南·萨巴诺维奇

# 如何使用带插座的 Laravel？超正析象管(Image Orthicon)

![7aJMZ4WrmXOVbNW82Og4w-SoKbHAlPK5TvCB](img/7749b24971f84fd895db81847a9f502c.png)

Image borrowed from Code Tutorials

**Websockets** 很酷。如果您想显示用户的实时活动(或者一些队列作业)，它们真的很有帮助。

现在，如果你害怕“ *Websockets* ”这个词，不要害怕。我会告诉你如何使用它，如果你需要的话，我会回答你的问题。

我遇到了这个挑战，我需要它来显示当前正在查看 **Laravel** 中特定 URL 的人的列表。所以我开始思考。我的一部分想做一个快速的黑客(幸运的是，这不是我最强的一面)。而另一个人想做一些很酷的、可重复使用的、持久耐用的东西。

### “为什么不直接用 Pusher？”

事情是这样的。

Laravel 配有推动器。尽管*推送器*看起来像是一个快速的*即插即用*解决方案(确实如此)，但它也有局限性。https://pusher.com/pricing[退房](https://pusher.com/pricing)

大多数教程用实现 Websockets 的标题来欺骗你，而实际上他们只是想给你一个 Pusher。(我最喜欢的部分是他们说你可以轻松地切换到 socket.io)

### “我们希望拥有无限数量的连接”

> 我们不想担心局限性。

我们开始吧。

我用的是流浪者/家园。

为此，我们需要阅读关于[事件广播](https://laravel.com/docs/5.6/broadcasting)的内容。

此处需要注意的事项(因此我不必重复):

**1。**事件的 ShouldBroadcast 接口

**2。**启用广播路由并使用 routes/channels.php 验证用户

**3。**公共频道—每个人都可以收听

**4。**私人渠道—在用户加入渠道之前，您需要对他们进行授权

**5。**Presence Channel——类似于 Private，但您可以在该通道上传递大量附加元数据，并获得已加入 channel.broadcastOn()事件方法的人员列表

### 创建您的活动

```
php artisan make:event MessagePushed
```

您甚至可以遵循事件广播文档中的特定示例。(我们确实应该这样做)。

### 安装 Redis

在此之前，我实际上使用 Supervisor/Redis/Horizon 设置了队列。地平线很棒，你可以在这里找到相关信息[https://laravel.com/docs/5.6/horizon](https://laravel.com/docs/5.6/horizon)

一旦让队列工作，MessagePushed 事件将需要使用队列。

**注意**:要使所有这些工作正常进行，请确保编辑您的。环境文件:

```
BROADCAST_DRIVER=redis
```

```
QUEUE_DRIVER=redis
```

```
(this is from the horizon setup actually, but we will need that for later)
```

```
REDIS_HOST=127.0.0.1
```

```
REDIS_PASSWORD=null
```

```
REDIS_PORT=6379
```

### 安装 Laravel Echo 服务器

因此，这部分实际上是我们安装 socket.io 服务器的地方，该服务器捆绑在 laravel-echo-server 中。你可以在这里找到它:[https://github.com/tlaverdure/laravel-echo-server](https://github.com/tlaverdure/laravel-echo-server)

**注意**:检查顶部的要求！

运行以下程序(如文档中所述)

```
npm install -g laravel-echo-server
```

然后运行 init，以便在应用程序根中生成 laravel-echo-server.json 文件(我们需要对其进行配置)。

```
laravel-echo-server init
```

一旦生成了 laravel-echo-server.json 文件，它应该看起来像这样。

```
{
```

```
"authHost": "http://local-website.app",
```

```
"authEndpoint": "/broadcasting/auth",
```

```
"clients": [
```

```
{
```

```
"appId": "my-app-id",
```

```
"key": "my-key-generated-with-init-command"
```

```
}
```

```
],
```

```
"database": "redis",
```

```
"databaseConfig": {
```

```
"redis": {},
```

```
"sqlite": {
```

```
"databasePath": "/database/laravel-echo-server.sqlite"
```

```
},
```

```
"port": "6379",
```

```
"host": "127.0.0.1"
```

```
},
```

```
"devMode": false,
```

```
"host": null,
```

```
"port": "6001",
```

```
"protocol": "http",
```

```
"socketio": {},
```

```
"sslCertPath": "",
```

```
"sslKeyPath": "",
```

```
"sslCertChainPath": "",
```

```
"sslPassphrase": ""
```

```
}
```

**注意**:如果你想把这个推送到你的公共服务器上，一定要把**的 laravel-echo-server.json** 添加到你的 **.gitignore. G** 服务器上生成这个文件，否则你需要一直改变你的 authHost。

*运行您的 Laravel Echo 服务器*

你必须运行它才能启动 websockets。

```
laravel-echo-server start 
```

(在您的根目录中——放置 laravel-echo-server.json 的位置)

应该能成功启动。(现在我们要将它添加到您服务器上的 supervisor，这样它会自动启动，并在崩溃时重新启动)

在您的**/etc/supervisor/conf . d/laravel-echo . conf**(只需在您的 **conf.d** 文件夹中创建该文件)中放置以下内容:

```
[program:laravel-echo]
```

```
directory=/var/www/my-website-folder
```

```
process_name=%(program_name)s_%(process_num)02d
```

```
command=laravel-echo-server start
```

```
autostart=true
```

```
autorestart=true
```

```
user=your-linux-user
```

```
numprocs=1
```

```
redirect_stderr=true
```

```
stdout_logfile=/var/www/my-website-folder/storage/logs/echo.log
```

一旦你确定了自己的位置，你就可以跑了

```
pwd
```

获取上面的“目录”路径和“stdout_logfile”前缀。

你的用户将是你的 Linux 用户(流浪者或 Ubuntu 或其他)

保存文件，然后出去。如果你使用的是 vim laravel-echo.conf，那么在里面的时候，按下键盘上的 I(比如伊斯坦布尔),用 vim 编辑一个文件，然后按下 ESC 键:wq！关闭文件并保存它。

接下来，我们需要运行以下命令:

```
sudo supervisorctl stop all sudo supervisorctl reread
```

```
sudo supervisorctl reload
```

然后检查 laravel echo 是否正在运行

```
sudo supervisorctl status
```

### 安装 Laravel Echo 和 Socket IO 客户端

```
npm install --save laravel-echo
```

```
npm install --save socket.io-client
```

然后在 bootstrap.js(我用的是 Vue js)中注册你的 Echo

```
import Echo from "laravel-echo"window.io = require('socket.io-client');
```

```
// Have this in case you stop running your laravel echo server
```

```
if (typeof io !== 'undefined') {  window.Echo = new Echo({    broadcaster: 'socket.io',    host: window.location.hostname + ':6001',  });}
```

现在再次检查如何在特定频道上收听您的事件。

根据我们上面分享的关于 Laravel Broadcasting 的文档，如果您设置您的 **broadcastOn()** 方法来返回一个**新的 PresenceChannel** (我将解释我所做的特定情况，但是如果您需要实现其他内容，请随时提问。我发现这比简单地使用一个公共通道更复杂，所以我们可以毫无问题地缩小规模)然后我们想在 Javascript 端(前端)监听那个通道。

这里有一个具体的例子:

1.  我将一个事件推送到一个在线频道上(我正在处理调查)

```
public function broadcastOn()
```

```
{
```

```
return new PresenceChannel('survey.' . $this->survey->id);
```

```
}
```

2.在你推动这个事件之后，它会通过 channels.php。在这里，我们想为这个用户创建一个授权。(记住返回一个用于存在通道授权的数组，而不是一个布尔值。)

```
Broadcast::channel('survey.{survey_id}', function ($user, $survey_id) {
```

```
return [
```

```
'id' => $user->id,
```

```
'image' => $user->image(),
```

```
'full_name' => $user->full_name
```

```
];
```

```
});
```

3.然后，在我的 VueJs 组件中，加载到我想要监控的页面上，我定义了一个方法，该方法将在加载时从 created()方法启动:

```
listenForBroadcast(survey_id) {
```

```
Echo.join('survey.' + survey_id)
```

```
.here((users) => {
```

```
this.users_viewing = users;
```

```
this.$forceUpdate();
```

```
})
```

```
.joining((user) => {
```

```
if (this.checkIfUserAlreadyViewingSurvey(user)) {
```

```
this.users_viewing.push(user);
```

```
this.$forceUpdate();
```

```
}
```

```
})
```

```
.leaving((user) => {
```

```
this.removeViewingUser(user);
```

```
this.$forceUpdate();
```

```
});
```

```
},
```

显然，我从上下文中提取了一些代码，但是我有这个“users_viewing”数组来保存我当前加入频道的用户。

那就真的是了。

我试图尽可能详细地描述，希望你能够理解。

编码快乐！

在 [Twitter](https://twitter.com/adnanxteam)
上关注我，在 [LinkedIn](https://www.linkedin.com/in/adnansabanovic) 上添加我