# 如何在 Go 中实现 Elasticsearch

> 原文：<https://www.freecodecamp.org/news/go-elasticsearch/>

今天，我将向大家展示如何在 Go 中实现 Elasticsearch。当然，在此之前，我会简单介绍一下 Elasticsearch。
如果你已经对 Elasticsearch 有了基本的了解，可以跳到下一部分。

## **弹性搜索**

Elasticsearch 最近越来越受欢迎。在关系数据库中搜索总是存在可伸缩性和性能方面的问题。

Elasticsearch 是 NoSQL 的一个数据库，在解决这些问题上非常成功。它提供了很好的可伸缩性和性能，最突出的特性之一是评分系统，它允许在搜索结果中有很大的灵活性。毕竟不叫弹性——无故搜索！

### 安装弹性搜索

首先，您需要在本地机器上安装 Elasticsearch。你可以去他们的[网站](https://www.elastic.co/guide/index.html)获取[的安装指南](https://www.elastic.co/guide/en/elasticsearch/reference/7.4/install-elasticsearch.html)。在我写这篇文章的时候，我正在使用版本号为 7.4.2 的 Elasticsearch。

Elasticsearch 在他们的版本中做了很多改变，其中之一是移除了映射类型。所以，如果你使用的是另一个版本的 Elasticsearch，就不要指望它能完全发挥作用。

完成安装后，不要忘记运行您的 elasticsearch 服务，这在他们的安装指南中提到得很清楚(对于 linux，简而言之就是这么做`./bin/elasticsearch`)。

**通过请求进入本地机器的端口 9200，确保您的 elasticsearch 正在运行**。

得到`localhost:9200`

点击它应该会显示如下。

```
{
  "name": "204371",
  "cluster_name": "elasticsearch",
  "cluster_uuid": "8Aa0PznuR1msDL9-PYsNQg",
  "version": {
    "number": "7.4.2",
    "build_flavor": "default",
    "build_type": "tar",
    "build_hash": "2f90bbf7b93631e52bafb59b3b049cb44ec25e96",
    "build_date": "2019-10-28T20:40:44.881551Z",
    "build_snapshot": false,
    "lucene_version": "8.2.0",
    "minimum_wire_compatibility_version": "6.8.0",
    "minimum_index_compatibility_version": "6.0.0-beta1"
  },
  "tagline": "You Know, for Search"
}
```

如果显示正确，那么恭喜你！您已经在本地机器上成功运行了 elasticsearch 服务。给自己一个掌声，喝杯咖啡，因为时间还早。

### 制作您的第一个索引

在 Elasticsearch 中，索引类似于数据库。而在之前，elasticsearch 中有一个叫做 type 的表。但是因为在当前版本中已经删除了 type，所以现在只有 index。

现在迷茫了？不要这样。简而言之，只要认为你只需要索引，然后你只需要将你的数据插入到 Elasticsearch。

现在，我们将通过执行下面的查询来制作一个名为`students`的索引。
放`localhost/9200/students`

```
{
	"settings": {
    	"number_of_shards": 1,
    	"number_of_replicas": 1
	},
   "mappings": {
       "properties": {
         "name": {
               "type": "text"
         },
         "age": {
               "type": "integer"      
         },
         "average_score": {
               "type": "float"
         }
     }
   }
}
```

如果没有出错，它应该会给出这个作为回应。

```
{
    "acknowledged": true,
    "shards_acknowledged": true
} 
```

您的索引应该被创建。现在我们将进入下一步:摆弄我们的弹性搜索指数。

### 填充您的弹性搜索

首先，我们现在要做的是用文档填充我们的弹性搜索索引。如果您不熟悉这个定义，只需知道它非常类似于数据库中的行。

在 NoSQL 数据库中，实际上每个文档都可能包含与模式不匹配的不同字段。

但是我们不要这样做——让我们用之前定义的模式来构造我们的列。前面的 API 将允许您在索引中填充文档。

帖子`localhost:9200/students/doc`

```
{
	"name":"Alice",
	"age":17,
	"average_score":81.1
} 
```

你的 Elasticsearch 现在应该有一个文档了。我们需要在弹性搜索中插入更多的数据。当然，我们不会一个接一个地插入我们的学生数据——那会很麻烦！

Elasticsearch 为了一次发送多个请求，专门准备了一个批量 API。让我们用它一次插入多个数据。

帖子`/students/_bulk`

```
{ "index":{"_index": "students" } }
{ "name":"john doe","age":18, "average_score":77.7 }
{ "index":{"_index": "students" } }
{ "name":"bob","age":16, "average_score":65.5 }
{ "index":{"_index": "students" } }
{ "name":"mary doe","age":18, "average_score":97.7 }
{ "index":{"_index": "students" } }
{ "name":"eve","age":15, "average_score":98.9 }
```

### 让我们查询数据

我们最终用几个学生的数据填充了我们的 Elasticsearch。现在让我们做 Elasticsearch 已知的事情:我们将尝试在我们的 Elasticsearch 中搜索我们刚刚插入的数据。

Elasticsearch 支持许多类型的搜索机制，但是对于这个例子，我们将使用一个简单的匹配查询。

让我们通过点击这个 API 来开始我们的搜索:

帖子`localhost:9200/_search`

```
{
    "query" : {
        "match" : { "name" : "doe" }
    }
} 
```

您将得到您的回答以及与您的相应查询相匹配的学生数据。现在你正式成为搜索工程师了！

```
{
    "took": 608,
    "timed_out": false,
    "_shards": {
        "total": 1,
        "successful": 1,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": {
            "value": 2,
            "relation": "eq"
        },
        "max_score": 0.74487394,
        "hits": [
            {
                "_index": "students",
                "_type": "_doc",
                "_id": "rgpef24BTFuh7kXolTpo",
                "_score": 0.74487394,
                "_source": {
                    "name": "john doe",
                    "age": 18,
                    "average_score": 77.7
                }
            },
            {
                "_index": "students",
                "_type": "_doc",
                "_id": "sApef24BTFuh7kXolTpo",
                "_score": 0.74487394,
                "_source": {
                    "name": "mary doe",
                    "age": 18,
                    "average_score": 97.7
                }
            }
        ]
    }
}
```

## 现在我们走吧！

![download--2-](img/21b4b8995fe4e70eeea8fbcc1fef1502.png)

Go in action!

如果你已经读到这一部分，你应该已经掌握了使用 Elasticsearch 的最基本的概念。现在，我们要在 Go 中实现 Elasticsearch。

实现 Elasticsearch 的一个非常原始的方法是，你可以一直对你的 Elasticsearch IP 做 http 请求。但是我们不会那样做。

我发现[这个](https://github.com/olivere/elastic)对于在 Go 中实现 Elasticsearch 非常有帮助。您应该在继续学习 Go 模块之前安装该库。

### 创建您的结构

首先，您肯定需要为您的模型创建一个 struct。在这个例子中，我们将使用与上一个例子相同的建模，在这个例子中是`Student`结构。

```
package main

type Student struct {
	Name         string  `json:"name"`
	Age          int64   `json:"age"`
	AverageScore float64 `json:"average_score"`
}
```

### 建立客户端连接

现在，让我们创建一个允许我们初始化 ES 客户端连接的函数。
如果您在本地主机之外有一个正在运行的 Elasticsearch 实例，您可以简单地更改`SetURL`中的部分。

```
func GetESClient() (*elastic.Client, error) {

	client, err :=  elastic.NewClient(elastic.SetURL("http://localhost:9200"),
		elastic.SetSniff(false),
		elastic.SetHealthcheck(false))

	fmt.Println("ES initialized...")

	return client, err

}
```

### **数据插入**

之后，我们可以做的第一件事就是尝试通过 Go 将我们的数据插入到 Elasticsearch 中。我们将制作一个`Student`的模型，并将其插入我们的 Elasticsearch 客户端。

```
package main

import (
	"context"
	"encoding/json"
	"fmt"

	elastic "gopkg.in/olivere/elastic.v7"
)

func main() {

	ctx := context.Background()
	esclient, err := GetESClient()
	if err != nil {
		fmt.Println("Error initializing : ", err)
		panic("Client fail ")
	}

	//creating student object
	newStudent := Student{
		Name:         "Gopher doe",
		Age:          10,
		AverageScore: 99.9,
	}

	dataJSON, err := json.Marshal(newStudent)
	js := string(dataJSON)
	ind, err := esclient.Index().
		Index("students").
		BodyJson(js).
		Do(ctx)

	if err != nil {
		panic(err)
	}

	fmt.Println("[Elastic][InsertProduct]Insertion Successful")

}
```

### **查询我们的数据**

最后，我们可以做一些搜索。下面的代码可能看起来有点复杂。不过放心，仔细过了对你更有意义。在下面的例子中，我将使用一个基本的匹配查询。

```
package main

import (
	"context"
	"encoding/json"
	"fmt"

	elastic "gopkg.in/olivere/elastic.v7"
)

func main() {

	ctx := context.Background()
	esclient, err := GetESClient()
	if err != nil {
		fmt.Println("Error initializing : ", err)
		panic("Client fail ")
	}

	var students []Student

	searchSource := elastic.NewSearchSource()
	searchSource.Query(elastic.NewMatchQuery("name", "Doe"))

	/* this block will basically print out the es query */
	queryStr, err1 := searchSource.Source()
	queryJs, err2 := json.Marshal(queryStr)

	if err1 != nil || err2 != nil {
		fmt.Println("[esclient][GetResponse]err during query marshal=", err1, err2)
	}
	fmt.Println("[esclient]Final ESQuery=\n", string(queryJs))
    /* until this block */

	searchService := esclient.Search().Index("students").SearchSource(searchSource)

	searchResult, err := searchService.Do(ctx)
	if err != nil {
		fmt.Println("[ProductsES][GetPIds]Error=", err)
		return
	}

	for _, hit := range searchResult.Hits.Hits {
		var student Student
		err := json.Unmarshal(hit.Source, &student)
		if err != nil {
			fmt.Println("[Getting Students][Unmarshal] Err=", err)
		}

		students = append(students, student)
	}

	if err != nil {
		fmt.Println("Fetching student fail: ", err)
	} else {
		for _, s := range students {
			fmt.Printf("Student found Name: %s, Age: %d, Score: %f \n", s.Name, s.Age, s.AverageScore)
		}
	}

}
```

查询应该打印出来，如下所示:

```
ES initialized...
[esclient]Final ESQuery=
 {"query":{"match":{"name":{"query":"Doe"}}}}
```

是的，这个问题将会被发布到 Elasticsearch 中。

如果您从一开始就遵循我的示例，那么您的查询结果也应该是这样的:

```
Student found Name: john doe, Age: 18, Score: 77.700000 
Student found Name: mary doe, Age: 18, Score: 97.700000 
Student found Name: Gopher doe, Age: 10, Score: 99.900000 
```

这就对了。

我关于如何在 Go 中实现 Elasticsearch 的教程到此结束。我希望我已经介绍了在 Go 中使用 Elasticsearch 的基本部分。

要获得关于这个主题的更多信息，你应该阅读一下 Elasticsearch 中的[查询 DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html) 和[函数评分](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-function-score-query.html)，在我看来这是 Elasticsearch 最好的事情之一。

而且 fret 不是，这个例子中使用的库也支持很多 Elasticsearch 特性，甚至是 Elasticsearch 中的函数评分查询。

感谢您通读我的文章！我真的希望它有用，能帮助你开始使用 Elasticsearch。

> 永不停止学习；知识每 14 个月翻一番。~安东尼·安吉洛