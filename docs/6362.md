# GraphQL API 集成简介

> 原文：<https://www.freecodecamp.org/news/a-gentle-introduction-to-graphql-api-integrations-6564312d402c/>

GraphQL 是 REST(或其他 HTTP API 设计)的一个很好的替代品。这是关于使用 GraphQL API 的核心概念的快速介绍。

要查看使用 GraphQL API 的一些示例:

*   在 Python 中，参见[使用 gql 的 Python GraphQL 客户端请求示例](https://codewithhugo.com/python-graphql-client-requests-example-using-gql/)
*   在 JavaScript [浏览器和节点中，查看上周的代码与 Hugo 简讯](https://codewithhugo.com/javascript-graphql-client-requests-in-node-and-the-browser-using-graphql.js/)

### GraphQL 是什么，它解决什么问题？

GraphQL 是[“你的 API 的一种查询语言”](https://graphql.org/)。

简单地说，它让客户端定义它需要什么(嵌套)数据。

如果我们将其与 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) 方法进行比较:

*   “纯”REST 方法是返回任何关联(或嵌套资源)的 id(或资源链接)。
*   不太纯粹的方法是扩展所有嵌套的内容。

第一种情况导致必须进行大量调用来获取所有数据。第二个导致巨大的负载和缓慢的加载时间。

在 GraphQL 中，客户机在请求中声明它希望在响应中扩展、重命名或其他什么。

它有一些很好的副作用，例如，由于客户端定义了它想要的东西，GraphQL 有一种方法来废弃字段，所以不需要对 API 进行版本化。

### (计划或理论的)纲要

[GraphQL](https://github.com/graphql/graphiql)，“一个用于探索 graph QL 的浏览器内集成开发环境。”可通过在浏览器中导航到端点来使用。可以使用 GraphQL CLI 生成模式(需要 Node + npm 5+):

```
npx graphql-cli get-schema --endpoint $BASE_URL/api/graphql --no-all -o schema.graphql
```

### 问题

#### GraphQL 查询概念

#### 菲尔茨

我们希望在查询中返回什么，参见[“字段”的 GraphQL 文档](https://graphql.org/learn/queries/#fields/)。返回字段`name`、`fleeRate`、`maxCP`、`maxHP`的 GraphQL 查询如下:

```
{
  pokemon(name: "Pikachu") {
    name
    fleeRate
    maxCP
    maxHP
  }
}
```

#### 争论

我们将如何向下过滤查询数据，参见[graph QL 文档中的“arguments”](https://graphql.org/learn/queries/#arguments)。为了获得前 10 个口袋妖怪的名字，我们使用`pokemons (first: 10) { FIELDS }`来查看输出[这里是](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemons%20(first%3A%2010)%20%7B%0A%20%20%20%20name%0A%20%20%20%20fleeRate%0A%20%20%20%20maxCP%0A%20%20%20%20maxHP%0A%20%20%7D%0A%7D%0A):

```
{
  pokemons (first: 10) {
    name
    fleeRate
    maxCP
    maxHP
  }
}
```

#### 别名

别名给了我们重命名字段的能力。(参见[“别名”](https://graphql.org/learn/queries/#aliases)的 GraphQL 文档)。我们实际上将使用它来映射查询中的字段，例如从骆驼到蛇的情况:

```
{
  pokemon(name: "Pikachu") {
    evolution_requirements: evolutionRequirements {
      amount
      name
    }
  }
}
```

运行这个查询([这里是](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20evolution_requirements%3A%20evolutionRequirements%20%7B%0A%20%20%20%20%20%20amount%0A%20%20%20%20%20%20name%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A))得到以下结果，其中`evolutionRequirements`是我们给它起的别名。

```
{
  "data": {
    "pokemon": {
      "evolution_requirements": {
        "amount": 50,
        "name": "Pikachu candies"
      }
    }
  }
}
```

#### 碎片

要在类型上扩展的字段的定义。这是一种保持查询干燥的方式，通常会分离出重复、重用或深度嵌套的字段定义，参见[graph QL 文档中的片段](https://graphql.org/learn/queries/#fragments)。这将意味着不去做([见](https://graphql-pokemon.now.sh/?query=%0A%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20id%0A%20%20%20%20number%0A%20%20%20%20weight%20%7B%0A%20%20%20%20%20%20minimum%0A%20%20%20%20%20%20maximum%0A%20%20%20%20%7D%0A%20%20%20%20height%20%7B%0A%20%20%20%20%20%20minimum%0A%20%20%20%20%20%20maximum%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D) [此处的动作查询](https://graphql-pokemon.now.sh/?query=%0A%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20weight%20%7B%0A%20%20%20%20%20%20minimum%0A%20%20%20%20%20%20maximum%0A%20%20%20%20%7D%0A%20%20%20%20height%20%7B%0A%20%20%20%20%20%20minimum%0A%20%20%20%20%20%20maximum%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)):

```
{
  pokemon(name: "Pikachu") {
    weight {
      minimum
      maximum
    }
    height {
      minimum
      maximum
    }
  }
}
```

例如，我们可以这样运行([查询](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20id%0A%20%20%20%20number%0A%20%20%20%20weight%20%7B...FullPokemonDimensions%7D%0A%20%20%20%20height%20%7B...FullPokemonDimensions%7D%0A%20%20%7D%0A%7D%0A%0Afragment%20FullPokemonDimensions%20on%20PokemonDimension%20%7B%0A%20%20minimum%0A%20%20maximum%0A%7D) [这里](https://graphql-pokemon.now.sh/?query=%0A%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20weight%20%7B...FullPokemonDimensions%7D%0A%20%20%20%20height%20%7B...FullPokemonDimensions%7D%0A%20%20%7D%0A%7D%0A%0Afragment%20FullPokemonDimensions%20on%20PokemonDimension%20%7B%0A%20%20minimum%0A%20%20maximum%0A%7D)):

```
{
  pokemon(name: "Pikachu") {
    weight {...FullPokemonDimensions}
    height {...FullPokemonDimensions}
  }
}
fragment FullPokemonDimensions on PokemonDimension {
  minimum
  maximum
}
```

输出相当于:

```
{
  "data": {
    "pokemon": {
      "weight": {
        "minimum": "5.25kg",
        "maximum": "6.75kg"
      },
      "height": {
        "minimum": "0.35m",
        "maximum": "0.45m"
      }
    }
  }
}
```

### 运行 GraphQL 查询

GraphQL 查询可以在 POST 或 GET 上运行，它包括:

#### 帖子(推荐)

*   必需的标题:`Content-Type: application/json`
*   必需的 JSON 主体参数:`query: { # insert your query }`

**原始 HTTP 请求**

```
POST / HTTP/1.1
Host: graphql-pokemon.now.sh
Content-Type: application/json
{
        "query": "{ pokemons(first: 10) { name } }"
}
```

**卷曲**

```
curl -X POST \
  https://graphql-pokemon.now.sh/ \
  -H 'Content-Type: application/json' \
  -d '{
        "query": "{ pokemons(first: 10) { name } }"
    }'
```

#### 得到

*   必需的查询参数:`query`

**原始 HTTP 请求**

```
GET /?query={%20pokemons(first:%2010)%20{%20name%20}%20} HTTP/1.1
Host: graphql-pokemon.now.sh
```

**卷曲**

```
curl -X GET 'https://graphql-pokemon.now.sh/?query={%20pokemons%28first:%2010%29%20{%20name%20}%20}'
```

### 顶级查询

目前在 GraphQL 口袋妖怪 API 上有两种类型的查询:

*   First X pokemon:获取所有项目(查询中定义的任何字段)
*   按名称排序的单个口袋妖怪:通过其 slug(带有查询中定义的任何字段)获取单个项目
*   通过 id 获取单个 Pokemon:通过其 slug 获取单个项目(带有查询中定义的任何字段)

#### 第一个 X 口袋妖怪

以下形式的查询([参见 GraphiQL](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemons(first%3A%205)%20%7B%0A%20%20%20%20name%0A%20%20%7D%0A%7D) 中的实际操作):

```
{
  pokemons(first: 5) {
    name
    # other fields
  }
}
```

#### 单个口袋妖怪的名字

以下形式的查询([参见 GraphiQL](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20name%0A%20%20%20%20classification%0A%20%20%7D%0A%7D) 中的实际操作):

```
{
  pokemon(name: "Pikachu") {
    name
    classification
    # other fields
  }
}
```

> *注意参数值*两边的双引号(`""`)

#### 单个口袋妖怪按 id

以下形式的查询([参见 GraphiQL](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemon(id%3A%20%22UG9rZW1vbjowMjU%3D%22)%20%7B%0A%20%20%20%20name%0A%20%20%20%20classification%0A%20%20%7D%0A%7D) 中的实际操作):

```
{
  pokemon(id: "UG9rZW1vbjowMjU=") {
    name
    classification
    # other fields
  }
}
```

> *注意参数值*两边的双引号(`""`)

### 示例查询

#### 获取一些口袋妖怪来创建优势/劣势/抗性分类

查询([见图一](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemons(first%3A%20100)%20%7B%0A%20%20%20%20name%0A%20%20%20%20image%0A%20%20%20%20maxHP%0A%20%20%20%20types%0A%20%20%20%20weaknesses%0A%20%20%20%20resistant%0A%20%20%7D%0A%7D)):

```
{
  pokemons(first: 100) {
    name
    image
    maxHP
    types
    weaknesses
    resistant
  }
}
```

#### 获得口袋妖怪和进化扩大物理统计和攻击

查询([见图一](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20...PokemonWithAttack%0A%20%20%20%20...FullPhysicalStats%0A%20%20%20%20evolutions%20%7B%0A%20%20%20%20%20%20...FullPhysicalStats%0A%20%20%20%20%20%20...PokemonWithAttack%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A%0Afragment%20PokemonWithAttack%20on%20Pokemon%20%7B%0A%20%20name%0A%20%20attacks%20%7B%0A%20%20%20%20fast%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20type%0A%20%20%20%20%20%20damage%0A%20%20%20%20%7D%0A%20%20%20%20special%20%7B%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20type%0A%20%20%20%20%20%20damage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A%0Afragment%20FullPhysicalStats%20on%20Pokemon%20%7B%0A%20%20height%20%7B%20...FullDimension%20%7D%0A%20%20weight%20%7B%20...FullDimension%20%7D%0A%7D%0A%0Afragment%20FullDimension%20on%20PokemonDimension%20%7B%0A%20%20minimum%0A%20%20maximum%0A%7D)):

```
{
  pokemon(name: "Pikachu") {
    ...PokemonWithAttack
    ...FullPhysicalStats
    evolutions {
      ...FullPhysicalStats
      ...PokemonWithAttack
    }
  }
}
fragment PokemonWithAttack on Pokemon {
  name
  attacks {
    fast {
      name
      type
      damage
    }
    special {
      name
      type
      damage
    }
  }
}
fragment FullPhysicalStats on Pokemon {
  height { ...FullDimension }
  weight { ...FullDimension }
}
fragment FullDimension on PokemonDimension {
  minimum
  maximum
}
```

#### 获得选定的口袋妖怪及其进化名称作为命名字段

查询([见 GraphiQL](https://graphql-pokemon.now.sh/?query=%7B%0A%20%20pikachu%3A%20pokemon(name%3A%20%22Pikachu%22)%20%7B%0A%20%20%20%20...FullPokemon%0A%20%20%20%20evolutions%20%7B%0A%20%20%20%20%20%20...FullPokemon%0A%20%20%20%20%7D%0A%20%20%7D%0A%20%20bulbasaur%3Apokemon(name%3A%20%22Bulbasaur%22)%20%7B%0A%20%20%20%20...FullPokemon%0A%20%20%20%20evolutions%20%7B%0A%20%20%20%20%20%20...FullPokemon%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A%0Afragment%20FullPokemon%20on%20Pokemon%20%7B%0A%20%20name%0A%7D%0A) )。

我们可以使用别名重命名顶级查询。如果我们想做以下事情，这很有帮助:

```
{
  pikachu: pokemon(name: "Pikachu") {
    ...FullPokemon
    evolutions {
      ...FullPokemon
    }
  }
  bulbasaur:pokemon(name: "Bulbasaur") {
    ...FullPokemon
    evolutions {
      ...FullPokemon
    }
  }
}
fragment FullPokemon on Pokemon {
  name
}
```

如果您想了解如何与 GraphQL API 集成:

-在 Python 中，使用 gql
查看 [Python GraphQL 客户端请求示例-在 JavaScript](https://codewithhugo.com/python-graphql-client-requests-example-using-gql/) [浏览器和节点中，使用 Hugo 简讯查看上周的代码](https://codewithhugo.com/javascript-graphql-client-requests-in-node-and-the-browser-using-graphql.js/)

在我的网站上阅读更多我的文章，[与雨果一起编码](https://codewithhugo.com)。