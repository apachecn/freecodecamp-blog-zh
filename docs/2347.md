# 如何用 React 构建 YouTube 克隆

> 原文：<https://www.freecodecamp.org/news/build-youtube-with-react/>

在本教程中，您将深入了解如何使用 React 通过 10 个步骤构建一个完整的 YouTube 克隆。

我将展示我是如何克隆 YouTube web 应用程序的，以及你可以采取的具体步骤，以便与其他类似的基于视频的应用程序一起构建自己的应用程序。

通过本指南，我们将介绍如何使用一系列基本技术通过 React 和 Node 构建强大的 web 应用程序，以及每个工具如何有助于创建我们的整体应用程序功能。

我们开始吧！

### 想要像这样用 React 构建惊人的应用程序吗？

[加入现实世界 React app 课程系列](https://bit.ly/12-react-projects)。在其中，您将学习如何从零开始每月构建一个令人印象深刻的全栈 React 项目。

## 步骤 1:建模我们的数据并创建我们的数据库

我们的应用程序由两个主要部分组成，我们的节点后端和我们的反应前端。

我们的后端将负责用户登录的认证和授权，并确保他们可以访问正确的内容。它还将负责提供我们的视频数据(如视频本身以及我们是否喜欢它)和用户相关数据(如每个用户的个人资料)。

后端将通过与我们的数据库交互来完成所有这些事情。我们将要使用的数据库是 SQL 数据库 Postgres。一个叫做 Prisma 的工具将负责建模这些数据(告诉我们的数据库它将存储什么数据)。

> Prisma 是众所周知的对象关系映射器。它负责管理我们的数据在数据库中的结构，包括我们所有的数据通过**模型**相互共享的关系。

我们的应用程序将由六个主要数据模型组成:`User`、`Comment`、`Subscription`、`Video`、`VideoLike`和`View`数据。

您可以在下面看到我们模式的最终版本:

```
// prisma.schema

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id           String         @id @default(uuid())
  createdAt    DateTime       @default(now())
  username     String
  email        String         @unique
  avatar       String         @default("https://reedbarger.nyc3.digitaloceanspaces.com/default-avatar.png")
  cover        String         @default("https://reedbarger.nyc3.digitaloceanspaces.com/default-cover-banner.png")
  about        String         @default("")
  videos       Video[]
  videoLikes   VideoLike[]
  comments     Comment[]
  subscribers  Subscription[] @relation("subscriber")
  subscribedTo Subscription[] @relation("subscribedTo")
  views        View[]
}

model Comment {
  id        String   @id @default(uuid())
  createdAt DateTime @default(now())
  text      String
  userId    String
  videoId   String
  user      User     @relation(fields: [userId], references: [id])
  video     Video    @relation(fields: [videoId], references: [id])
}

model Subscription {
  id             String   @id @default(uuid())
  createdAt      DateTime @default(now())
  subscriberId   String
  subscribedToId String
  subscriber     User     @relation("subscriber", fields: [subscriberId], references: [id])
  subscribedTo   User     @relation("subscribedTo", fields: [subscribedToId], references: [id])
}

model Video {
  id          String      @id @default(uuid())
  createdAt   DateTime    @default(now())
  title       String
  description String?
  url         String
  thumbnail   String
  userId      String
  user        User        @relation(fields: [userId], references: [id])
  videoLikes  VideoLike[]
  comments    Comment[]
  views       View[]
}

model VideoLike {
  id        String   @id @default(uuid())
  createdAt DateTime @default(now())
  like      Int      @default(0)
  userId    String
  videoId   String
  user      User     @relation(fields: [userId], references: [id])
  video     Video    @relation(fields: [videoId], references: [id])
}

model View {
  id        String   @id @default(uuid())
  createdAt DateTime @default(now())
  userId    String?
  videoId   String
  user      User?    @relation(fields: [userId], references: [id])
  video     Video    @relation(fields: [videoId], references: [id])
}
```

这些模型中的每一个都包括各种属性及其关联的数据类型。

每个模型的第一列是不同的字段或每个模型包含的单独属性，例如数据库创建给定条目时的`id`或唯一标识符或`createdAt`时间戳。

> 您可以将每个模型视为一种特殊类型的 JavaScript 对象，它具有我们在模式中创建的特殊属性。

如果我们看第二列，我们可以看到每个字段的数据类型必须是什么。这些值很大程度上对应于普通的 JavaScript 类型:字符串、整数和日期。

关联类型也可以是不同的数据模型。例如，查看我们的`User`模型，我们看到它有一个`videos`字段，字段的数据类型为`Video[]`，这意味着它是一个数据类型为`Video`的数组。

这很有意义——从逻辑上讲，每个用户都可以拥有自己创作的多个视频。这同样适用于他们的赞、评论、订阅者、他们订阅的用户以及他们的视频观看。

## 步骤 2:创建授权、视频和用户路线

现在我们已经创建了模式，我们可以为后端创建业务逻辑了。

我们将在 library Express 中使用 Node 来构建我们的后端。Express 使得构建强大的 API 变得非常容易，这正是我们的 YouTube 应用程序所需要的。

我们的 API 的最大部分将是路由，或者我们的 React 应用程序将向其发出数据请求的单个端点。我们将为身份验证、视频和用户相关资源提供单独的路由，具体如下:

```
http://localhost:3001/api/v1/auth
http://localhost:3001/api/v1/videos
http://localhost:3001/api/v1/users
```

我不会一一介绍我们需要创建的每条路线，但为了让您了解其中一条路线的大致情况，我们来看看与视频相关的路线。

```
// server/src/routes/video.js

import { PrismaClient } from "@prisma/client";
import express from "express";

const prisma = new PrismaClient();

function getVideoRoutes() {
  const router = express.Router();

  router.get("/", getRecommendedVideos);
  router.get("/trending", getTrendingVideos);

  // ... many more routes omitted

  return router;
}

export async function getVideoViews(videos) {
  for (const video of videos) {
    const views = await prisma.view.count({
      where: {
        videoId: {
          equals: video.id,
        },
      },
    });
    video.views = views;
  }
  return videos;
}

async function getRecommendedVideos(req, res) {
  let videos = await prisma.video.findMany({
    include: {
      user: true,
    },
    orderBy: {
      createdAt: "desc",
    },
  });

  if (!videos.length) {
    return res.status(200).json({ videos });
  }

  videos = await getVideoViews(videos);

  res.status(200).json({ videos });
}

async function getTrendingVideos(req, res) {
  let videos = await prisma.video.findMany({
    include: {
      user: true,
    },
    orderBy: {
      createdAt: "desc",
    },
  });

  if (!videos.length) {
    return res.status(200).json({ videos });
  }

  videos = await getVideoViews(videos);
  videos.sort((a, b) => b.views - a.views);

  res.status(200).json({ videos });
}
```

我们使用功能`getVideoRoutes`使用`express.Router`将所有子路线添加到主路线(`/api/v1/videos`)。我们通过使用适当的方法指定可以向其发出什么类型的请求来创建单独的路由:`get`、`post`、`put`或`delete`。

我们向该方法传递我们希望前端向哪个端点发出请求，以及一个处理对该端点的任何传入请求的函数。

> 我们的路由下面的这些用于处理每个 API 端点请求的函数通常被称为**控制器**。

你可以看到我们在这里使用的一些控制器，如`getRecommendedVideos`或`getTrendingVideos`。它们的名字清楚地表明了它们的功能。

例如，如果我们的 React 应用程序向`/api/v1/videos/`发出 GET 请求，我们的控制器会用用户推荐的视频进行响应。

> 注意，在每个控制器中，我们使用`PrismaClient`与我们的数据库交互，该数据库是基于我们创建的`prisma.schema`文件生成的。

对于我们的`getRecommendedVideos`控制器，我们使用`findMany`方法来获取许多视频(它们的数组)，其中包括每个视频的用户数据(对于`user`字段使用`include`操作符)。

我们按照从最新到最老的`createdAt`字段对结果进行排序(使用`desc`或降序)。

## 步骤 3:用中间件保护授权路由

除了我们的控制器之外，我们还需要一些重要的中间件来关联我们的一些路线。

> 什么是中间件？**中间件**是用于在另一个函数之前运行的函数，以提供一个值或执行一个动作。在我们的例子中，对于每个路由，中间件将在我们的控制器功能之前运行。

当用户想要获得他们喜欢的视频时，我们首先需要编写一些中间件，在我们的控制器试图用用户数据进行响应之前获取当前用户。

```
// server/src/routes/user.js

import { PrismaClient } from "@prisma/client";
import express from "express";
import { protect } from "../middleware/authorization";

const prisma = new PrismaClient();

function getUserRoutes() {
  const router = express.Router();

  router.get("/liked-videos", protect, getLikedVideos);

  return router;
}
```

`protect`中间件放在`getLikedVideos`之前，意味着它会先运行。

`protect`功能的代码如下:

```
// server/src/middleware/authorization.js

import { PrismaClient } from "@prisma/client";
import jwt from "jsonwebtoken";

const prisma = new PrismaClient();

export async function protect(req, res, next) {
  if (!req.cookies.token) {
    return next({
      message: "You need to be logged in to visit this route",
      statusCode: 401,
    });
  }

  try {
    const token = req.cookies.token;
    const decoded = jwt.verify(token, process.env.JWT_SECRET);

    const user = await prisma.user.findUnique({
      where: {
        id: decoded.id,
      },
      include: {
        videos: true,
      },
    });

    req.user = user;
    next();
  } catch (error) {
    next({
      message: "You need to be logged in to visit this route",
      statusCode: 401,
    });
  }
}
```

在我们的`protect`中间件函数中，如果我们没有用户，或者如果用户有一个无效的 JSON Web 令牌，我们使用`next`函数以 401 错误响应客户端。

> 401 错误代码意味着当前用户无权访问他们请求的特定资源。

否则，如果用户确实有一个有效的令牌，我们用我们的 Prisma 客户端获取它们，并将其传递给我们的`getLikedVideos`控制器。我们可以通过向请求或`req`对象添加一个属性，然后调用`next`函数(也是一个中间件函数)来实现。

中间件在我们的应用程序中是必不可少的，主要用于授权当前经过身份验证的用户，以及保护包含安全信息的端点。

中间件还有助于处理我们后端的错误，这样我们就可以成功地从错误中恢复，并确保我们的应用程序在出现错误时不会中断。

## 步骤 4:创建 React 客户端页面和样式

转到 React 前端，在 Create React app 的帮助下，我们可以轻松地创建 React App 来使用我们的节点 API。

[要开始使用 Create React App](https://reedbarger.com/create-react-app-10-steps) ，只需在项目文件夹的根目录下运行命令:

```
npx create-react-app client
```

安装完成后，我们将有一个 React 应用程序放在文件夹`client`中，就在`server`文件夹中我们的服务器代码旁边。

React 应用程序的第一步是为我们的应用程序设置所有单独的路线。这些将被放置在 App.js 组件中，并与 YouTube 为其应用程序提供的路线相对应:

```
// client/src/App.js

import React from "react";
import { Route, Switch } from "react-router-dom";
import MobileNavbar from "./components/MobileNavbar";
import Navbar from "./components/Navbar";
import Sidebar from "./components/Sidebar";
import { useLocationChange } from "./hooks/use-location-change";
import Channel from "./pages/Channel";
import History from "./pages/History";
import Home from "./pages/Home";
import Library from "./pages/Library";
import LikedVideos from "./pages/LikedVideos";
import NotFound from "./pages/NotFound";
import SearchResults from "./pages/SearchResults";
import Subscriptions from "./pages/Subscriptions";
import Trending from "./pages/Trending";
import WatchVideo from "./pages/WatchVideo";
import YourVideos from "./pages/YourVideos";
import Container from "./styles/Container";

function App() {
  const [isSidebarOpen, setSidebarOpen] = React.useState(false);
  const handleCloseSidebar = () => setSidebarOpen(false);
  const toggleSidebarOpen = () => setSidebarOpen(!isSidebarOpen);
  useLocationChange(handleCloseSidebar);

  return (
    <>
      <Navbar toggleSidebarOpen={toggleSidebarOpen} />
      <Sidebar isSidebarOpen={isSidebarOpen} />
      <MobileNavbar />
      <Container>
        <Switch>
          <Route exact path="/" component={Home} />
          <Route path="/watch/:videoId" component={WatchVideo} />
          <Route path="/channel/:channelId" component={Channel} />
          <Route path="/results/:searchQuery" component={SearchResults} />
          <Route path="/feed/trending" component={Trending} />
          <Route path="/feed/subscriptions" component={Subscriptions} />
          <Route path="/feed/library" component={Library} />
          <Route path="/feed/history" component={History} />
          <Route path="/feed/my_videos" component={YourVideos} />
          <Route path="/feed/liked_videos" component={LikedVideos} />
          <Route path="*" component={NotFound} />
        </Switch>
      </Container>
    </>
  );
}
```

对于我们的路由器和所有路由，我们使用库`react-router-dom`，它也将为我们提供一些有用的 React 钩子来访问路由参数(`useParams`)之类的值，并以编程方式在应用程序中导航我们的用户(`useHistory`)。

在构建应用程序的外观时，我们将使用一个名为`styled-components`的库。样式化组件非常有用的一点是，它是一个 **CSS-in-JS** 库。

> CSS-in-JS 库的好处是我们可以在我们的。js 文件。它允许我们使用 React 和 JavaScript 特性，这是我们在普通 CSS 样式表中无法使用的。

我们可以像对待普通的 react 组件一样，将某些值作为道具传递给我们的样式化组件。

这里看一下我们的一个样式组件，我们根据属性`red`的值有条件地设置了几个样式规则。

正如您可能已经猜到的那样，通过将值为 true 的 prop blue 传递给我们的样式化按钮组件，它使我们的按钮变成了 YouTube 的红色。

```
// client/src/styles/Button.js

import styled, { css } from "styled-components";

const Button = styled.button`
  padding: 10px 16px;
  border-radius: 1px;
  font-weight: 400;
  font-size: 14px;
  font-size: 0.875rem;
  font-weight: 500;
  line-height: 1.75;
  text-transform: uppercase;
  letter-spacing: 0.02857em;

  ${(props) =>
    props.red &&
    css`
      background: ${(props) => props.theme.darkRed};
      border: 1px solid ${(props) => props.theme.darkRed};
      color: white;
    `}
`;

export default Button;
```

下面是我们如何使用上面创建的带有`red`属性的`Button`样式的组件:

```
// example usage:
import React from "react";
import Button from "../styles/Button";
import Wrapper from "../styles/EditProfile";

function EditProfile() {
  return (
    <Wrapper>
      <div>
        <Button red onClick={() => setShowModal(true)}>
          Edit Profile
        </Button>
      </div>
    </Wrapper> 
  );
```

使用样式化组件的另一个好处是它给了我们**范围样式**。

换句话说，在样式化组件中编写的样式将只应用于使用它们的组件，而不会应用于应用程序中的其他地方。

这与普通的 CSS 样式表非常不同，在普通的 CSS 样式表中，如果你将它们包含在应用程序中，它们是全局的，它们会应用于整个应用程序。

## 步骤 5:使用 Google OAuth 添加客户端身份验证

下一步是在 Google OAuth 的帮助下添加身份验证。

在一个名为`react-google-login`的库的帮助下，这是一个非常容易设置的东西。它为我们提供了一个定制的钩子和一个特殊的 React 组件，如果用户有 Google 帐户，我们可以用它来登录用户。

下面是用于`GoogleAuth`组件的代码，用户可以使用谷歌的弹出模式按下该组件立即登录:

```
// client/src/components/GoogleAuth.js

import React from "react";
import Button from "../styles/Auth";
import { SignInIcon } from "./Icons";
import { GoogleLogin } from "react-google-login";
import { authenticate } from "../utils/api-client";

function GoogleAuth() {
  return (
    <GoogleLogin
      clientId="your-client-id-from-google-oauth"
      cookiePolicy="single_host_origin"
      onSuccess={authenticate}
      onFailure={authenticate}
      render={(renderProps) => (
        <Button
          tabIndex={0}
          type="button"
          onClick={renderProps.onClick}
          disabled={renderProps.disabled}
        >
          <span className="outer">
            <span className="inner">
              <SignInIcon />
            </span>
            sign in
          </span>
        </Button>
      )}
    />
  );
}

export default GoogleAuth;
```

## 步骤 6:使用 React Query 轻松获取数据

一旦我们能够认证我们的用户，我们就可以继续创建我们的页面或页面内容，并开始向我们的 API 端点发出请求。

用于发出 HTTP 请求的功能最全、最简单的库之一叫做`axios`。此外，跨 React 组件最容易发出请求的方法是使用一个名为`react-query`的特殊库。

React Query 非常有用的一点是它的自定义 React 挂钩。它们不仅可以请求数据，还允许我们缓存(保存)每次查询的结果，因此如果数据已经在本地缓存中，我们就不必重新提取数据。

换句话说，React Query 是一个强大的数据获取和状态管理库。

下面是我如何使用 React query 在主页上为用户请求所有推荐视频的快速示例。

```
// client/src/pages/Home.js

import axios from "axios";
import React from "react";
import { useQuery } from "react-query";
import ErrorMessage from "../components/ErrorMessage";
import VideoCard from "../components/VideoCard";
import HomeSkeleton from "../skeletons/HomeSkeleton";
import Wrapper from "../styles/Home";
import VideoGrid from "../styles/VideoGrid";

function Home() {
  const {
    data: videos,
    isSuccess,
    isLoading,
    isError,
    error,
  } = useQuery("Home", () =>
    axios.get("/videos").then((res) => res.data.videos)
  );

  if (isLoading) return <HomeSkeleton />;
  if (isError) return <ErrorMessage error={error} />;

  return (
    <Wrapper>
      <VideoGrid>
        {isSuccess
          ? videos.map((video) => <VideoCard key={video.id} video={video} />)
          : null}
      </VideoGrid>
    </Wrapper>
  );
}

export default Home;
```

如果我们处于加载状态，我们会像 YouTube 应用程序一样显示一个加载框架。如果有错误，我们会在页面中显示一条错误消息。

否则，如果请求成功，我们显示我们的后端推荐给我们的用户的视频。

## 第七步:上传和播放用户视频

为了上传我们的视频，我们将使用图书馆云。

我们可以通过使用文件输入将视频从 React 上传到 Cloudinary，我们将使用文件输入从计算机中选择视频文件，然后向 Cloudinary API 发出请求。一旦视频上传到它的服务器上，它就会给我们一个 URL。

从那里，用户将能够提供他们的视频信息。一旦他们点击发布，我们可以将他们的视频信息保存在我们的数据库中。

当显示用户创建的视频时，我们将使用一个名为`video.js`的开源库。

要观看单个视频，我们需要根据视频的 id 获取视频。之后，我们将把 URL 传递给 video.js 播放器，这将使用户能够滚动视频，全屏显示，并改变音量。

```
// client/src/components/VideoPlayer.js

import React from "react";
import videojs from "video.js";
import "video.js/dist/video-js.css";
import { addVideoView } from "../utils/api-client";

function VideoPlayer({ video }) {
  const videoRef = React.useRef();

  const { id, url, thumbnail } = video;

  React.useEffect(() => {
    const vjsPlayer = videojs(videoRef.current);

    vjsPlayer.poster(thumbnail);
    vjsPlayer.src(url);

    vjsPlayer.on("ended", () => {
      addVideoView(id);
    });
  }, [id, thumbnail, url]);

  return (
    <div data-vjs-player>
      <video
        controls
        ref={videoRef}
        className="video-js vjs-fluid vjs-big-play-centered"
      ></video>
    </div>
  );
}

export default VideoPlayer;
```

在视频下面，用户可以添加评论，喜欢和不喜欢视频，以及订阅视频作者的频道。

通过向适当的 API 端点发出网络请求，所有这些不同的特性都将成为可能(再次使用`axios`)。

## 步骤 8:用自定义钩子保护授权动作

一旦我们创建了许多这样的功能，我们需要锁定一些未经验证的用户的操作。

我们不希望未经授权的用户能够尝试登录，创建评论或喜欢视频，等等。这些都是只有某些经过身份验证的用户才能执行的操作。

因此，我们可以创建一个自定义钩子来保护一个经过验证的动作。创建这个钩子的原因是为了在我们的许多组件之间方便地重用，这些组件在内部使用经过验证的动作。

这个定制钩子将被称为`useAuthAction`。

```
// client/src/hooks/use-auth-action.js

import { useGoogleLogin } from "react-google-login";
import { useAuth } from "../context/auth-context";
import { authenticate } from "../utils/api-client";

export default function useAuthAction() {
  const user = useAuth();
  const { signIn } = useGoogleLogin({
    onSuccess: authenticate,
    clientId: "your-client-id",
  });

  function handleAuthAction(authAction, data) {
    if (user) {
      authAction(data);
    } else {
      signIn();
    }
  }

  return handleAuthAction;
}
```

`handleAuthAction`函数将从我们的钩子返回，并将接受我们想要执行的函数作为参数，例如喜欢或不喜欢某个视频的函数。

`handleAuthAction`将接受该函数的参数作为其第二个参数:

```
// client/src/pages/WatchVideo.js

function WatchVideo() {
  const handleAuthAction = useAuthAction();

  function handleLikeVideo() {
    handleAuthAction(likeVideo, video.id);
  }

  function handleDislikeVideo() {
    handleAuthAction(dislikeVideo, video.id);
  }

  function handleToggleSubscribe() {
    handleAuthAction(toggleSubscribeUser, video.user.id);
  }

// rest of component
}
```

如果一个未经验证的用户试图登录或创建一个评论，而不是向我们的 API 发出创建评论的请求，他们将通过来自`react-google-login`库的`useGoogleLogin`钩子自动登录。

## 步骤 9:更改用户频道数据

此时，我们已经显示了用户喜欢的所有视频、他们的观看历史、他们关注的频道、热门视频等等。

最后，我们还将显示每个用户的渠道，使他们能够改变他们的用户信息，如他们的用户名，简历，头像和封面图片。

这些图像上传将再次使用 Cloudinary 执行。用户将能够选择他们想要的图像作为他们的封面头像图像。我们将请求 Cloudinary API 给我们一个 URL，然后我们将使用它来更新我们的用户信息。

所有这些变化都将通过我们将要创建的模型来实现。我们将使用包`@reach/dialog`来创建它，该包将为我们提供一个考虑到可访问性的模型，并且我们可以根据自己的喜好来设计样式。

下面是我们将在模型中使用的代码，用于上传用户的图像并更新他们的频道。

```
// client/src/components/EditChannelModal.js

import React from "react";
import { useSnackbar } from "react-simple-snackbar";
import Button from "../styles/Button";
import Wrapper from "../styles/EditChannelModal";
import { updateUser } from "../utils/api-client";
import { uploadMedia } from "../utils/upload-media";
import { CloseIcon } from "./Icons";

function EditChannelModal({ channel, closeModal }) {
  const [openSnackbar] = useSnackbar();
  const [cover, setCover] = React.useState(channel.cover);
  const [avatar, setAvatar] = React.useState(channel.avatar);

  async function handleCoverUpload(event) {
    const file = event.target.files[0];

    if (file) {
      const cover = await uploadMedia({
        type: "image",
        file,
        preset: "your-cover-preset",
      });
      setCover(cover);
    }
  }

  async function handleAvatarUpload(event) {
    const file = event.target.files[0];

    if (file) {
      const avatar = await uploadMedia({
        type: "image",
        file,
        preset: "your-avatar-preset",
      });
      setAvatar(avatar);
    }
  }

  async function handleEditChannel(event) {
    event.preventDefault();
    const username = event.target.elements.username.value;
    const about = event.target.elements.about.value;

    if (!username.trim()) {
      return openSnackbar("Username cannot be empty");
    }

    const user = {
      username,
      about,
      avatar,
      cover,
    };

    await updateUser(user);
    openSnackbar("Channel updated");
    closeModal();
  }

  return (
    <Wrapper>
      <div className="edit-channel">
        <form onSubmit={handleEditChannel}>
          <div className="modal-header">
            <h3>
              <CloseIcon onClick={closeModal} />
              <span>Edit Channel</span>
            </h3>
            <Button type="submit">Save</Button>
          </div>

          <div className="cover-upload-container">
            <label htmlFor="cover-upload">
              <img
                className="pointer"
                width="100%"
                height="200px"
                src={cover}
                alt="cover"
              />
            </label>
            <input
              id="cover-upload"
              type="file"
              accept="image/*"
              style={{ display: "none" }}
              onChange={handleCoverUpload}
            />
          </div>

          <div className="avatar-upload-icon">
            <label htmlFor="avatar-upload">
              <img src={avatar} className="pointer avatar lg" alt="avatar" />
            </label>
            <input
              id="avatar-upload"
              type="file"
              accept="image/*"
              style={{ display: "none" }}
              onChange={handleAvatarUpload}
            />
          </div>
          <input
            type="text"
            placeholder="Insert username"
            id="username"
            defaultValue={channel.username}
            required
          />
          <textarea
            id="about"
            placeholder="Tell viewers about your channel"
            defaultValue={channel.about}
          />
        </form>
      </div>
    </Wrapper>
  );
}

export default EditChannelModal;
```

## 步骤 10:将我们的应用程序发布到网络上

一旦我们添加了我们想要的所有功能，我们将使用 Heroku 将我们的 React 和 Node 应用程序部署到 web 上。

首先，我们需要在 Node package.json 文件中添加一个安装后脚本，告诉 Heroku 在部署时自动构建 React 应用程序:

```
{
  "name": "server",
  "version": "0.1.0",
  "scripts": {
    "start": "node server",
    ...
    "postinstall": "cd client && npm install && npm run build"
  }
} 
```

为了能够告诉我们的节点后端，我们希望将它与 React 前端一起部署在同一个域上，我们需要将以下代码添加到创建 Express 应用程序的位置，在所有中间件之后:

```
// server/src/start.js

if (process.env.NODE_ENV === "production") {
    app.use(express.static(path.resolve(__dirname, "../client/build")));

    app.get("*", function (req, res) {
      res.sendFile(path.resolve(__dirname, "../client/build", "index.html"));
    });
}
```

上面的代码说:如果一个 GET 请求被发送到我们的应用程序，但是没有被我们的 API 处理，那么用我们的 React 客户端的构建版本来响应。

换句话说，如果我们不从后端请求数据，就将构建的 React 客户端发送给我们的用户。

## 结论

希望这篇教程能给你一些关于如何构建下一个 React 项目的想法，尤其是如果你想构建像 YouTube 这样令人印象深刻的应用。

## 想要构建像这样令人惊叹的 React 应用程序吗？

每个月底，我都会发布一个特别的课程，一步一步地向你展示如何像这个 YouTube 克隆版一样构建令人惊叹的 React 项目。

[**如果你想用 React 构建外观和工作方式都和你日常使用的一样的真实世界的应用程序，点击这里注册等候名单**](https://bit.ly/12-react-projects) 。