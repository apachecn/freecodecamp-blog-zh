# Python 返回多个值——如何返回一个元组、列表或字典

> 原文：<https://www.freecodecamp.org/news/python-returns-multiple-values-how-to-return-a-tuple-list-dictionary/>

在 Python 中，可以从一个函数返回多个值。

为此，返回一个包含多个值的数据结构，比如包含每周跑步英里数的列表。

```
def miles_to_run(minimum_miles):
   week_1 = minimum_miles + 2
   week_2 = minimum_miles + 4
   week_3 = minimum_miles + 6
   return [week_1, week_2, week_3]

print(miles_to_run(2))
# result: [4, 6, 8] 
```

Python 中的数据结构用于存储数据集合，这些数据可以从函数中返回。在本文中，我们将探索如何从这些数据结构中返回多个值:元组、列表和字典。

## 元组

元组是有序的、不可变的序列。也就是说，一个元组*不能*改变。

例如，使用元组来存储关于一个人的信息:他们的姓名、年龄和位置。

```
nancy = ("nancy", 55, "chicago") 
```

下面是如何编写一个返回元组的函数。

```
def person():
   return "bob", 32, "boston"

print(person())
# result: ('bob', 32, 'boston') 
```

注意，我们在 return 语句中没有使用括号。这是因为您可以通过用逗号分隔每个项目来返回一个元组，如上例所示。

“实际上是逗号组成了元组，而不是括号，”[文档](https://docs.python.org/3/library/stdtypes.html#tuple)指出。然而，[括号*是*必需的](https://docs.python.org/3/library/stdtypes.html#tuple)，带有空元组或者是为了避免混淆。

下面是一个使用括号`()`返回元组的函数的例子。

```
def person(name, age):
   return (name, age)
print(person("henry", 5))
#result: ('henry', 5) 
```

## 目录

列表是有序的、可变的序列。这意味着，列表*可以*改变。

您可以使用列表来存储城市:

```
cities = ["Boston", "Chicago", "Jacksonville"] 
```

或者考试成绩:

```
test_scores = [55, 99, 100, 68, 85, 78] 
```

看看下面的函数。它返回一个包含十个数字的列表。

```
def ten_numbers():
   numbers = []
   for i in range(1, 11):
       numbers.append(i)
   return numbers

print(ten_numbers())
#result: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 
```

这是另一个例子。这一次我们在调用函数时传递几个参数。

```
def miles_ran(week_1, week_2, week_3, week_4):
   return [week_1, week_2, week_3, week_4]

monthly_mileage = miles_ran(25, 30, 28, 40)
print(monthly_mileage)
#result: [25, 30, 28, 40] 
```

很容易混淆元组和列表。毕竟两者都是存储对象的容器。但是，请记住以下主要区别:

*   元组不能变。
*   列表可以改变。

## 字典

字典包含用花括号 **`{}`** 括起来的键值对。每个“键”都有一个相关的“值”

考虑下面的员工字典。每个员工的名字是一把“钥匙”，他们的职位是“价值”

```
employees = {
   "jack": "engineer",
   "mary": "manager",
   "henry": "writer",
} 
```

下面是如何编写一个函数来返回一个带有键、值对的字典。

```
def city_country(city, country):
   location = {}
   location[city] = country
   return location

favorite_location = city_country("Boston", "United States")
print(favorite_location)
# result: {'Boston': 'United States'} 
```

在上面的例子中，“Boston”是**键**，而“美国”是**值**。

我们已经走过了很多地方。关键点是:可以从一个 Python 函数返回多个值，有几种方法可以做到这一点。

我写了你需要发展的编程技能和你需要学习的概念，以及在 amymhaddad.com 学习它们的最好方法。