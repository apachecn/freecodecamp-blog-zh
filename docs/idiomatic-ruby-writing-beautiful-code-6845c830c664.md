# 惯用的 Ruby:编写漂亮的代码

> 原文：<https://www.freecodecamp.org/news/idiomatic-ruby-writing-beautiful-code-6845c830c664/>

Ruby 是一种漂亮的编程语言。

根据 [Ruby](http://www.ruby-lang.org/en/) 的官方网页，Ruby 是一个:

> ***动态、开源的编程语言，专注于简单性和生产力。它有一个优雅的语法，读起来很自然，写起来也很容易。”***

**Ruby 是由日本软件工程师松本幸宏(Yukihiro Matsumoto)创造的。自 2011 年起，他在 [Heroku](https://www.heroku.com/) 担任 Ruby 的首席设计师&软件工程师。**

**松本经常说，他试图让红宝石自然，而不是简单，以一种反映生活的方式。**

> **“红宝石外表简单，但内部非常复杂，就像我们的人体。”——松本幸宏**

**我对露比也有同感。它是一种复杂但非常自然的编程语言，具有漂亮而直观的语法。**

**有了更直观和更快的代码，我们就能够构建更好的软件。在这篇文章中，我将向你展示我如何用 Ruby 通过使用代码片段来表达我的想法(也就是代码)。**

### **用数组方法表达我的想法**

#### **地图**

**使用 **map** 方法来简化你的代码，得到你想要的。**

**方法 **map** 返回一个新数组，其结果是对 enum 中的每个元素运行一次块。**

**让我们来试试:**

```
`an_array.map { |element| element * element }`
```

**就这么简单。**

**但是当你开始用 Ruby 编码时，总是使用 **each** 迭代器是很容易的。**

**每个迭代器的**如下所示****

```
`user_ids = []
users.each { |user| user_ids << user.id }`
```

**可以用一行漂亮的代码简化**图**:**

```
`user_ids = users.map { |user| user.id }`
```

**或者更好(更快):**

```
`user_ids = users.map(&:id)`
```

#### **挑选**

**而且当你习惯用 **map** 编码的时候，有时候你的代码可以是这样的:**

```
`even_numbers = [1, 2, 3, 4, 5].map { |element| element if element.even? } # [ni, 2, nil, 4, nil]
even_numbers = even_numbers.compact # [2, 4]`
```

**使用 **map** 只选择偶数也将返回**nil** 对象。使用**压缩**方法移除所有**零**对象。**

**哒哒，你选择了所有的偶数。**

**任务完成。**

**加油，我们可以做得更好！你听说过从可枚举模块中选择方法吗？**

```
`[1, 2, 3, 4, 5].select { |element| element.even? }`
```

**就一句台词。简单的代码。很好理解。**

#### ****奖金****

```
`[1, 2, 3, 4, 5].select(&:even?)`
```

#### **样品**

**假设你需要从一个数组中获取一个随机元素。您刚刚开始学习 Ruby，所以您的第一个想法将是，“让我们使用**随机**方法”，这就是所发生的情况:**

```
`[1, 2, 3][rand(3)]`
```

**嗯，我们可以理解代码，但我不确定它是否足够好。如果我们使用**洗牌**的方法呢？**

```
`[1, 2, 3].shuffle.first`
```

**嗯。比起 T2 和 T3，我更喜欢用 T0 洗牌 T1。但是当我发现**样品**方法时，它变得更有意义了:**

```
`[1, 2, 3].sample`
```

**非常非常简单。**

**非常自然和直观。我们从一个数组中获取一个**样本**，然后方法返回它。现在我很开心。**

**你呢？**

### **用 Ruby 语法表达我的想法**

**正如我之前提到的，我喜欢 Ruby 让我编码的方式。对我来说真的很自然。我将展示部分漂亮的 Ruby 语法。**

#### **隐性回报**

**Ruby 中的任何语句都返回上一次计算的表达式的值。一个简单的例子是 **getter** 方法。我们调用一个方法并期望得到一些回报。**

**让我们看看:**

```
`def get_user_ids(users)
  return users.map(&:id)
end`
```

**但是我们知道，Ruby 总是返回最后一个求值的表达式。为什么要使用 **return** 语句？**

**在使用 Ruby 年后，我感觉几乎每种方法都很棒，没有使用 **return** 语句。**

```
`def get_user_ids(users)
  users.map(&:id)
end`
```

#### **多重任务**

**Ruby 允许我同时给多个变量赋值。开始时，您可能会这样编码:**

```
`def values
  [1, 2, 3]
end

one   = values[0]
two   = values[1]
three = values[2]`
```

**但是为什么不同时给多个变量赋值呢？**

```
`def values
  [1, 2, 3]
end

one, two, three = values`
```

**相当棒。**

#### **提问的方法(也称为谓词)**

**在我学习 Ruby 的时候引起我注意的一个特性是**问号(？)**方法，也叫**谓词**方法。一开始看起来很奇怪，但现在看起来很有意义。您可以编写这样的代码:**

```
`movie.awesome # => true`
```

**好吧…这没什么不对。但是让我们用问号:**

```
`movie.awesome? # => true`
```

**这段代码更具表现力，我希望方法的答案要么返回 **true** 要么返回 **false** 值。**

**我常用的一个方法是 **any？**这就像问一个数组里面有没有**任何**的东西。**

```
`[].any? # => false
[1, 2, 3].any? # => true`
```

#### **插入文字**

**对我来说，字符串插值比字符串串联更直观。句号。让我们看看它的实际效果。**

**字符串串联的一个示例:**

```
`programming_language = "Ruby"
programming_language + " is a beautiful programming_language" # => "Ruby is a beautiful programming_language"`
```

**字符串插值的一个示例:**

```
`programming_language = "Ruby"
"#{programming_language} is a beautiful programming_language" # => "Ruby is a beautiful programming_language"`
```

**我更喜欢字符串插值。**

**你怎么想呢?**

#### **if 语句**

**我喜欢使用 **if** 语句:**

```
`def hey_ho?
  true
end

puts "let’s go" if hey_ho?`
```

**像这样编码很不错。**

**感觉很自然。**

#### **try 方法(Rails 模式打开时)**

****try** 方法调用由符号标识的方法，向其传递任何参数和/或指定的块。这类似于 Ruby 的**对象#send。**与该方法不同，如果接收对象是 **nil** 对象或 **NilClass，将返回 **nil** 。****

**使用 **if and unless** 条件语句:**

```
`user.id unless user.nil?`
```

**使用**尝试**方法:**

```
`user.try(:id)`
```

**从 Ruby 2.3 开始，我们可以使用 Ruby 的安全导航操作符 **( &)。)**代替铁轨**试试**的方法。**

```
`user&.id`
```

#### **双管等于( **||=)** /记忆化**

**这个特性太 C-O-O-L 了，就像在变量中缓存一个值。**

```
`some_variable ||= 10
puts some_variable # => 10

some_variable ||= 99
puts some_variable # => 10`
```

**你不需要使用 **if** 语句。只要用双管等于 **(||=)** 就搞定了。**

**简单易行。**

#### **类静态方法**

**我喜欢编写 Ruby 类的一种方式是定义一个**静态**方法(类方法)。**

```
`GetSearchResult.call(params)`
```

**简单。太美了。直觉。**

**后台发生了什么？**

```
`class GetSearchResult
  def self.call(params)
    new(params).call
  end

  def initialize(params)
    @params = params
  end

  def call
    # ... your code here ...
  end
end`
```

****self.call** 方法初始化一个实例，这个对象调用 **call** 方法。 [**交互器设计模式**](https://github.com/collectiveidea/interactor) 使用它。**

#### **Getters 和 setters**

**对于同一个 **GetSearchResult** 类，如果我们想使用 params，我们可以使用@params**

```
`class GetSearchResult
  def self.call(params)
    new(params).call
  end

  def initialize(params)
    @params = params
  end

  def call
    # ... your code here ...
    @params # do something with @params
  end
end`
```

**我们定义了一个**设置器**和**获取器:****

```
`class GetSearchResult
  def self.call(params)
    new(params).call
  end

  def initialize(params)
    @params = params
  end

  def call
    # ... your code here ...
    params # do something with params method here
  end

  private

  def params
    @params
  end

  def params=(parameters)
    @params = parameters
  end
end`
```

**或者我们可以定义 **attr_reader** 、 **attr_writer、**或者 **attr_accessor****

```
`class GetSearchResult
  attr_reader :param

  def self.call(params)
    new(params).call
  end

  def initialize(params)
    @params = params
  end

  def call
    # ... your code here ...
    params # do something with params method here
  end
end`
```

**很好。**

**我们不需要定义 **getter** 和 **setter** 方法。代码变得更简单了，这正是我们想要的。**

#### **轻敲，水龙头**

**假设您想要定义一个 **create_user** 方法。该方法将实例化、设置参数、保存并返回用户。**

**让我们开始吧。**

```
`def create_user(params)
  user       = User.new
  user.id    = params[:id]
  user.name  = params[:name]
  user.email = params[:email]
  # ...
  user.save
  user
end`
```

**简单。这里没什么问题。**

**现在让我们用 **tap** 方法来实现它**

```
`def create_user(params)
  User.new.tap do |user|
    user.id    = params[:id]
    user.name  = params[:name]
    user.email = params[:email]
    # ...
    user.save
  end
end`
```

**你只需要担心用户参数， **tap** 方法会为你返回用户对象。**

### **好了**

**我们学习了如何用**

*   **数组方法**
*   **句法**

**我们还了解了 Ruby 是如何的漂亮和直观，并且运行得更快。**

**就这样，伙计们！我会在我的[博客](https://medium.com/@leandrotk_/)中更新和包含更多的细节。这个想法是分享伟大的内容，社区有助于改善这个职位！☺**

**我希望你们能欣赏这些内容，并学会如何编写漂亮的代码(和更好的软件)。**

**如果你想要一个完整的 Ruby 课程，学习真实世界的编码技能，搭建项目，试试 [***一个月 Ruby Bootcamp***](https://onemonth.com/courses/ruby?mbsy=lG6tt&mbsy_source=97541b09-e3ab-45d7-a9b1-dbc77028e008&campaignid=33446&discount_code=TKRuby1) 。那里见，☺**

**本帖最早出现在我的 [**文艺复兴开发者刊物**](https://medium.com/the-renaissance-developer) 上 [**这里**](https://medium.com/the-renaissance-developer/idiomatic-ruby-1b5fa1445098) 。**

**玩得开心，不断学习，一直坚持编码！**

**我的[Twitter](https://twitter.com/LeandroTk_)&[Github](https://github.com/LeandroTk)。☺**