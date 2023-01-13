# Java 中的 Apache Flink 批处理示例

> 原文：<https://www.freecodecamp.org/news/apache-flink-batch-example-in-java/>

## **Flink 批处理示例 JAVA**

Apache Flink 是一个开源的流处理框架，具有强大的流和批处理功能。

### **先决条件**

*   类似 Unix 的环境(Linux、Mac OS X、Cygwin)
*   饭桶
*   Maven(我们推荐 3.0.4 版本)
*   Java 7 或 8
*   IntelliJ IDEA 或 Eclipse IDE

```
git clone https://github.com/apache/flink.git
cd flink
mvn clean package -DskipTests # this will take up to 10 minutes
```

## 资料组

对于批处理数据，我们将使用这里的数据集:[数据集](http://files.grouplens.org/datasets/movielens/ml-latest-small.zip)在本例中，我们将使用 movies.csv 和 ratings.csv，创建一个新的 java 项目，并将它们放在应用程序库的一个文件夹中。

## 例子

我们将执行一次操作，通过电影类型检索整个数据集的平均评级。

### 环境和数据集

首先创建一个新的 Java 文件，我将把它命名为 AverageRating.java

我们要做的第一件事是创建执行环境，并将 csv 文件加载到数据集中。像这样:

```
ExecutionEnvironment env = ExecutionEnvironment.getExecutionEnvironment();
DataSet<Tuple3<Long, String, String>> movies = env.readCsvFile("ml-latest-small/movies.csv")
  .ignoreFirstLine()
  .parseQuotedStrings('"')
  .ignoreInvalidLines()
  .types(Long.class, String.class, String.class);

DataSet<Tuple2<Long, Double>> ratings = env.readCsvFile("ml-latest-small/ratings.csv")
  .ignoreFirstLine()
  .includeFields(false, true, true, false)
  .types(Long.class, Double.class);
```

在这里，我们为电影创建一个带有<long string="">的数据集，忽略错误、引用和标题行，为电影评级创建一个带有<long double="">的数据集，也忽略标题、无效行和引用。</long></long>

### 弗林克加工

这里我们将使用 flink 处理数据集。结果将是一个字符串列表，双元组。其中流派将在字符串中，平均收视率将在双。

首先，我们将通过每个数据集中存在的电影 Id 来连接评级数据集和电影数据集。这样，我们将创建一个包含电影名称、类型和分数的新元组。稍后，我们将这个元组按流派分组，并将所有相同流派的分数相加，最后我们将分数除以总结果，我们得到了我们想要的结果。

```
List<Tuple2<String, Double>> distribution = movies.join(ratings)
  .where(0)
  .equalTo(0)
  .with(new JoinFunction<Tuple3<Long, String, String>,Tuple2<Long, Double>, Tuple3<StringValue, StringValue, DoubleValue>>() {
    private StringValue name = new StringValue();
    private StringValue genre = new StringValue();
    private DoubleValue score = new DoubleValue();
    private Tuple3<StringValue, StringValue, DoubleValue> result = new Tuple3<>(name,genre,score);

    @Override
    public Tuple3<StringValue, StringValue, DoubleValue> join(Tuple3<Long, String, String> movie,Tuple2<Long, Double> rating) throws Exception {
      name.setValue(movie.f1);
      genre.setValue(movie.f2.split("\\|")[0]);
      score.setValue(rating.f1);
      return result;
    }
})
  .groupBy(1)
  .reduceGroup(new GroupReduceFunction<Tuple3<StringValue,StringValue,DoubleValue>, Tuple2<String, Double>>() {
    @Override
    public void reduce(Iterable<Tuple3<StringValue,StringValue,DoubleValue>> iterable, Collector<Tuple2<String, Double>> collector) throws Exception {
      StringValue genre = null;
      int count = 0;
      double totalScore = 0;
      for(Tuple3<StringValue,StringValue,DoubleValue> movie: iterable){
        genre = movie.f1;
        totalScore += movie.f2.getValue();
        count++;
      }

      collector.collect(new Tuple2<>(genre.getValue(), totalScore/count));
    }
})
  .collect();
```

有了这个，你就有了一个工作的批处理 flink 应用程序。尽情享受吧！