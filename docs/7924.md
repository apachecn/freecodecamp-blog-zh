# 通过使用。选择。地图和。一起减少方法

> 原文：<https://www.freecodecamp.org/news/ruby-using-the-select-map-and-reduce-methods-together-a9b2af30804b/>

迪克兰·米恩

# 通过使用。选择。地图和。一起减少方法

![dfZf53ssAP7xSeSf4xzcCwYnphDNnskwkL9Z](img/26a925169afb0ed51da63fc9885d7bd3.png)

image from kgntechnologies

你绝对不应该在写代码的时候重复自己。换句话说，不要重复自己两次。明确一点——不要写已经解释过的东西。

这被称为同义反复，在编写代码时，应该始终避免这种情况。例如，如果我只用三个强有力的词“永远不要，重复，你自己”，而不是阅读这篇冗长的段落，不是很好吗？

这就是我要向你们展示的如何使用 Ruby 的。选择。地图和。减少(或。注入)方法。

### 例子

让我们假设您正在查看一个充满员工姓名、职务和工资的字典。让我们假设您想要找出公司在开发人员工资上花费的总额。现在，如果不使用 Ruby 中的单一方法，您很可能会写出类似这样的代码:

```
people = [
  {
    first_name: "Gary", 
    job_title: "car enthusiast", 
    salary: "14000" 
  },  
  {
    first_name: "Claire", 
    job_title: "developer", 
    salary: "15000"
  },  
  {
    first_name: "Clem", 
    job_title: "developer", 
    salary: "12000"
  }
]
person1 = people[0][:job_title]
person2 = people[1][:job_title]
person3 = people[2][:job_title]
total = 0
if person1 == "developer"
    total += people[0][:salary].to_i
end
if person2 == "developer"
    total += people[1][:salary].to_i
end
if person3 == "developer"
    total += people[2][:salary].to_i
end
puts total
```

哇——只找到三个人就要写很多行。想象一下，如果该公司雇佣了数百人！

现在，如果您对循环有所了解，那么下一个最简单的步骤就是编写一个 each 方法，将所有的薪水放在一起。这可能只占五或六行，但看看这个！

```
puts people.select{|x| x[:job_title] == "developer"}.map{|y| y[:salary].to_i}.reduce(:+)
```

![CVi6LVbCoOsskAzyQdVJGQ2GzBZq68XOExMJ](img/869c96bd0a4af9858a221e00f7b43016.png)

It may look confusing but let's break it down into pieces.

你会注意到每个方法都以一个花括号开始和结束。如果是一个单独的行块，这可以代替 do 和 end 命令。

```
{} == (do end) #for single-line blocks only
```

### 。挑选

让我们从。选择方法。我们创建一个变量(x)并迭代 people 数组中的每个方法。然后用布尔表达式检查(:job_title)的关键字是否等于“developer”字符串。如果 boolean 返回 true，那么 select 方法将返回 true 的散列放入一个新对象中。

### 。地图

map 方法用于创建一个新数组，该数组不会影响它正在循环的数组。我用这个方法创建了一个新的变量(y)，然后用这个变量来抓取键的值(:salary)。最后，我把这个值从一个字符串转换成一个整数。

### 。减少

这个可能是看起来最混乱的，所以让我们扩展一下。

```
.reduce(0){|sum, indv| sum + indv} #is the same as .reduce(:+)
```

reduce 方法创建一个新变量，您在第一个括号(0)中将该变量的值设置为等于。然后，您创建两个新值(sum 和 indv ),其中一个是您添加个人薪金的总和。

我希望这能很好地解释它！如果你有任何问题，请让我知道。