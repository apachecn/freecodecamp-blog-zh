# React 中关注点的分离——如何使用容器和表示组件

> 原文：<https://www.freecodecamp.org/news/separation-of-concerns-react-container-and-presentational-components/>

许多新的 React 开发人员在同一个 React 组件中结合了逻辑和表示。他们可能不知道为什么把这两者分开很重要——他们只是想让它发挥作用。

但是后来，他们会发现他们需要对文件进行修改，而这样做就变成了一项艰巨的任务。然后，他们将不得不重新工作，以分离这两个部分。

这是因为不了解关注点的分离以及表示和容器组件模式。这就是为什么我要告诉你这些，这样你就可以在项目开发周期的早期减轻这个问题。

在本文中，我们将深入到容器和表示组件中，并简要介绍关注点分离的概念。

事不宜迟，我们开始吧！

## 目录

*   [什么是关注点分离？](#whatistheseparationofconcerns)
*   什么是表示和容器组件？
*   [我们为什么需要这些组件？](#whydoweneedthesecomponents)
*   [演示和容器组件示例](#presentationandcontainercomponentexample)
*   [如何用 React 挂钩更换容器组件](#howtoreplacecontainercomponentswithreacthooks)
*   [总结](#summary)

## 什么是关注点分离？

关注点分离是一个在编程中广泛使用的概念。它指出执行不同动作的逻辑不应该被分组或组合在一起。

例如，我们在引言部分讨论的内容违反了关注点分离，因为我们将获取数据和呈现数据的逻辑放在了同一个组件中。

为了解决这个问题并坚持关注点的分离，我们应该将这两个逻辑部分——也就是获取数据并将其呈现在 UI 上——分成两个不同的组件。

这就是容器和表示组件模式将帮助我们解决这个问题的地方。在接下来的几节中，我们将深入探讨这种模式。

## 什么是容器和表示组件？

为了实现关注点的分离，我们有两种类型的组件:

*   容器组件
*   表象成分

### 容器组件

这些组件为子组件提供、创建或保存数据。

容器组件唯一的工作就是处理数据。它不包含任何自己的用户界面。相反，它由表示组件组成，作为使用该数据的子组件。

一个简单的例子是一个名为`FetchUserContainer`的组件，它包含一些获取所有用户的逻辑。

### 表象成分

这些组件的主要职责是在 UI 上显示数据。它们从容器组件中获取数据。

这些组件是无状态的，除非它们需要自己的状态来呈现 UI。它们不会改变接收到的数据。

一个例子是显示所有用户的`UserList`组件。

## 为什么我们需要这些组件？

为了理解这一点，我们举一个简单的例子。我们希望显示从 [JSON 占位符 API](https://jsonplaceholder.typicode.com/) 获取的帖子列表。下面是相同的代码:

```
import { useEffect, useState } from "react";

interface Post {
  userId: number;
  id: number;
  title: string;
  body: string;
}

/**
 * An example of how we shouldn't combine the logic and presentation of data.
 */
export default function DisplayPosts() {
  const [posts, setPosts] = useState<Post[] | null>(null);
  const [isLoading, setIsLoading] = useState<Boolean>(false);
  const [error, setError] = useState<unknown>();

// Logic
  useEffect(() => {
    (async () => {
      try {
        setIsLoading(true);
        const resp = await fetch("https://jsonplaceholder.typicode.com/posts");
        const data = await resp.json();
        setPosts(data);
        setIsLoading(false);
      } catch (err) {
        setError(err);
        setIsLoading(false);
      }
    })();
  }, []);

// Presentation
  return isLoading ? (
    <span>Loading... </span>
  ) : posts ? (
    <ul>
      {posts.map((post: Post) => (
        <li key={`item-${post.id}`}>
          <span>{post.title}</span>
        </li>
      ))}
    </ul>
  ) : (
    <span>{JSON.stringify(error)}</span>
  );
} 
```

该组件的作用如下:

*   它有三个状态变量:`posts`、`isLoading`和`error`。
*   我们有一个由业务逻辑组成的`useEffect`钩子。这里我们用[获取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) 从 API`[https://jsonplaceholder.typicode.com/posts](https://jsonplaceholder.typicode.com/posts)`获取数据。
*   我们确保当获取数据时，我们使用`setPosts`将它存储在`posts`状态变量中。
*   我们还确保在各自的场景中切换`isLoading`和`error`值。
*   我们把这整个逻辑放在一个异步生命中。
*   最后，我们以无序列表的形式返回帖子，并映射之前获取的所有帖子。

上面的问题是获取数据和显示数据的逻辑被编码到一个组件中。我们可以说，组件现在与逻辑紧密耦合。这正是我们不想要的东西。

以下是我们需要容器和表示组件的一些原因:

*   它们帮助我们创建松散耦合的组件
*   它们帮助我们保持关注点的分离
*   代码重构变得容易多了。
*   代码变得更加有组织和可维护
*   这使得测试更加容易。

## 表示和容器组件示例

好了，说够了——让我们从一个简单的例子开始。我们将使用与上面相同的例子——从 JSON 占位符 API 获取数据。

让我们理解这里的文件结构:

*   我们的容器组件将是`PostContainer`
*   我们将有两个演示组件:
    *   `Posts`:有一个无序列表的组件。
    *   `SinglePost`:呈现列表标签的组件。这将呈现列表中的每个元素。

注意:我们将上述所有组件存储在一个名为`components`的单独文件夹中。

现在我们知道了哪些东西去了哪里，让我们从容器组件开始:`PostContainer`。将以下代码复制粘贴到`components/PostContainer.tsx`文件中

```
import { useEffect, useState } from "react";
import { ISinglePost } from "../Definitions";
import Posts from "./Posts";

export default function PostContainer() {
  const [posts, setPosts] = useState<ISinglePost[] | null>(null);
  const [isLoading, setIsLoading] = useState<Boolean>(false);
  const [error, setError] = useState<unknown>();

  useEffect(() => {
    (async () => {
      try {
        setIsLoading(true);
        const resp = await fetch("https://jsonplaceholder.typicode.com/posts");
        const data = await resp.json();
        setPosts(data.filter((post: ISinglePost) => post.userId === 1));
        setIsLoading(false);
      } catch (err) {
        setError(err);
        setIsLoading(false);
      }
    })();
  }, []);

  return isLoading ? (
    <span>Loading... </span>
  ) : posts ? (
    <Posts posts={posts} />
  ) : (
    <span>{JSON.stringify(error)}</span>
  );
} 
```

从本文前一节看到的例子中，上面的代码只包含获取数据的逻辑。这个逻辑存在于`useEffect`钩子中。在这里，这个容器组件将这个数据传递给`Posts`表示组件。

让我们来看看`Posts`演示组件。将下面的代码复制粘贴到`components/Posts.tsx`文件中:

```
/**
 * A presentational component
 */

import { ISinglePost } from "../Definitions";
import SinglePost from "./SinglePost";

export default function Posts(props: { posts: ISinglePost[] }) {
  return (
    <ul
      style={{
        display: "flex",
        flexDirection: "column",
        alignItems: "center"
      }}
    >
      {props.posts.map((post: ISinglePost) => (
        <SinglePost {...post} />
      ))}
    </ul>
  );
} 
```

如您所见，这是一个简单的文件，由一个`ul`标签组成——一个无序列表。然后这个组件映射到作为道具传递的`posts`。我们将每个传递给`SinglePost`组件。

还有另一个呈现列表标签的表示组件，即`li`标签。它显示文章的标题和正文。将下面的代码复制粘贴到`components/SinglePost.tsx`文件中:

```
import { ISinglePost } from "../Definitions";

export default function SinglePost(props: ISinglePost) {
  const { userId, id, title, body } = props;
  return (
    <li key={`item-${userId}-${id}`} style={{ width: 400 }}>
      <h4>
        <strong>{title}</strong>
      </h4>
      <span>{body}</span>
    </li>
  );
} 
```

如您所见，这些表示组件只是在屏幕上显示数据。仅此而已。他们不做别的。因为它们只是在这里显示数据，所以它们也有自己的样式。

现在我们已经设置了组件，让我们回顾一下我们在这里取得的成绩:

*   在这个例子中没有违反关注点分离的概念。
*   为每个组件编写单元测试变得更加容易。
*   代码的可维护性和可读性要好得多。因此，我们的代码库变得更有组织性。

我们已经实现了我们想要的，但是我们可以在钩子的帮助下进一步增强这个模式。

## 如何用 React 钩子替换容器组件

自从 **React 16.8.0** 以来，在功能组件和钩子的帮助下构建和开发组件变得容易多了。

我们将在这里利用这些功能，并用一个钩子替换容器组件。

将以下代码复制粘贴到`hooks/usePosts.ts`文件中:

```
import { useEffect, useState } from "react";
import { ISinglePost } from "../Definitions";

export default function usePosts() {
  const [posts, setPosts] = useState<ISinglePost[] | null>(null);
  const [isLoading, setIsLoading] = useState<Boolean>(false);
  const [error, setError] = useState<unknown>();

  useEffect(() => {
    (async () => {
      try {
        setIsLoading(true);
        const resp = await fetch("https://jsonplaceholder.typicode.com/posts");
        const data = await resp.json();
        setPosts(data.filter((post: ISinglePost) => post.userId === 1));
        setIsLoading(false);
      } catch (err) {
        setError(err);
        setIsLoading(false);
      }
    })();
  }, []);

  return {
    isLoading,
    posts,
    error
  };
} 
```

这里我们有，

*   将出现在`PostContainer`组件中的逻辑提取到一个钩子中。
*   这个钩子将返回一个包含`isLoading`、`posts`和`error`值的对象。

现在我们可以简单地移除容器组件`PostContainer`。然后，我们可以在`Posts`表示组件中直接使用这个钩子，而不是将容器的数据作为道具传递给表示组件。

对`Posts`组件进行以下编辑:

```
/**
 * A presentational component
 */

import { ISinglePost } from "../Definitions";
import usePosts from "../hooks/usePosts";
import SinglePost from "./SinglePost";

export default function Posts(props: { posts: ISinglePost[] }) {
  const { isLoading, posts, error } = usePosts();

  return (
    <ul
      style={{
        display: "flex",
        flexDirection: "column",
        alignItems: "center"
      }}
    >
      {isLoading ? (
        <span>Loading...</span>
      ) : posts ? (
        posts.map((post: ISinglePost) => <SinglePost {...post} />)
      ) : (
        <span>{JSON.stringify(error)}</span>
      )}
    </ul>
  );
} 
```

通过使用钩子，我们消除了存在于这些表示组件之上的额外组件层。

使用钩子，我们获得了与容器/表示组件模式相同的结果。

## 摘要

因此，在本文中，我们了解了:

*   关注点分离
*   容器和表示组件
*   为什么我们需要这些组件
*   钩子如何取代容器组件

对于进一步的阅读，我强烈推荐浏览一下[反应表:](https://tanstack.com/table/v8/)。这个库广泛使用了钩子，并且有很好的例子。

您可以在这个 [codesandbox](https://codesandbox.io/s/container-presentation-pattern-lm1osl?file=/src/components/PostContainer.tsx) 中找到本文的完整代码。

感谢阅读！

在 [Twitter](https://twitter.com/keurplkar) 、 [GitHub](https://github.com/keyurparalkar) 和 [LinkedIn](https://www.linkedin.com/in/keyur-paralkar-494415107/) 上关注我。