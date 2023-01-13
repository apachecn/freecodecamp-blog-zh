# 将 Socket.io 添加到多线程 Node.js

> 原文：<https://www.freecodecamp.org/news/how-to-add-socket-io-to-multi-threaded-node-js-df404b424276/>

Node 的一个缺点就是单线程。当然，有一种方法可以绕过它——即一个名为**集群**T2 的模块。集群使我们能够将应用程序分布在多个线程上。

然而，现在一个新的问题出现了。看，我们跨多个实例运行的代码实际上有一些明显的缺点。其中之一就是没有全局状态。

通常，在单线程实例中，这不是什么大问题。对我们来说，现在它改变了一切。

我们来看看为什么。

### 那么，问题是什么呢？

我们的应用程序是一个运行在四个线程上的简单在线聊天。这使得用户可以同时在他们的电话和计算机上登录。

想象一下，我们按照为一个线程设置套接字的方式来设置套接字。换句话说，我们现在有了一个拥有套接字的全球大国。

当用户登录他们的计算机时，网站打开与我们服务器上的 Socket.io 实例的连接。套接字存储在线程#3 的状态中。

现在，想象一下用户带着手机去厨房拿零食——很自然地想继续和他们的朋友在网上发短信。

他们的电话连接到线程#4，套接字保存在线程的状态中。

从他们的手机发送信息对用户没有任何好处。只有来自线程#3 的人能够看到该消息。这是因为保存在线程#3 上的套接字并没有神奇地保存在线程#1、#2 和#4 上。

有趣的是，一旦用户从厨房回来，他们自己也不会在电脑上看到他们的信息。

当然，当他们刷新网站时，我们可以发送一个 GET 请求并获取最近的 50 条消息，但我们不能说这是“动态”的方式，不是吗？

### 为什么会这样？

在多线程上分布我们的服务器在某种程度上相当于拥有几个独立的服务器。他们不知道彼此的存在，当然也不分享任何记忆。这意味着一个实例上的对象在另一个实例上不存在。

保存在线程#3 中的套接字不一定是用户此刻正在使用的所有套接字。如果用户的朋友在不同的线程上，他们不会看到用户的消息，除非他们刷新网站。

理想情况下，我们希望将事件通知给用户的其他实例。通过这种方式，我们可以确保每台连接的设备都接收到实时更新。

### 一个解决方案

我们可以通过使用[Redis](https://redis.io/)‘发布/订阅[消息传递范式](https://redis.io/topics/pubsub) ( **pubsub** )来通知其他线程。

**Redis** 是一个开源( **BSD** -licensed)内存数据结构**T5)存储。它可以用作数据库、缓存和消息代理。**

这意味着我们可以使用 Redis 在我们的实例之间分发事件。

请注意，通常我们可能会将整个结构存储在 Redis 中。但是，由于该结构是不可序列化的，并且需要在内存中保持“活动”状态，所以我们将在每个实例中存储它的一部分。

### 流动

现在让我们考虑一下处理传入事件的步骤。

1.  名为 **message** 的事件到达我们的一个套接字——这样，我们就不必监听每一个可能的事件。
2.  在作为参数传递给该事件处理程序的对象内部，我们可以找到事件的名称。比如**发送消息** — `.on('message', ({ event }) =>{})`。
3.  如果这个名字有一个处理程序，我们将执行它。
4.  处理程序可以执行带有响应的**分派**。
5.  调度将响应事件发送到我们的 Redis publibsub*。从那里*它得到**发射**到我们的每一个实例。
6.  每个实例将它发送到它们的 socketsState，确保每个连接的客户端都将接收到该事件。

我知道这看起来很复杂，但请耐心听我说。

### 履行

