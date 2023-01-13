# Ruby 中的数据类型——True、False 和 Nil 用例子解释

> 原文：<https://www.freecodecamp.org/news/data-types-in-ruby-true-false-and-nil-explained-with-examples/>

`true`、`false`和`nil`是 Ruby 中特殊的内置数据类型。这些关键字中的每一个都计算为一个对象，该对象是其各自类的唯一实例。

```
true.class
 => TrueClass
false.class
 => FalseClass
nil.class
 => NilClass
```

`true`和`false`是 Ruby 的原生布尔值。布尔值是只能是两种可能值之一的值:true 或 not true。物体`true`代表真理，而`false`代表相反。您可以将变量赋值给`true` / `false`，将它们传递给方法，并像使用其他对象一样使用它们(比如数字、字符串、数组、散列)。

`nil`是一个特殊值，表示没有值——这是 Ruby 指代“无”的方式。遇到`nil`对象的一个例子是当你要求不存在或找不到的东西时:

```
hats = ["beret", "sombrero", "beanie", "fez", "flatcap"]

hats[0]
 => "beret" # the hat at index 0
hats[2]
 => "beanie" # the hat at index 2
hats[4]
 => "flatcap" # the hat at index 4
hats[5]
 => nil # there is no hat at index 5, index 5 holds nothing (nil)
```

零不是什么都没有(它是一个数字，是一些东西)。同样，空字符串、数组和散列也不是什么都没有(它们是对象，只是碰巧是空的)。你可以调用方法`nil?`来检查一个对象是否为零。

```
0.nil?
 => false
"".nil?
 => false
[].nil?
 => false
{}.nil?
 => false
nil.nil?
 => true
 # from the example above
hats[5].nil?
 => true
```

Ruby 中的每个对象都有一个布尔值，这意味着它在布尔上下文中要么被认为是真，要么被认为是假。在这种情况下，被认为是真的是“真的”，被认为是假的是“假的”在 Ruby 中，*只有* `false`和`nil`是“假的”，其他的都是“真的”

## 更多信息:

*   [学习 Ruby:从零到英雄](https://www.freecodecamp.org/news/learning-ruby-from-zero-to-hero-90ad4eecc82d/)
*   [惯用的 Ruby:编写漂亮的代码](https://www.freecodecamp.org/news/idiomatic-ruby-writing-beautiful-code-6845c830c664/)
*   [如何使用简单的 Ruby 脚本将数据库表导出为 CSV 格式](https://www.freecodecamp.org/news/export-a-database-table-to-csv-using-a-simple-ruby-script-2/)