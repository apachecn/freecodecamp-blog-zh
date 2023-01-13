# JSON 字符串示例——如何用 JS 解析 JSON 对象

> 原文：<https://www.freecodecamp.org/news/json-stringify-example-how-to-parse-a-json-object-with-javascript/>

JSON，或者说 JavaScript Object Notation，就在我们身边。如果您曾经使用过 web 应用程序，那么它很有可能使用 JSON 在其服务器和您的设备之间构建、存储和传输数据。

在本文中，我们将简要介绍 JSON 和 JavaScript 之间的区别，然后跳到在浏览器和 Node.js 项目中用 JavaScript 解析 JSON 的不同方法。

## JSON 和 JavaScript 的区别

虽然 JSON 看起来像普通的 JavaScript，但最好将 JSON 视为一种数据格式，类似于文本文件。恰好 JSON 受到 JavaScript 语法的启发，这就是它们看起来如此相似的原因。

让我们看一下 JSON 对象和 JSON 数组，并把它们与 JavaScript 对象进行比较。

### JSON 对象与 JavaScript 对象文字

首先，这里有一个 JSON 对象:

```
{
  "name": "Jane Doe",
  "favorite-game": "Stardew Valley",
  "subscriber": false
} 
```

jane-profile.json

JSON 对象和普通 JavaScript 对象(也称为对象文字)的主要区别在于引号。JSON 对象中的所有键和字符串类型值都必须用双引号(`"`)括起来。

JavaScript 对象文字更加灵活。使用对象文字，您不需要用双引号将键和字符串括起来。相反，您可以使用单引号(`'`)，或者不为键使用任何类型的引号。

下面是上面的代码作为 JavaScript 对象文字的样子:

```
const profile = {
  name: 'Jane Doe',
  'favorite-game': 'Stardew Valley',
  subscriber: false
} 
```

注意，键`'favorite-game'`用单引号括起来。对于对象文字，您需要将单词由引号中的破折号(`-`)分隔的键包装起来。

如果您想避免引号，您可以重写密钥以使用 camel case ( `favoriteGame`)或者用下划线(`favorite_game`)来分隔单词。

### JSON 数组与 JavaScript 数组

JSON 数组的工作方式与 JavaScript 中的数组非常相似，可以包含字符串、布尔值、数字和其他 JSON 对象。例如:

```
[
  {
    "name": "Jane Doe",
    "favorite-game": "Stardew Valley",
    "subscriber": false
  },
  {
    "name": "John Doe",
    "favorite-game": "Dragon Quest XI",
    "subscriber": true
  }
] 
```

profiles.json

这在普通的 JavaScript 中可能是这样的:

```
const profiles = [
  {
    name: 'Jane Doe',
    'favorite-game': 'Stardew Valley',
    subscriber: false
  },
  {
    name: 'John Doe',
    'favorite-game': 'Dragon Quest XI',
    subscriber: true
  }
]; 
```

## 字符串形式的 JSON

您可能想知道，如果有 JSON 对象和数组，难道不能像普通的 JavaScript 对象文字或数组一样在程序中使用它吗？

之所以不能这样做，是因为 JSON 真的只是一个字符串。

例如，当您在一个单独的文件中编写 JSON 时，比如上面的`jane-profile.json`或`profiles.json`，该文件实际上包含 JSON 对象或数组形式的文本，看起来就像 JavaScript。

如果你向一个 API 发出请求，它会像这样返回:

```
{"name":"Jane Doe","favorite-game":"Stardew Valley","subscriber":false}
```

就像文本文件一样，如果您想在项目中使用 JSON，您需要解析它或者将其转换成您的编程语言能够理解的内容。例如，用 Python 解析 JSON 对象将创建一个字典。

有了这样的理解，让我们看看用 JavaScript 解析 JSON 的不同方法。

## 如何在浏览器中解析 JSON

如果您在浏览器中使用 JSON，您可能会通过 API 接收或发送数据。

让我们来看几个例子。

### 如何用`fetch`解析 JSON

从 API 获取数据最简单的方法是使用`fetch`，它包含了将 JSON 响应自动解析成可用的 JavaScript 对象文字或数组的`.json()`方法。

