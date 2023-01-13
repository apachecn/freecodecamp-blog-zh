# 电子表格让你沮丧？将行数据转换成 JSON 树轻而易举。

> 原文：<https://www.freecodecamp.org/news/spreadsheets-and-rowsets-getting-you-down-fd6ff7599052/>

像你们中的许多人一样，我经常不得不接受 SQL 查询的结果并将行集转换成 JSON 数据对象。有时我不得不对电子表格中的 CSV 文件做同样的事情。转变过程可能会很麻烦，尽管任何人都可以做到。然而，这既费时又容易出错。这篇文章将向你展示如何使用[**tree ize**](https://www.npmjs.com/package/treeize)**node . js 包，用很少的几行代码简化这个过程。**

**在继续之前，我首先需要一个数据集作为一些例子的基础。这个领域将会是书籍，它们可以进行各种分类。我将使用一个名为 [casual](https://github.com/boo1ean/casual) 的假数据生成器，我之前在 [GraphQL 测试](https://medium.freecodecamp.org/mocking-graphql-with-graphql-tools-42c2dd9d0364)的帖子中用它来模拟。**

**帐簿数据将具有以下结构:**

```
`casual.define('book', () => {
    const author = casual.random_element(authors);

const book = {
        first_name: author.first,
        last_name: author.last,
        title: casual.random_element(author.titles),
        category: casual.random_element(author.category)
    }

return book;
});`
```

**每次我请求一个`casual.book`时，我都会得到一本有一套新价值观的书。这并不完全是随机的。生成器为知名作者使用一些[预定义数据](https://github.com/JeffML/rowsets2json/blob/master/src/mocks/index.js)，为其他作者使用或多或少随机生成的数据。这里有一个例子:**

```
`{ dataset:
   [ { first_name: 'Barbara',
       last_name: 'Cartland',
       title: 'The Pirate and the Piano Teacher',
       category: 'thriller' },
     { first_name: 'Carlie',
       last_name: 'Haley',
       title: 'Digitized Global Orchestration',
       category: 'engineering' },
     { first_name: 'Arthur',
       last_name: 'Doyle',
       title: 'The Case of the Spotted Dick',
       category: 'mystery' },
     { first_name: 'Reinhold',
       last_name: 'Gutmann',
       title: 'Managed Directional Benchmark',
       category: 'management' },
     { first_name: 'Isaac',
       last_name: 'Asimov',
       title: 'Once in a Venusian Sun',
       category: 'science fiction' },
     { first_name: 'R. L.',
       last_name: 'Stein',
       title: 'Why are You Scared of Me?',
       category: 'childrens books' },
     { first_name: 'Alicia',
       last_name: 'Cruickshank',
       title: 'Balanced Local Database',
       category: 'engineering' },
     { first_name: 'Chase',
       last_name: 'Runte',
       title: 'Ergonomic Tertiary Solution',
       category: 'engineering' } ] }`
```

**如果你对这些数据是如何产生的感兴趣，这篇文章中使用的全部源代码可以在[这里](https://github.com/JeffML/rowsets2json)找到。为了增加一点真实性，生成的数据将被扔进一个[内存中的 SQL 数据库](https://www.npmjs.com/package/alasql)供以后检索。以下是 SQL 查询结果的格式:**

```
`SELECT title, category, first_name, last_name
FROM book
JOIN author ON author.id = book.author`
```

**该格式实际上与之前显示的**数据集**的格式相同，例如:**

```
`[ { title: 'Proactive Regional Forecast',
    category: 'mystery',
    first_name: 'Arthur',
    last_name: 'Doyle' },
  { title: 'More Scary Stuff',
    category: 'suspense',
    first_name: 'Steven',
    last_name: 'King' },
  { title: 'Scary Stuff',
    category: 'occult',
    first_name: 'Steven',
    last_name: 'King' },
  { title: 'Persistent Neutral Info Mediaries',
    category: 'management',
    first_name: 'Maegan',
    last_name: 'Frami' },
  { title: 'Enhanced Background Frame',
    category: 'engineering',
    first_name: 'Winifred',
    last_name: 'Turner' },...`
```

**数据集和行集之间的主要区别在于，当从随机生成的数据填充数据库时，我消除了重复的作者(按姓名)和书名(按类别):**

```
`const addEntities = (dataset) => {
    dataset.forEach(d => {
        d.title = d.title.replace(/'/, "''")

        const stmt =
            `
            IF NOT EXISTS (
                select * from author
                where first_name = '${d.first_name}'
                and last_name = '${d.last_name}')
            INSERT INTO author (first_name, last_name) VALUES('${d.first_name}', '${d.last_name}');
            IF NOT EXISTS (
                select * from book
                where title = '${d.title}'
                and category = '${d.category}')
            INSERT INTO book (title, category, author) VALUES('${d.title}', '${d.category}',
            (select id from author where first_name ='${d.first_name}' and last_name = '${d.last_name}'))
            `
        try {
            alasql(stmt)
        } catch (e) {
            console.error(stmt);
            throw (e);
        }

    })
};`
```

**addEntities.js**

### **转换为 JSON**

**![5eHzM7anyF1JlIGM11bcKtslW6D1MVQ2b66g](img/0f8d31e44a91707e4c54f878640ac8b0.png)

Gratuitous kitten picture.** 

**您可能会注意到数据集结果已经是 JSON 格式了。然而，这篇文章的目的是构建一个包含层次结构，以简洁的方式显示作者、书籍和类别之间的关系。对于行集值来说就不是这样了，行集值的结果是美化了的键-值对，每一对都是表行中的列名和值。**

**例如，假设我想列出作者、他们所写的类别以及他们所写的类别中的书名。我想每个类别只显示一次，每个类别中的每本书也应该只列出一次。**

**这是一种非常常见的归约操作，通常应用于行集数据。解决这个问题的一个方法是声明一个容器对象，然后通过循环行集来填充它。典型的实现可能是:**

```
`const handrolled = (rs) => {
    const authors = {}

    rs.forEach(r => {
        const authname = [r.last_name, r.first_name].join(',');
        if (!authors[authname]) {
            authors[authname] = {
                categories: {}
            }
        }
        var author = authors[authname];
        if (!author.categories[r.category]) {
            author.categories[r.category] = {
                titles: []
            }
        }
        var category = author.categories[r.category]
        if (!category.titles.includes(r.title)) {
            category.titles.push(r.title)
        }
    })

    return authors;
}`
```

**handrolled.js**

**层次越深，`handrolled()`方法就越麻烦。局部变量用于减少长路径长度。我们必须记住元结构，以便在 JSON 对象中编写正确的属性初始化。还有比这更简单的吗？**

**返回的结果是:**

```
`...
        "Doyle,Arthur": {
            "categories": {
                "thriller": {
                    "titles": [
                        "The Case of the Spotted Dick",
                        "The Case of the Mashed Potato"
                    ]
                },
                "mystery": {
                    "titles": [
                        "The Case of the Spotted Dick"
                    ]
                }
            }
        },
        "Asimov,Isaac": {
            "categories": {
                "science": {
                    "titles": [
                        "Once in a Venusian Sun",
                        "Total Multi Tasking Forecast"
                    ]
                },
                "general interest": {
                    "titles": [
                        "Total Multi Tasking Forecast",
                        "Once in a Venusian Sun",
                        "Fourth Foundation"
                    ]
                }
            }
        },
        "Kilback,Bradley": {
            "categories": {
                "management": {
                    "titles": [
                        "Mandatory Solution Oriented Leverage"
                    ]
                },
                "engineering": {
                    "titles": [
                        "Multi Layered Fresh Thinking Framework",
                        "Total Scalable Neural Net",
                        "Mandatory Solution Oriented Leverage"
                    ]
                },
                "reference": {
                    "titles": [
                        "Multi Layered Fresh Thinking Framework"
                    ]
                }
            }
        },...`
```

### **构建一棵具有树大小的树**

**npm 模块 treeize 旨在通过使用描述性键来简化行集到结构化 JSON 数据的转换。通过 npm 的安装通常是:**

```
`npm install --save treeize`
```

#### **JSON 行集**

**Treeize 能够识别行集中重复出现的模式。它根据作为种子结构传入的元数据中如何定义键名来转换它们。代码如下:**

```
`import Treeize from 'treeize'

const treeized = rs => {
    var authors = new Treeize();

    const seed = rs.map(r => ({
        name: [r.last_name, r.first_name].join(', '),
        'categories:type': r.category,
        'categories:titles:name': r.title
    }))

    authors.grow(seed);
    return authors.getData();
}`
```

**treeize.js**

**这大约是十几行代码，相比之下，手卷版本的代码要多一倍。请注意映射操作中使用的键值。Treeize 将复数识别为集合，所以`categories`和`titles`将是数组。名称中的冒号(':')表示嵌套。`Type`将是类别数组中一个对象的属性，`name`将是标题中所有对象的属性。**

**树是在调用`authors.grow(seed)`时构建的，通过`authors.getData()`检索结果。然而，它并没有*完全*产生与我们从手擀面方法中得到的相同的结果:**

```
`...,
{
    "name": "Glover, Ashley",
    "categories": [
        {
            "type": "engineering",
            "titles": [
                {
                    "name": "Intuitive Full Range Capacity"
                },
                {
                    "name": "Organic Encompassing Core"
                }
            ]
        },
        {
            "type": "reference",
            "titles": [
                {
                    "name": "Distributed Client Server Service Desk"
                },
                {
                    "name": "Organic Encompassing Core"
                }
            ]
        },
        {
            "type": "management",
            "titles": [
                {
                    "name": "Organic Encompassing Core"
                }
            ]
        }
    ]
},...`
```

**一个显著的区别是类别不是命名的对象(像以前一样)，而是具有`name`属性的对象。`Title`也不仅仅是一个字符串数组，而是一个以`name`为标题的对象数组。Treeize 将`categories`和`titles`解释为对象数组，而不是贴图(或图元数组)。对于大多数用例来说，这不是什么大问题。但是，如果您需要通过名称快速找到一个类别(而不是遍历一个类别数组)，那么您可以通过几个归约运算来处理这个[，以获得与之前相同的结构:](https://gist.github.com/JeffML/1bbe228f271765d3ee3a917196e8a81c)**

```
`,...   
    "Doyle, Arthur": {
        "categories": {
            "mystery": {
                "titles": [
                    "The Case of the Spotted Dick",
                    "Pre Emptive Needs Based Approach",
                    "The Case of the Mashed Potato"
                ]
            },
            "thriller": {
                "titles": [
                    "The Case of the Mashed Potato",
                    "The Pound Puppies of the Baskervilles"
                ]
            }
        }
    },...`
```

#### **电子表格**

**有时数据来自电子表格，而不是关系数据库。Treeize 也擅长处理这种情况。我们没有像处理 JSON 格式的行集数据那样使用描述性键，而是使用相同的描述性格式作为标题行中的列值:**

```
`var seed = [
['name', 'categories:type', 'categories:titles:name'], 
['Doyle, Arthur', 'mystery', 'The Adventure of the Gyring Gerbils'],
['Schuppe, Katarina', 'engineering', 'Configurable Discrete Locks'],
['Doyle, Arthur', 'mystery', 'Holmes Alone 2'],
['Asimov, Isaac', 'science fiction', 'A Crack in the Foundation']
];

// same as before...
var authors = new Treeize();
authors.grow(seed);
return authors.getData();`
```

**treeize 支持相当多的选项，我只展示了基本的选项。它是一个强大的工具，可以轻松地转换基于行的数据结构。**

**完整的源码[可以在我的 GitHub](https://github.com/JeffML/rowsets2json) 找到。**