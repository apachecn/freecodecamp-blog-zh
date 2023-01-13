# 关于承诺，你需要知道的一切

> 原文：<https://www.freecodecamp.org/news/promise-all-in-javascript-with-example-6c8c5aea3e32/>

JavaScript 中的承诺是帮助我们进行异步操作的强大 API 之一。

Promise.all 将异步操作提升到了一个新的水平，因为它可以帮助您聚合一组承诺。

换句话说，我可以说它帮助你做并发操作(有时是免费的)。

#### 先决条件:

你必须知道 JavaScript 中的**承诺**是什么。

#### 什么是承诺？

Promise.all 实际上是一个接受一组承诺作为输入的承诺(一个 iterable)。然后，当所有的承诺都得到解决，或者其中任何一个被拒绝时，问题就解决了。

例如，假设您有十个承诺(执行网络调用或数据库连接的异步操作)。你必须知道什么时候所有的承诺都解决了，或者你必须等到所有的承诺都解决了。所以你把所有的十个承诺传递给了 Promise.all，那么，Promise.all 本身作为一个承诺将会得到解决，一旦所有的十个承诺都得到解决，或者十个承诺中的任何一个被错误地拒绝。

**让我们用代码来看看:**

```
Promise.all([Promise1, Promise2, Promise3])
 .then(result) => {
   console.log(result)
 })
 .catch(error => console.log(`Error in promises ${error}`))
```

如您所见，我们将一个数组传递给 Promise.all。当三个承诺都被解析时，Promise.all 被解析，输出得到安慰。

**我们来看一个例子:**

```
// A simple promise that resolves after a given time
const timeOut = (t) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(`Completed in ${t}`)
    }, t)
  })
}

// Resolving a normal promise.
timeOut(1000)
 .then(result => console.log(result)) // Completed in 1000

// Promise.all
Promise.all([timeOut(1000), timeOut(2000)])
 .then(result => console.log(result)) // ["Completed in 1000", "Completed in 2000"]
```

在上面的示例中，Promise.all 在 2000 ms 后解析，输出以数组的形式进行控制。

关于承诺，有趣的一点是承诺的顺序是保持不变的。数组中的第一个承诺将被解析为输出数组的第一个元素，第二个承诺将是输出数组中的第二个元素，依此类推。

让我们看另一个例子:

```
// A simple promise that resolves after a given time
const timeOut = (t) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(`Completed in ${t}`)
    }, t)
  })
}

const durations = [1000, 2000, 3000]

const promises = []

durations.map((duration) => {
  // In the below line, two things happen.
  // 1\. We are calling the async function (timeout()). So at this point the async function has started and enters the 'pending' state.
  // 2\. We are pushing the pending promise to an array.
  promises.push(timeOut(duration)) 
})

console.log(promises) // [ Promise { "pending" }, Promise { "pending" }, Promise { "pending" } ]

// We are passing an array of pending promises to Promise.all
// Promise.all will wait till all the promises get resolves and then the same gets resolved.
Promise.all(promises)
.then(response => console.log(response)) // ["Completed in 1000", "Completed in 2000", "Completed in 3000"] 
```

从上面的例子可以清楚地看出，Promise.all 一直等到所有的承诺都解决了。

让我们看看如果任何一个承诺被拒绝会发生什么。

```
// A simple promise that resolves after a given time
const timeOut = (t) => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (t === 2000) {
        reject(`Rejected in ${t}`)
      } else {
        resolve(`Completed in ${t}`)
      }
    }, t)
  })
}

const durations = [1000, 2000, 3000]

const promises = []

durations.map((duration) => {
  promises.push(timeOut(duration)) 
})

// We are passing an array of pending promises to Promise.all
Promise.all(promises)
.then(response => console.log(response)) // Promise.all cannot be resolved, as one of the promises passed got rejected.
.catch(error => console.log(`Error in executing ${error}`)) // Promise.all throws an error. 
```