下面是一些使用`fetch`向免费的 [Chuck Norris 笑话 API](https://api.chucknorris.io/) 请求一个开发者主题笑话的代码:

```
fetch('https://api.chucknorris.io/jokes/random?category=dev')
  .then(res => res.json()) // the .json() method parses the JSON response into a JS object literal
  .then(data => console.log(data)); 
```

如果您在浏览器中运行该代码，您会看到控制台中记录了如下内容:

```
{
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "elgv2wkvt8ioag6xywykbq",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/elgv2wkvt8ioag6xywykbq",
    "value": "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
} 
```

虽然这看起来像一个 JSON 对象，但它实际上是一个 JavaScript 对象文字，您可以在程序中自由使用它。

### 如何用`JSON.stringify()`字符串化 JSON

但是如果你想把数据发送给一个 API 呢？

例如，假设您想将一个 Chuck Norris 笑话发送到 Chuck Norris 笑话 API，以便其他人稍后可以阅读它。

首先，将笑话写成 JS 对象文字:

```
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
}; 
```

然后，由于您正在向 API 发送数据，您需要将您的`newJoke`对象文字转换成 JSON 字符串。

幸运的是，JavaScript 包含了一个非常有用的方法来做到这一点—`JSON.stringify()`:

```
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
};

console.log(JSON.stringify(newJoke)); // {"categories":["dev"],"value":"Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."}

console.log(typeof JSON.stringify(newJoke)); // string 
```

在这个例子中，当我们将一个对象文字转换成一个 JSON 字符串时，`JSON.stringify()`也可以处理数组。

最后，您只需要用一个`POST`请求将 JSON 字符串化的笑话发送回 API。

注意 Chuck Norris 笑话 API 实际上没有这个特性。但是如果是这样的话，代码可能是这样的:

```
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
};

fetch('https://api.chucknorris.io/jokes/submit', { // fake API endpoint
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(newJoke), // turn the JS object literal into a JSON string
})
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => {
    console.error(err);
  });
```

就像这样，用`fetch`解析传入的 JSON，并用`JSON.stringify()`将 JS 对象文字转换成 JSON 字符串。

### 如何在浏览器中使用本地 JSON 文件

不幸的是，在浏览器中加载本地 JSON 文件是不可能的(也是不可取的)。

如果您尝试加载本地文件，将会抛出一个错误。例如，假设您有一个包含一些笑话的 JSON 文件:

```
[
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "elgv2wkvt8ioag6xywykbq",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/elgv2wkvt8ioag6xywykbq",
    "value": "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
  },
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "ae-78cogr-cb6x9hluwqtw",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/ae-78cogr-cb6x9hluwqtw",
    "value": "There is no Esc key on Chuck Norris' keyboard, because no one escapes Chuck Norris."
  }
] 
```

jokes.json

您希望解析它并在一个简单的 HTML 页面上创建一个笑话列表。

如果您创建了一个包含以下内容的页面，并在浏览器中打开它:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="viewport" content="width=device-width" />
    <title>Fetch Local JSON</title>
  </head>
  <script>
    fetch("./jokes.json", { mode: "no-cors" }) // disable CORS because path does not contain http(s)
      .then((res) => res.json())
      .then((data) => console.log(data));
  </script>
</html> 
```

index.html

您将在控制台中看到:

```
Fetch API cannot load file://<path>/jokes.json. URL scheme "file" is not supported 
```

默认情况下，出于安全原因，浏览器不允许访问本地文件。这是一件好事，你不应该试图绕过这种行为。

相反，最好的办法是将本地 JSON 文件转换成 JavaScript。幸运的是，这非常容易，因为 JSON 语法与 JavaScript 非常相似。

您需要做的就是创建一个新文件，并将您的 JSON 声明为一个变量:

```
const jokes = [
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "elgv2wkvt8ioag6xywykbq",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/elgv2wkvt8ioag6xywykbq",
    "value": "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
  },
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "ae-78cogr-cb6x9hluwqtw",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/ae-78cogr-cb6x9hluwqtw",
    "value": "There is no Esc key on Chuck Norris' keyboard, because no one escapes Chuck Norris."
  }
] 
```

jokes.js

并将其作为单独的脚本添加到您的页面中:

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
    <meta name="viewport" content="width=device-width" />
    <title>Fetch Local JSON</title>
  </head>
  <script src="jokes.js"></script>
  <script>
    console.log(jokes);
  </script>
</html> 
```

