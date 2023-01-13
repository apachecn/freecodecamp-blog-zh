# GraphQL 中的突变介绍:它们是什么以及如何使用它们

> 原文：<https://www.freecodecamp.org/news/an-intro-to-mutations-in-graphql-what-they-are-and-how-to-use-them-e959735abd8d/>

这篇博文是我之前关于 GraphQL 基础的博文的延续。[点击此处](https://medium.freecodecamp.org/an-introduction-to-graphql-how-it-works-and-how-to-use-it-91162ecd72d0)查看 GraphQL 基础帖子。

为了充分利用这篇文章，有必要阅读 GraphQL 基础知识的帖子。

### GraphQL 中的突变是什么？

每当您想要将数据写回服务器时，就会使用突变。

### 突变和查询有什么不同？

**查询**在你想从服务器读取一些数据的时候使用。**突变**用在你想把数据写回服务器的时候。

但是等等。难道我不能去**查询**中的解析器做一个写操作吗？

虽然可以在**查询**中进行写操作，但是不应该这样做。有必要将读写操作分开，因此需要**突变**。

### 密码

[点击这里](https://github.com/aditya-sridhar/graphql-with-nodejs)从我之前的博客文章中获取代码。在本文中，我们将在这段代码中添加变异逻辑。

### 添加电影突变

让我们创建一个可以用来添加新电影的突变。

创建一个名为 **mutation.js** 的新文件。将以下代码复制到 **mutation.js** 中:

```
const { GraphQLObjectType
} = require('graphql');
const _ = require('lodash');

const {movieType} = require('./types.js');
const {inputMovieType} = require('./inputtypes.js');
let {movies} = require('./data.js');

const mutationType = new GraphQLObjectType({
    name: 'Mutation',
    fields: {
        addMovie: {
            type: movieType,
            args: {
                input: { type: inputMovieType }
            },
            resolve: function (source, args) {

                let movie = {
                    id: args.input.id, 
                    name: args.input.name, 
                    year: args.input.year, 
                    directorId: args.input.directorId};

                movies.push(movie);

                return _.find(movies, { id: args.input.id });
            }
        }
    }
});

exports.mutationType = mutationType;
```

您会注意到一个突变看起来非常类似于一个查询。主要区别在于 **GraphQLObjectType** 的名字是**突变**。

这里我们添加了一个名为 **addMovie** 的变种，它的返回类型为**movie type**(*movie type*在[之前的](https://adityasridhar.com/posts/what-is-graphql-and-how-to-use-it)博客中有所介绍)。

在 args 中，我们提到我们需要一个名为 **input** 的参数，它的类型是 **inputMovieType**

那么这里的 **inputMovieType** 是什么呢？

### 输入类型

可能多个突变需要相同的输入参数。因此，创建**输入类型**并为所有这些突变重用输入类型是一个好的实践。

这里我们为电影创建一个输入类型，叫做 **inputMovieType** 。

我们可以看到 **inputMovieType，**依次来自 **inputtypes.js** 文件。让我们现在就创造这一切。

创建名为 **inputtypes.js.** 的新文件

将以下代码复制到 inputtypes.js 中:

```
const {
    GraphQLInputObjectType,
    GraphQLID,
    GraphQLString,
    GraphQLInt
} = require('graphql');

inputMovieType = new GraphQLInputObjectType({
    name: 'MovieInput',
    fields: {
        id: { type: GraphQLID },
        name: { type: GraphQLString },
        year: { type: GraphQLInt },
        directorId: { type: GraphQLID }

    }
});

exports.inputMovieType = inputMovieType;
```

我们可以看到，一个输入类型看起来与 GraphQL 中的任何其他类型完全一样。**graphqinputobjecttype**用于创建输入类型，而**graphqobjecttype**用于创建普通类型。

### 解析突变的功能

变异的解析功能是实际写操作发生的地方。

在实际应用中，这可能是一个数据库写操作。

对于这个例子，我们只是将数据添加到电影数组，然后将添加的电影返回。

```
resolve: function (source, args) {

                let movie = {
                    id: args.input.id, 
                    name: args.input.name, 
                    year: args.input.year, 
                    directorId: args.input.directorId};

                movies.push(movie);

                return _.find(movies, { id: args.input.id });
            }
```

resolve 中的上述代码执行以下操作:

*   从**输入** arg 获取输入电影参数。
*   将新电影添加到电影数组中
*   返回通过从电影数组中获取而添加的新电影

### 添加导向突变

让我们创建一个突变，可以用来添加一个新的指导。

这与添加电影突变是一样的。

**inputtypes.js** 添加了导演突变:

```
const {
    GraphQLInputObjectType,
    GraphQLID,
    GraphQLString,
    GraphQLInt
} = require('graphql');

inputMovieType = new GraphQLInputObjectType({
    name: 'MovieInput',
    fields: {
        id: { type: GraphQLID },
        name: { type: GraphQLString },
        year: { type: GraphQLInt },
        directorId: { type: GraphQLID }

    }
});

inputDirectorType = new GraphQLInputObjectType({
    name: 'DirectorInput',
    fields: {
        id: { type: GraphQLID },
        name: { type: GraphQLString },
        age: { type: GraphQLInt }

    }
});

exports.inputMovieType = inputMovieType;
exports.inputDirectorType = inputDirectorType;
```

**突变. js** 后添加 **addDirector** 突变:

```
const { GraphQLObjectType
} = require('graphql');
const _ = require('lodash');

const {movieType,directorType} = require('./types.js');
const {inputMovieType,inputDirectorType} = require('./inputtypes.js');
let {movies,directors} = require('./data.js');

const mutationType = new GraphQLObjectType({
    name: 'Mutation',
    fields: {
        addMovie: {
            type: movieType,
            args: {
                input: { type: inputMovieType }
            },
            resolve: function (source, args) {

                let movie = {
                    id: args.input.id, 
                    name: args.input.name, 
                    year: args.input.year, 
                    directorId: args.input.directorId};

                movies.push(movie);

                return _.find(movies, { id: args.input.id });
            }
        },
        addDirector: {
            type: directorType,
            args: {
                input: { type: inputDirectorType }
            },
            resolve: function (source, args) {
                let director = {
                    id: args.input.id, 
                    name: args.input.name, 
                    age: args.input.age};

                directors.push(director);

                return _.find(directors, { id: args.input.id });
            }
        }
    }
});

exports.mutationType = mutationType;
```

### 促成突变

到目前为止，我们已经定义了突变终点及其功能。但是我们还没有启动变异。

要启用它们，必须将突变添加到模式中。

使用以下代码在 **server.js** 中添加突变:

```
// Define the Schema
const schema = new GraphQLSchema(
    { 
        query: queryType,
        mutation: mutationType 
    }
);
```

添加变异后完成 **server.js** 中的代码:

```
//get all the libraries needed
const express = require('express');
const graphqlHTTP = require('express-graphql');
const {GraphQLSchema} = require('graphql');

const {queryType} = require('./query.js');
const {mutationType} = require('./mutation.js');

//setting up the port number and express app
const port = 5000;
const app = express();

 // Define the Schema
const schema = new GraphQLSchema(
    { 
        query: queryType,
        mutation: mutationType 
    }
);

//Setup the nodejs GraphQL server 
app.use('/graphql', graphqlHTTP({
    schema: schema,
    graphiql: true,
}));

app.listen(port);
console.log(`GraphQL Server Running at localhost:${port}`);
```

### 密码

本文的完整代码可以在 [this git repo](https://github.com/aditya-sridhar/graphql-mutations-with-nodejs) 中找到。

### 测试突变终点

使用以下命令运行应用程序:

```
node server.js
```

打开你的网络浏览器，进入以下网址 **localhost:5000/graphql**

### 测试 addMovie 突变端点

输入:

```
mutation {
	addMovie(input: {id: 4,name: "Movie 4", year: 2020,directorId:4}){
    id,
    name,
	year,
    directorId
  }

}
```

输出:

```
{
  "data": {
    "addMovie": {
      "id": "4",
      "name": "Movie 4",
      "year": 2020,
      "directorId": "4"
    }
  }
}
```

输入:

```
mutation {
	addMovie(input: {id: 5,name: "Movie 5", year: 2021,directorId:4}){
    id,
    name,
	year,
    directorId
  }

}
```

输出:

```
{
  "data": {
    "addMovie": {
      "id": "5",
      "name": "Movie 5",
      "year": 2021,
      "directorId": "4"
    }
  }
}
```

### 测试 addDirector 突变端点

输入:

```
mutation {
	addDirector(input: {id: 4,name: "Director 4", age: 30}){
    id,
    name,
	age,
    movies{
      id,
      name,
      year
    }
  }

}
```

输出:

```
{
  "data": {
    "addDirector": {
      "id": "4",
      "name": "Director 4",
      "age": 30,
      "movies": [
        {
          "id": "4",
          "name": "Movie 4",
          "year": 2020
        },
        {
          "id": "5",
          "name": "Movie 5",
          "year": 2021
        }
      ]
    }
  }
}
```

### 恭喜你。

您现在知道 GraphQL 中的突变了！

您可以查看[文档](https://graphql.github.io/learn/)来了解更多关于 GraphQL 的信息。

### 关于作者

我热爱技术，关注该领域的进步。

请随时通过我的 LinkedIn 账户与我联系[https://www.linkedin.com/in/aditya1811/](https://www.linkedin.com/in/aditya1811/)

你也可以在推特上关注我[https://twitter.com/adityasridhar18](https://twitter.com/adityasridhar18)

我的网站:[https://adityasridhar.com/](https://adityasridhar.com/)

在 adityasridhar.com 的博客上阅读更多我的文章。