正如你所看到的，如果其中一个承诺失败了，那么其余的承诺都会失败。那就答应吧，所有的都被拒绝了。

对于一些用例，你不需要这样。你需要履行所有的承诺，即使有些已经失败，或者你可以以后再处理失败的承诺。

让我们看看如何处理。

```
const durations = [1000, 2000, 3000]

promises = durations.map((duration) => {
  return timeOut(duration).catch(e => e) // Handling the error for each promise.
})

Promise.all(promises)
  .then(response => console.log(response)) // ["Completed in 1000", "Rejected in 2000", "Completed in 3000"]
  .catch(error => console.log(`Error in executing ${error}`))
view raw
```

#### 承诺的用例. all

假设您必须执行大量的异步操作，比如向成千上万的用户发送批量营销电子邮件。

简单的伪代码是:

```
for (let i=0;i<50000; i += 1) {
 sendMailForUser(user[i]) // Async operation to send a email
}
```

上面的例子很简单。但它不是很有表现力。栈将变得太重，在某个时间点，JavaScript 将有大量打开的 HTTP 连接，这可能会杀死服务器。

一种简单的性能方法是分批完成。取前 500 个用户，触发邮件，等待所有 HTTP 连接关闭。然后拿下一批去加工等等。

让我们看一个例子:

```
// Async function to send mail to a list of users.
const sendMailForUsers = async (users) => {
  const usersLength = users.length

  for (let i = 0; i < usersLength; i += 100) { 
    const requests = users.slice(i, i + 100).map((user) => { // The batch size is 100\. We are processing in a set of 100 users.
      return triggerMailForUser(user) // Async function to send the mail.
       .catch(e => console.log(`Error in sending email for ${user} - ${e}`)) // Catch the error if something goes wrong. So that it won't block the loop.
    })

    // requests will have 100 or less pending promises. 
    // Promise.all will wait till all the promises got resolves and then take the next 100.
    await Promise.all(requests)
     .catch(e => console.log(`Error in sending email for the batch ${i} - ${e}`)) // Catch the error.
  }
}

sendMailForUsers(userLists)
```

让我们考虑另一个场景:您必须构建一个 API，它从多个第三方 API 获取信息，并聚合来自这些 API 的所有响应。

承诺。一切都是做到这一点的最佳方式。让我们看看怎么做。

```
// Function to fetch Github info of a user.
const fetchGithubInfo = async (url) => {
  console.log(`Fetching ${url}`)
  const githubInfo = await axios(url) // API call to get user info from Github.
  return {
    name: githubInfo.data.name,
    bio: githubInfo.data.bio,
    repos: githubInfo.data.public_repos
  }
}

// Iterates all users and returns their Github info.
const fetchUserInfo = async (names) => {
  const requests = names.map((name) => {
    const url = `https://api.github.com/users/${name}`
    return fetchGithubInfo(url) // Async function that fetches the user info.
     .then((a) => {
      return a // Returns the user info.
      })
  })
  return Promise.all(requests) // Waiting for all the requests to get resolved.
}

fetchUserInfo(['sindresorhus', 'yyx990803', 'gaearon'])
 .then(a => console.log(JSON.stringify(a)))

/*
Output:
[{
  "name": "Sindre Sorhus",
  "bio": "Full-Time Open-Sourcerer ·· Maker ·· Into Swift and Node.js ",
  "repos": 996
}, {
  "name": "Evan You",
  "bio": "Creator of @vuejs, previously @meteor & @google",
  "repos": 151
}, {
  "name": "Dan Abramov",
  "bio": "Working on @reactjs. Co-author of Redux and Create React App. Building tools for humans.",
  "repos": 232
}]
*/ 
```

总之，Promise.all 是将一组承诺聚合成一个承诺的最佳方式。这是 JavaScript 中实现并发性的方法之一。

希望你喜欢这篇文章。如果有，请鼓掌分享。

即使你没有，也没关系，你还是可以做的:P