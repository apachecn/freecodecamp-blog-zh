# Python 中的 Lambda 表达式

> 原文：<https://www.freecodecamp.org/news/lambda-expressions-in-python/>

## **λ表达式**

当我们需要做一些简单的事情，并且对快速完成工作比正式命名函数更感兴趣时，Lambda 表达式是理想的选择。Lambda 表达式也称为匿名函数。

Python 中的 Lambda 表达式是声明小型匿名函数的一种快捷方式(不需要为 lambda 函数提供名称)。

Lambda 函数的行为就像用关键字`def`声明的常规函数一样。当你想用简洁的方式定义一个小函数时，它们就派上了用场。它们只能包含一个表达式，因此不太适合包含控制流语句的函数。

### Lambda 函数的语法

`lambda arguments: expression`

Lambda 函数可以有任意数量的参数，但只能有一个表达式。

### 示例代码

```
# Lambda function to calculate square of a number
square = lambda x: x ** 2
print(square(3)) # Output: 9

# Traditional function to calculate square of a number
def square1(num):
  return num ** 2
print(square(5)) # Output: 25
```

在上面的 lambda 示例中，`lambda x: x ** 2`产生了一个匿名函数对象，它可以与任何名称相关联。因此，我们将函数对象与`square`相关联。所以从现在开始我们可以像调用任何传统函数一样调用`square`对象，例如`square(10)`

## **λ函数的例子**

### **初学者**

```
lambda_func = lambda x: x**2 # Function that takes an integer and returns its square
lambda_func(3) # Returns 9
```

### **中级**

```
lambda_func = lambda x: True if x**2 >= 10 else False
lambda_func(3) # Returns False
lambda_func(4) # Returns True
```

### **复杂**

```
my_dict = {"A": 1, "B": 2, "C": 3}
sorted(my_dict, key=lambda x: my_dict[x]%3) # Returns ['C', 'A', 'B']
```

## 使用案例

假设您想从一个`list`中过滤出奇数。您可以使用一个`for`循环:

```
my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
filtered = []

for num in my_list:
     if num % 2 != 0:
         filtered.append(num)

print(filtered)      # Python 2: print filtered
# [1, 3, 5, 7, 9]
```

或者你可以把它写成一个包含列表理解的命令行:

```
filtered = [x for x in [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] if x % 2 != 0]
```

但是您可能会尝试使用内置的`filter`函数。为什么？第一个例子有点太冗长，一行程序可能更难理解。但是`filter`提供了这两个词中最好的。更重要的是，内置函数通常更快。

```
my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

filtered = filter(lambda x: x % 2 != 0, my_list)

list(filtered)
# [1, 3, 5, 7, 9]
```

注意:在 Python 3 中，内置函数返回生成器对象，所以必须调用`list`。另一方面，在 Python 2 中，它们返回一个`list`、`tuple`或`string`。

发生了什么事？您告诉`filter`获取`my_list`中的每个元素并应用 lambda 表达式。过滤掉返回`False`的值。

#### **更多信息:**

*   [正式文件](https://docs.python.org/3/reference/expressions.html#lambda)