# 如何在 Flutter 中序列化一个对象

> 原文：<https://www.freecodecamp.org/news/serialize-object-flutter/>

如果您打算将用户数据保存到 Flutter 应用程序的共享首选项或本地存储中，您将需要手动序列化它。

这是因为这两种方法都只支持有限的基本对象类型，并且您的对象可能不属于任何类别。为此，我们将使用******dart:convert******解码器及其 jsonEncode/jsonDecode 方法，因此请确保将其导入到您的项目中。

为此，请在主 dart 文件中导入该类:

```
import 'dart:convert'; 
```

## 如何在 Dart 中构建模型类

为了让我们的对象能够解码/编码，我们首先需要为它创建一个模型类。这个类将表示对象及其字段，并具有重要的方法来完成编码/解码的繁重工作。第一个叫做******from JSON******第二个叫做 ****toJson**** 。我们将展示复杂对象的各种例子以及如何序列化它们，但是首先，我们将从一个简单的例子开始。

我们想象有一家甜甜圈店，里面有各种各样的甜甜圈。每个圆环都是一个包含以下键的对象:

*   名称字符串
*   填充绳
*   顶线
*   价格翻倍

在 Dart 中，它是这样的:

```
class Doughnut {
  final String name;
  final String filling;
  final String topping;
  final double price;

  Doughnut(this.name, this.filling, this.topping, this.price);

  Doughnut.fromJson(Map<String, dynamic> json)
      : name = json['name'],
        filling = json['filling'],
        topping = json['topping'],
        price = json['price'];

  Map<String, dynamic> toJson() => {
    'name' : name,
    'filling' : filling,
    'topping' : topping,
    'price' : price
  };
}
```

你可能已经注意到里面有一种怪异的类型叫做*。Dynamic 表示 dart 语言中的未知类型，它将在运行时实现。可以把它想象成类似于 Java 中的 Object 类型，所有类型都继承自它。任何对象都可以被转换成动态的，以调用其上的方法。如果没有为变量或方法的返回类型声明类型，则假定它是动态的。*

*![image-242](img/38f2ceb52f78e3971f4b973bd41cc028.png)

Photo by [Sharon McCutcheon](https://unsplash.com/@sharonmccutcheon?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)* 

## *如何在 Flutter 中序列化和反序列化数据*

*现在我们有了模型类，我们可以用它来序列化和反序列化我们的数据。下面是一个如何做到这一点的例子。*

*创建甜甜圈对象很简单:*

```
*`Doughnut myDoughnut = new Doughnut("Glazed", "None", "Sprinkles", 2.99);`* 
```

*现在我们可以使用 jsonEncode 来序列化它:*

```
*`String encodedDoughnut = jsonEncode(myDoughnut);`* 
```

*在幕后，jsonEncode 调用我们自己在甜甜圈模型类中创建的 toJson 方法。这同样适用于 jsonDecode，因为它调用 fromJson 方法。*

*在字符串形式中，我们的甜甜圈对象现在看起来像这样:*

```
*`{"name":"Glazed","filling":"None","topping":"Sprinkles","price":2.99}`* 
```

*我们解码后，*

```
*`Map<String, dynamic> decodedDoughnut = jsonDecode(encodedDoughnut);`* 
```

*我们将获得:*

```
*`{name: Glazed, filling: None, topping: Sprinkles, price: 2.99}`* 
```

*你注意到细微的差别了吗？*

*解码后的对象现在是一个带有字符串类型的键和动态类型的值的映射。如果我们想将这种数据类型转换回我们的原始对象，我们需要访问映射的属性:*

```
*`Doughnut newDoughnut = new Doughnut(decodedDoughnut["name"], 
                                    decodedDoughnut["filling"], 
                                    decodedDoughnut["topping"], 
                                    decodedDoughnut["price"]);`*
```

## *如何在 Dart 中序列化数组*

*生活不是甜甜圈那么简单(要是就好了，对吧？)，所以我们必须考虑到，我们不会总是处理简单的参数类型。让我们假设 toppings 字段不是一个字符串，而是一个字符串数组。首先，让我们重新定义浇头字段:*

```
*`String topping => List<String> toppings;`* 
```

*然后，在我们的 fromJson 方法中，我们必须执行以下操作:*

```
*`Doughnut.fromJson(Map<String, dynamic> json) {
    name = json['name'];
    filling = json['filling'];
    var toppingsFromJson = json['toppings'];
    toppings = new List<String>.from(toppingsFromJson);
    price = json['price'];

  }`*
```

*请注意，我们如何使用一个临时变量首先从 json 变量中提取值，然后显式地将值转换为浇头列表的原始类型。我们必须这样做，因为首先，toppings 的值的类型是动态的，dart 不知道它是哪种类型。* 

## *复杂物体*

*没有人会只买一个甜甜圈，对吧？那是没用的。假设我们有一打。所以现在我们正在处理一个甜甜圈类型的对象列表。为了序列化/反序列化对象列表，我们将使用上面的模型类，但是我们需要创建一个不同的模型类来处理列表。*

```
*`import 'package:serialization_example/models/doughnut.dart';

class DoughnutList {
  final List<Doughnut> doughnuts;

  DoughnutList(this.doughnuts);

  DoughnutList.fromJson(Map<String, dynamic> json)
  : doughnuts = json['doughnuts'] != null ? List<Doughnut>.from(json['doughnuts']) : null;

  Map<String, dynamic> toJson()  =>
  {
    'doughnuts': doughnuts,
  };

}`*
```

*如您所见，我们现在有了一个模型类，它有一个 List 类型的字段，用于保存甜甜圈对象。*

*与上面的例子类似，为了序列化对象，我们使用 jsonEncode 方法，当我们想要解码对象时，我们必须执行以下操作:*

```
*`Map<String, dynamic> decodedDoughnuts = jsonDecode(encodedJson);
List<dynamic> decodedJson =  decodedDoughnuts['doughnuts'];
decodedJson.map((elem) => jsonDecode(elem));`*
```

*要查看我们在本文中涉及的所有内容，请前往 GitHub 上的资源库[。](https://github.com/TomerPacific/MediumArticles/tree/master/serialization_example)*