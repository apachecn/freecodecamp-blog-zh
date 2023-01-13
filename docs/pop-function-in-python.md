# Python 中的 Pop 函数

> 原文：<https://www.freecodecamp.org/news/pop-function-in-python/>

# pop 功能是什么？

pop()方法移除并返回列表中的最后一个元素。有一个可选参数，它是要从列表中删除的元素的索引。如果没有指定索引，a.pop()将移除并返回列表中的最后一项。如果传递给 pop()方法的索引不在范围内，则抛出 IndexError: pop index out of range 异常。

### 用法示例

```
cities = ['New York', 'Dallas', 'San Antonio', 'Houston', 'San Francisco'];

print "City popped is : ", cities.pop()
print "City at index 2 is  : ", cities.pop(2)
```

#### **输出**

```
City popped is :  San Francisco
City at index 2 is  :  San Antonio
```

### 基本堆栈功能

在 Python 应用程序中，`pop()`方法通常与`append()`结合使用来实现基本的堆栈功能。

```
stack = []

for i in range(5):
    stack.append(i)

while len(stack):
    print(stack.pop())
```

#### **更多信息:**

`pop()`的官方文档可以在[这里](https://docs.python.org/3.6/tutorial/datastructures.html)找到