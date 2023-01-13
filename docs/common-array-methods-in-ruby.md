# 你应该知道的最常见的 Ruby 数组方法

> 原文：<https://www.freecodecamp.org/news/common-array-methods-in-ruby/>

## **常用数组方法**

Ruby 数组构成了 Ruby 编程的核心基础，事实上也是大多数语言的核心基础。它被用得如此之多，以至于知道甚至记住一些最常用的数组方法将是有益的。如果你想了解更多关于 Ruby 数组的知识，我们有一篇关于它们的文章。

出于本指南的目的，我们的阵列如下:

```
array = [0, 1, 2, 3, 4]
```

#### **。长度**

的。length 方法计算数组中元素的数量并返回计数:

```
array.length
=> 5
```

#### **。第一个**

的。第一个方法访问数组的第一个元素，即索引为 0 的元素:

```
array.first
=> 0
```

#### **。最后一个**

的。last 方法访问数组的最后一个元素:

```
array.last
=> 4
```

#### **。乘坐**

的。take 方法返回数组的前 n 个元素:

```
array.take(3)
=> [0, 1, 2]
```

#### **。掉落**

的。drop 方法返回数组中 n 个元素之后的元素:

```
array.drop(3)
=> [3, 4]
```

#### **数组索引**

您可以通过访问数组中特定元素的索引来访问它。如果数组中不存在索引，将返回 nil:

```
array[2]
=> 2

array[5]
=> nil
```

#### **。爆音**

的。pop 方法将永久删除数组的最后一个元素:

```
array.pop
=> [0, 1, 2, 3]
```

#### **。换档**

的。shift 方法将永久删除数组的第一个元素并返回该元素:

```
array.shift
=> 0  
array
=> [1, 2, 3, 4]
```

#### **。按下**

的。push 方法将允许您在数组末尾添加一个元素:

```
array.push(99)
=> [0, 1, 2, 3, 4, 99]
```

#### **。未换档**

的。unshift 方法将允许您在数组的开头添加一个元素:

```
array = [2, 3]
array.unshift(1)
=> [1, 2, 3]
```

#### **。删除**

的。delete 方法从数组中永久移除指定的元素:

```
array.delete(1)
=> [0, 2, 3, 4]
```

#### **。删除 _ 在**

的。delete_at 方法允许您永久移除指定索引处的数组元素:

```
array.delete_at(0)
=> [1, 2, 3, 4]
```

#### **。反向**

的。reverse 方法反转数组，但不改变它(原始数组保持原样):

```
array.reverse
=> [4, 3, 2, 1, 0]
```

#### **。选择**

的。select 方法循环访问一个数组，并返回一个新数组，该数组包含为所提供的表达式返回 true 的所有项。

```
array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
array.select { |number| number > 4 }
=> [5, 6, 7, 8, 9, 10]
array
=> [5, 6, 7, 8, 9, 10]
```

#### **。包括？**

包括什么？方法检查给定的参数是否包含在数组中:

```
array = [1, 2, 3, 4, 5]
=> [1, 2, 3, 4, 5]
array.include?(3)
=> true

#### .flatten
The flatten method can be used to take an array that contains nested arrays and create a one-dimensional array:

``` ruby
array = [1, 2, [3, 4, 5], [6, 7]]
array.flatten
=> [1, 2, 3, 4, 5, 6, 7]
```

#### **。加入**

的。join 方法返回由分隔符参数分隔的数组所有元素的字符串。如果 separator 参数为 nil，则该方法使用空字符串作为字符串之间的分隔符。

```
array.join
=> "1234"
array.join("*")
=> "1*2*3*4"
```

#### **。每个**

的。每个方法迭代数组的每个元素，允许您对它们执行操作。

```
array.each do |element|
  puts element
end
=> 
0
1
2
3
4
```

#### **。地图**

的。映射方法与。收集方法。的。地图和。collect 方法迭代数组的每个元素，允许您对它们执行操作。的。地图和。收集方法不同于。每个方法都返回一个包含已转换元素的数组。

```
array.map { |element| element * 2 }
  puts element
end
=> 
0
2
4
6
8
```

#### **。唯一的**

的。uniq 方法接受一个包含重复元素的数组，并返回一个只包含唯一元素的数组副本，所有重复元素都从数组中删除。

```
array = [1, 1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 5, 6, 7, 8]
array.uniq
=> [1, 2, 3, 4, 5, 6, 7, 8]
```

#### **。串联**

的。concat 方法将数组中的元素追加到原始数组中。的。concat 方法可以接受多个数组作为参数，然后将多个数组追加到原始数组中。

```
array = [0, 1, 2, 3, 4]
array.concat([5, 6, 7], [8, 9, 10])
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

## **更多信息**

*   [Ruby 数组文档](http://ruby-doc.org/core-2.5.1/Array.html)
*   [你需要知道的六种 Ruby 数组方法](https://www.freecodecamp.org/news/six-ruby-array-methods-you-need-to-know-5f81c1e268ce/)
*   [学习 Ruby——从零到英雄](https://www.freecodecamp.org/news/learning-ruby-from-zero-to-hero-90ad4eecc82d/)