这里是准备好环境的[库](https://github.com/maciejcieslar/multithreaded-socket)，这样我们就不必自己安装和设置所有的东西。

首先，我们要用 **Express** 设置一个服务器。

```
import * as moduleAlias from 'module-alias';

moduleAlias.addAliases({
  src: __dirname,
});

import * as express from 'express';
import * as http from 'http';
import * as socketio from 'socket.io';

const port = 7999;

const app = express();
const server = http.createServer(app);
const io = initSocket(socketio(server).of('/socket'));

server.listen(port, () => {
  console.log(`Listening on port ${port}.`);
});
```

我们创建了一个 Express 应用程序、HTTP 服务器和 init 套接字。

现在我们可以专注于添加套接字。

我们将Socket.io 的服务器实例传递给我们设置中间件的函数。

```
const initSocket = (instance: socketio.Namespace): socketio.Namespace =>
  instance.use(onAuth).use(onConnection);
```

### oauth

onAuth 函数简单地模仿了一个模拟授权。在我们的例子中，它是基于令牌的。

就我个人而言，将来我可能会用 [JWT](https://jwt.io/) 来代替它，但它不会以任何方式被强制执行。

```
const onAuth: SocketMiddleware = (socket, next) => {
  const { token, id }: { token: string; id: string } =
    socket.request._query || socket.request.headers;

  if (!token) {
    return next(new Error('Authorization failed, no token has been provided!'));
  }

  // mock
  const user = checkToken(token, id);

  socket.user = user;

  return next();
};
```

现在，让我们继续讨论 **onConnection** 中间件。

### on 连接

```
const onConnection: SocketMiddleware = (socket, next) => {
  if (!socket.user) {
    return next(new Error('Something went wrong.'));
  }

  const { id } = socket.user;

  socketsState.add(id, socket);

  socket.on('message', ({ event, args }) => {
    const handler = handlers[event];

    if (!handler) {
      return null;
    }

    return handler && handler({ id, args });
  });

  socket.on('disconnect', () => {
    return socketsState.remove(id, socket);
  });

  return next();
};
```

这里我们看到，我们检索了用户的 **id** ，它是在以前的中间件中设置的，并将其保存在我们的 socketsState 中，键是 id，值是套接字的数组。

接下来，我们监听**消息**事件。我们的整个逻辑都基于此——前端发送给我们的每个事件都将被称为:**消息**。

事件的名称将在 arguments 对象中发送——如上所述。

### 经理人

正如您在 onConnection 中看到的，特别是在 message 事件的侦听器中，我们正在寻找一个基于事件名称的处理程序。

我们的**处理程序**只是一个对象，其中键是事件名，值是函数。我们将使用它来监听事件并做出相应的响应。

```
const dispatchTypes = {
  MESSAGE_SENT: 'message_sent',
  POST_UPDATED_NOTIFICATION: 'post_updated_notification',
};

interface Handlers {
  [key: string]: ({ id, args }: { id: string; args: any }) => any;
}

const handlers: Handlers = {
  sendMessage: async ({ id, args }) => {
    // await sendMessageToUser();

    dispatch({
      id,
      event: dispatchTypes.MESSAGE_SENT,
      args: {
        message: `A message from user with id: ${id} has been send`,
      },
    });
  },
  postUpdated: async ({ id, args }) => {
    dispatch({
      id,
      event: dispatchTypes.POST_UPDATED_NOTIFICATION,
      args: {
        message: 'A post you have been mentioned in has been updated',
      },
    });
  },
};

export = handlers;
```

此外，稍后，我们将添加 **dispatch** 函数，并使用它跨实例发送事件。

### SocketsState

我们知道状态的接口，但是我们还没有实现它。

我们添加了添加和移除套接字的方法，以及发出事件的方法。

```
import * as socketio from 'socket.io';

interface SocketsState {
  [id: string]: socketio.Socket[];
}

const socketsState: SocketsState = {};

const add = (id: string, socket: socketio.Socket) => {
  if (!socketsState[id]) {
    socketsState[id] = [];
  }

  socketsState[id] = [...socketsState[id], socket];

  return socketsState[id];
};

const remove = (id: string, socket: socketio.Socket) => {
  if (!socketsState[id]) {
    return null;
  }

  socketsState[id] = socketsState[id].filter((s) => s !== socket);

  if (!socketsState[id].length) {
    socketsState[id] = undefined;
  }

  return null;
};

const emit = ({
  event,
  id,
  args,
}: {
  event: string;
  id: string;
  args: any;
}) => {
  if (!socketsState[id]) {
    return null;
  }

  socketsState[id].forEach((socket) =>
    socket.emit('message', { event, id, args }),
  );

  return null;
};

export { add, remove, emit }; 
```

**add** 函数检查状态是否有一个等于用户 id 的属性。如果是这种情况，那么我们只需将它添加到现有的数组中。否则，我们首先创建一个新的数组。

**remove** 函数还检查状态的属性中是否有用户的 id。如果没有，它什么也不做。否则，它过滤数组以从数组中移除套接字。然后，如果数组为空，它会将其从状态中移除，将属性设置为**未定义的**。

### 再来一次

为了创建我们的 **pubsub** ，我们将使用名为 **node-redis-pubsub** 的包。

```
import * as NRP from 'node-redis-pubsub';

const client = new NRP({
  port: 6379,
  scope: 'message',
});

export = client;
```

### 添加派单

好了，现在剩下要做的就是添加调度功能…

```
const dispatch = ({
  event,
  id,
  args,
}: {
  event: string;
  id: string;
  args: any;
}) => pubsub.emit('outgoing_socket_message', { event, id, args });
```

…并为 **outgoing_socket_message** 添加一个监听器。这样，每个实例接收事件并将其发送到用户的套接字。

```
pubsub.on('outgoing_socket_message', ({ event, id, args }) =>
  socketsState.emit({ event, id, args }),
);
```

### 使它成为多线程的

最后，让我们添加多线程服务器所需的代码。

```
import * as os from 'os';
import * as cluster from 'cluster';

const spawn = () => {
  const numWorkes = os.cpus().length;

  for (let i = 0; i < numWorkes; i += 1) {
    cluster.fork();
  }

  cluster.on('online', () => {
    console.log('Worker spawned');
  });

  cluster.on('exit', (worker, code, status) => {
    if (code === 0 || worker.exitedAfterDisconnect) {
      console.log(`Worker ${worker.process.pid} finished his job.`);
      return null;
    }

    console.log(
      `Worker ${
        worker.process.pid
      } crashed with code ${code} and status ${status}.`,
    );
    return cluster.fork();
  });
};

export { spawn };
```

```
import * as moduleAlias from 'module-alias';

moduleAlias.addAliases({
  src: __dirname,
});

import * as express from 'express';
import * as http from 'http';
import * as cluster from 'cluster';
import * as socketio from 'socket.io';
import * as killPort from 'kill-port';
import { initSocket } from 'src/common/socket';
import { spawn } from 'src/clusters';

const port = 7999;

if (cluster.isMaster) {
  killPort(port).then(spawn);
} else {
  const app = express();
  const server = http.createServer(app);
  const io = initSocket(socketio(server).of('/socket'));

  server.listen(port, () => {
    console.log(`Listening on port ${port}.`);
  });
} 
```

注意:我们必须杀死这个端口，因为在用 Ctrl + c 退出我们的 **Nodemon** 进程后，它就挂在那里了。

稍加调整，我们现在在所有实例中都有了工作套接字。结果是:更高效的服务器。

非常感谢您的阅读！

我明白，一开始这一切可能看起来势不可挡，一下子全理解起来很吃力。考虑到这一点，我强烈建议您再次完整地阅读代码，并将其作为一个整体来思考。

如果你有任何问题或意见，欢迎在下面的评论区提出，或者给我发[消息](https://www.mcieslar.com/contact)。

查看我的[社交媒体](https://www.maciejcieslar.com/about/)！

[加入我的简讯](http://eepurl.com/dAKhxb)！

*最初发布于 2018 年 9 月 10 日[www.mcieslar.com](https://www.mcieslar.com/adding-socket-io-to-multithreaded-node)。*