# 对任何对象列表使用 Java 的 Arrays.sort()

> 原文：<https://www.freecodecamp.org/news/utilizing-javas-arrays-sort-for-any-list-of-objects-e3e2db61d70b/>

排序可能很棘手，尤其是当您的列表不是基本的 Java 数值类型(字节、整数、短整型、长整型、双精度型、浮点型)时。现在，所有的情况会有所不同，所以这种方法可能不是最好的情况。然而，我发现它对于简单的编码挑战和大学实验室作业非常有用。

首先，选择你的清单。对于这个例子，我将使用一个简单的`Graph`数据结构中的`Edges`列表:

```
// Very simple Edge classpublic class Edge {    public Vertex src;    public Vertex dst;    public double cost;        // creates an edge between two vertices    Edge(Vertex s, Vertex d, double c) {        src = s;        dst = d;        cost = c;    }}
```

```
// List of edgesEdge[] edges = graph.getEdges();
```

接下来，定义`java.util.Comparator`接口的实现:

```
class SortByCost implements Comparator<Edge> {    public int compare(Edge a, Edge b) {        if ( a.cost < b.cost ) return -1;        else if ( a.cost == b.cost ) return 0;        else return 1;    }}
```

在这个例子中，我们将根据成本或者从`src`(源)顶点到`dst`(目的地)顶点的距离对`edges`进行排序。

最后使用标准的`java.util.Arrays.sort()`方法:

```
Arrays.sort(edges, new SortByCost())
```

就这样， `Edges`列表现在按照升序(从少到多)排序。

如果您有任何问题，请随时联系 [Twitter](https://twitter.com/ArrowoodTech)

你也可以在 [GitHub](https://github.com/ethan-arrowood) 或者我的个人[网站](https://ethanarrowood.com)上找到我

~快乐编码

——伊桑·阿罗伍德