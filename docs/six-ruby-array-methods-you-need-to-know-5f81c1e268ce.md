# 你需要知道的六种 Ruby 数组方法

> 原文：<https://www.freecodecamp.org/news/six-ruby-array-methods-you-need-to-know-5f81c1e268ce/>

数组是编程的基本结构之一。能够快速操作并从中读取数据，对于用计算机成功构建材料至关重要。这里有六种方法是你离不开的。

### 地图/每个

这两种方法非常相似。它们允许你遍历数组中的“每一个”项，并对其进行操作。

查看一些代码:

```
array = [1, 2, 3]
effects = array.each{|x| # create record from x }
added = array.map{ |x| x + 2 }
```

如果我们从`added`开始读，我们会得到`[3, 4, 5]`。如果我们从`effects`开始读，我们还是会得到`[1, 2, 3]`。这两者的区别是:`.map`将返回一个新的修改过的数组，而`.each`将返回原来的数组。

#### 地图中的副作用

如果你习惯于函数式编程，Ruby 的`.map`可能看起来很奇怪。看看这个例子。我的项目中有一个简单的`Event`类:

```
# we create an array of records
2.3.0 :025 > array = [e, e2, e3]
 => [#<Event id: 1, name: nil>, #<Event id: 2, name: nil">, #<Event id: 3, name: nil>]
# so far so good
2.3.0 :026 > new_array = array.map{|e| e.name = "a name"; e}
 => [#<Event id: 1, name: "a name">, #<Event id: 2, name: "a name">, #<Event id: 3, name: "a name">]
# uh-oh, that ain't right
2.3.0 :027 > array
 => [#<Event id: 1, name: "a name">, #<Event id: 2, name: "a name">, #<Event id: 3, name: "a name">]
```

我们可能认为我们正在处理数组中记录的某种副本，但是我们没有。这只是说:小心。您可以很容易地在您的`.map`函数中创建副作用。

好了，放轻松，这是最难的一个。从现在开始一帆风顺。

### 挑选

`.select`允许你在一个数组中“寻找”一个元素。您必须给`.select`一个返回 true 或 false 的函数，这样它就知道是否“保留”一个数组元素。

```
2.3.0 :028 > array = ['hello', 'hi', 'goodbye']
2.3.0 :029 > array.select{|word| word.length > 3}
 => ["hello", "goodbye"]
```

一个稍微复杂一点的例子，可能更接近你实际使用它的方式。让我们在结尾加上`.map`以便更好地衡量:

```
2.3.0 :030 > valid_colors = ['red', 'green', 'blue']
2.3.0 :031 > cars = [{type: 'porsche', color: 'red'}, {type: 'mustang', color: 'orange'}, {type: 'prius', color: 'blue'}]
2.3.0 :032 > cars.select{ |car| valid_colors.include?(car[:color]) }.map{ |car| car[:type]}
=> ["porsche", "prius"]
```

是的，伙计们，你可以加入这些方法来施展不可思议的力量。好吧，你大概能想象出来，但还是很酷。

#### 更干净的语法:。映射(&:方法)

如果我们处理的是 car 对象，而不仅仅是一个简单的散列，我们可以使用更简洁的语法。为了简单起见，我将使用一个不同的例子。也许我们正在准备在 API 中发送的汽车列表，并需要生成 JSON。我们可以使用`.to_json`方法:

```
# using regular map syntax
2.3.0 :047 > cars.select{ |car| valid_colors.include?(car[:color]) }.map{|car| car.to_json}
 => ["{\"type\":\"porsche\",\"color\":\"red\"}", "{\"type\":\"prius\",\"color\":\"blue\"}"]
# using the cleaner syntax
2.3.0 :046 > cars.select{|car| valid_colors.include?(car[:color]) }.map(&:to_json)
 => ["{\"type\":\"porsche\",\"color\":\"red\"}", "{\"type\":\"prius\",\"color\":\"blue\"}"]
```

### 拒绝

`.select`是阴转阳:

```
2.3.0 :048 > cars.reject{|car| valid_colors.include?(car[:color]) }.map{|car| car[:type]}
 => ["mustang"]
```

我们将**拒绝**所有不会使我们的函数返回 true 的东西，而不是**为我们想要的数组项选择**。请记住，我们的 reject 中的**函数决定数组项是否被返回——如果为真，则该项被返回，否则不返回。**

### 减少

Reduce 的结构比我们的其他数组方法更复杂，但它通常用于 Ruby 中非常简单的事情——主要是数学方面的事情。我们取一个数组，然后对数组中的每一项运行一个函数。这一次，我们关心的是从*其他数组项*返回什么。通常我们会把一堆数字加起来:

```
2.3.0 :049 > array = [1, 2, 3]
2.3.0 :050 > array.reduce{|sum, x| sum + x}
 => 6
```

请注意，我们可以用同样的方式处理字符串:

```
2.3.0 :053 > array = ['amber', 'scott', 'erica']
2.3.0 :054 > array.reduce{|sum, name| sum + name}
 => "amberscotterica"
```

如果我们要查看大量的工作记录，这可能会有所帮助。如果我们需要计算总工作时间，或者如果我们想知道上个月所有捐赠的总数。关于`.reduce.` 的最后一个注意事项是，如果你使用的不是常规的旧数字(或字符串)，你需要包含一个起始值作为参数:

```
array = [{weekday: 'Monday', pay: 123}, {weekday: 'Tuedsay', pay: 244}]
array.reduce(0) {|sum, day| sum + day[:pay]}
 => 367
array.reduce(100) {|sum, day| sum + day[:pay]}
 => 467
```

当然，使用`.reduce`还有更高级的方法，但这足以让你开始。

### 加入

我把`.join`作为奖金，因为它太有用了。让我们再次使用我们的汽车:

```
2.3.0 :061 > cars.map{|car| car[:type]}.join(', ')
 => "porsche, mustang, prius"
```

`.join`很像`.reduce`，除了它有一个超级干净的语法。它接受一个参数:一个将被插入到所有数组元素之间的字符串。`.join`从你给它的任何东西中创建一个长字符串，即使你的数组是一堆非字符串的东西:

```
2.3.0 :062 > cars.join(', ')
 => "{:type=>\"porsche\", :color=>\"red\"}, {:type=>\"mustang\", :color=>\"orange\"}, {:type=>\"prius\", :color=>\"blue\"}"
2.3.0 :065 > events.join(', ')
 => "#<Event:0x007f9beef84a08>, #<Event:0x007f9bf0165e70>, #<Event:0x007f9beb5b9170>"
```

### 为什么不把它们都扔在一起呢

让我们一起使用**本帖中的所有**数组方法！10 天的杂务，每件杂务要花多长时间是随机的。我们想知道我们要花在家务上的总时间。这是假设我们懈怠下来，忽略所有超过 15 分钟的事情。或者把能在 5:

```
days = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
days.map{|day| day.odd? ? 
  {task: 'dishes', minutes: Random.rand(20)} :
  {task: 'sweep', minutes: Random.rand(20)}}
  .select{|task| task[:minutes] < 15}
  .reject{|task| task[:minutes] < 5}
  .reduce(0) {|sum, task| sum + task[:minutes]}
```

我的答案是不相关的，因为你会得到不同的随机分钟来完成你的任务。如果这些内容是新鲜的或令人困惑的，就启动 Ruby 控制台，试一试。

PS:`.map`上的那个`? :`业务叫做`ternary`。这只是一个 if-else 语句。我在这里使用它只是为了更好，让一切都在“一”行上。您应该在自己的代码库中避免使用如此复杂的三元组。

下次见！