您将能够在代码中自由使用`jokes`数组。

您也可以使用 JavaScript `[modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)`来做同样的事情，但是这超出了本文的范围。

但是，如果您想使用本地 JSON 文件并安装了 Node.js，该怎么办呢？现在让我们来看看如何做到这一点。

## 如何解析 Node.js 中的 JSON

Node.js 是一个 JavaScript 运行时，允许您在浏览器之外运行 JavaScript。你可以在这里阅读所有关于 [Node.js 的内容。](https://www.freecodecamp.org/news/the-definitive-node-js-handbook-6912378afc6e/)

无论是使用 Node.js 在本地计算机上运行代码，还是在服务器上运行整个 web 应用程序，了解如何使用 JSON 都是有益的。

对于下面的例子，我们将使用相同的`jokes.json`文件:

```
[
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "elgv2wkvt8ioag6xywykbq",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/elgv2wkvt8ioag6xywykbq",
    "value": "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."
  },
  {
    "categories": ["dev"],
    "created_at": "2020-01-05 13:42:19.324003",
    "icon_url": "https://assets.chucknorris.host/img/avatar/chuck-norris.png",
    "id": "ae-78cogr-cb6x9hluwqtw",
    "updated_at": "2020-01-05 13:42:19.324003",
    "url": "https://api.chucknorris.io/jokes/ae-78cogr-cb6x9hluwqtw",
    "value": "There is no Esc key on Chuck Norris' keyboard, because no one escapes Chuck Norris."
  }
] 
```

jokes.json

### 如何用`require()`解析一个 JSON 文件

让我们从最简单的方法开始。

如果您有一个本地 JSON 文件，您需要做的就是使用`require()`像加载任何其他 Node.js 模块一样加载它:

```
const jokes = require('./jokes.json'); 
```

JSON 文件将被自动解析，您可以开始在您的项目中使用它:

```
const jokes = require('./jokes.json');

console.log(jokes[0].value); // "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris." 
```

请注意，这是同步的，这意味着您的程序将停止，直到它解析整个文件，然后再继续。非常大的 JSON 文件会导致你的程序变慢，所以要小心。

此外，因为以这种方式解析 JSON 会将整个内容加载到内存中，所以最好将这种方法用于静态 JSON 文件。如果在程序运行时 JSON 文件发生了变化，那么在重启程序并解析更新后的 JSON 文件之前，您将无法访问这些变化。

### 如何用`fs.readFileSync(`和`JSON.parse()`解析一个 JSON 文件

这是在 Node.js 项目中解析 JSON 文件的更传统的方式(因为缺少更好的术语)——用`fs`(文件系统)模块读取文件，然后用`JSON.parse()`解析。

让我们看看如何用`fs.readFileSync()`方法来实现这一点。首先，将`fs`模块添加到项目中:

```
const fs = require('fs'); 
```

然后，创建一个新变量来存储`jokes.json`文件的输出，并将其设置为等于`fs.readFileSync()`:

```
const fs = require('fs');
const jokesFile = fs.readFileSync(); 
```

需要几个论据。第一个是您要读取的文件的路径:

```
const fs = require('fs');
const jokesFile = fs.readFileSync('./jokes.json'); 
```

但是如果你现在登录`jokesFile`到控制台，你会看到这样的内容:

```
<Buffer 5b 0a 20 20 7b 0a 20 20 20 20 22 63 61 74 65 67 6f 72 69 65 73 22 3a 20 5b 22 64 65 76 22 5d 2c 0a 20 20 20 20 22 63 72 65 61 74 65 64 5f 61 74 22 3a ... 788 more bytes>
```

这仅仅意味着`fs`模块正在读取文件，但是它不知道文件的编码或格式。`fs`可以用来加载几乎任何文件，而不仅仅是像 JSON 这样基于文本的文件，所以我们需要告诉它文件是如何编码的。

对于基于文本的文件，编码通常是`utf8`:

```
const fs = require('fs');
const jokesFile = fs.readFileSync('./jokes.json', 'utf8'); 
```

现在，如果您将`jokesFile`登录到控制台，您将看到文件的内容。

但是目前为止我们只是在读取文件，它仍然是一个字符串。我们需要使用另一种方法将`jokesFile`解析成一个可用的 JavaScript 对象或数组。

为此，我们将使用`JSON.parse()`:

```
const fs = require('fs');
const jokesFile = fs.readFileSync('./jokes.json', 'utf8');
const jokes = JSON.parse(jokesFile);

console.log(jokes[0].value); // "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris." 
```

顾名思义，`JSON.parse()`接受一个 JSON 字符串，并将其解析成一个 JavaScript 对象文字或数组。

像上面的`require`方法一样，`fs.readFileSync()`是一个同步方法，这意味着如果它正在读取一个大文件，JSON 或其他，它可能会导致你的程序变慢。

此外，它只读取文件一次，并将其加载到内存中。如果文件发生变化，您需要在某个时候再次读取该文件。为了使事情变得简单，您可能想要创建一个简单的函数来读取文件。

这可能是这样的:

```
const fs = require('fs');
const readFile = path => fs.readFileSync(path, 'utf8');

const jokesFile1 = readFile('./jokes.json');
const jokes1 = JSON.parse(jokesFile1);

console.log(jokes1[0].value); // "Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris."

// the jokes.json file changes at some point

const jokesFile2 = readFile('./jokes.json');
const jokes2 = JSON.parse(jokesFile2);

console.log(jokes2[0].value); // "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
```

### 如何用`fs.readFile(`和`JSON.parse()`解析 JSON

除了异步工作之外，`fs.readFile()`方法与`fs.readFileSync()`非常相似。如果您有一个大文件要读取，并且您不希望它阻碍您的代码的其余部分，这是非常好的。

这里有一个基本的例子:

```
const fs = require('fs');

fs.readFile('./jokes.json', 'utf8');
```

到目前为止，这看起来类似于我们对`fs.readFileSync()`所做的，除了我们没有把它赋给像`jokesFile`这样的变量。因为它是异步的，`fs.readFile()`之后的任何代码都将在它读完文件之前运行。

相反，我们将使用回调函数并解析其中的 JSON:

```
const fs = require('fs');

fs.readFile('./jokes.json', 'utf8', (err, data) => {
  if (err) console.error(err);
  const jokes = JSON.parse(data);

  console.log(jokes[0].value);
});

console.log("This will run first!");
```

它将以下内容打印到控制台:

```
This will run first!
Chuck Norris's keyboard doesn't have a Ctrl key because nothing controls Chuck Norris. 
```

与`fs.readFileSync()`一样，`fs.readFile()`将文件加载到内存中，这意味着如果文件发生变化，您将需要再次读取该文件。

此外，尽管`fs.readFile()`是异步的，但它最终会将正在读取的整个文件加载到内存中。如果你有一个庞大的文件，那么查看一下 [Node.js 流](https://www.freecodecamp.org/news/node-js-streams-everything-you-need-to-know-c9141306be93/)可能会更好。

### 如何在 Node.js 中用`JSON.stringify()`字符串化 JSON

最后，如果您使用 Node.js 解析 JSON，很有可能在某个时候需要返回 JSON，可能是作为 API 响应。

幸运的是，这与在浏览器中的工作方式相同——只需使用`JSON.stringify()`将 JavaScript 对象文字或数组转换成 JSON 字符串:

```
const newJoke = {
  categories: ['dev'],
  value: "Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."
};

console.log(JSON.stringify(newJoke)); // {"categories":["dev"],"value":"Chuck Norris's keyboard is made up entirely of Cmd keys because Chuck Norris is always in command."} 
```

就是这样！我们已经介绍了在浏览器和 Node.js 项目中使用 JSON 所需了解的一切。

现在开始解析或字符串化 JSON，随心所欲。

我错过了什么吗？如何在项目中解析 JSON？在推特上让我知道。