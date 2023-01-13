# Python 字典数据结构解释

> 原文：<https://www.freecodecamp.org/news/python-dictionary-data-structure-explained/>

字典是 Python 中最常用的数据结构之一。字典是条目的无序集合，我们通常将键和值存储在字典中。让我们来看几个例子，看看字典通常是如何使用的。

```
# dictionary declaration 1
dict1 = dict()

# dictionary declaration 2
dict2 = {}

# Add items to the dictionary
# The syntax to add and retrieve items is same for either of the two objects we defined above. 
key = "X"
value = "Y"
dict1[key] = value

# The dictionary doesn't have any specific data-type. 
# So, the values can be pretty diverse. 
dict1[key] = dict2
```

现在让我们看看一些检索方法。

```
# Since "X" exists in our dictionary, this will retrieve the value
value = dict1[key]

# This key doesn't exist in the dictionary. 
# So, we will get a `KeyError`
value = dict1["random"]
```

### **避免按键错误:使用。获取功能**

如果字典中不存在给定的键，Python 将抛出一个`KeyError`。对此有一个简单的解决方法。让我们来看看如何避免`KeyError`使用内置的`.get`函数用于字典。

```
dict_ = {}

# Some random key
random_key = "random"

# The most basic way of doing this is to check if the key 
# exists in the dictionary or not and only retrieve if the 
# key exists. Otherwise not. 
if random_key in dict_:
  print(dict_[random_key])
else:
  print("Key = {} doesn't exist in the dictionary".format(dict_))
```

很多时候，当键不存在时，我们可以得到一个默认值。例如当建造柜台时。在缺少键的情况下，有一种更好的方法从字典中获取默认值，而不是依赖于标准的`if-else`。

```
# Let's say we want to build a frequency counter for items in the following array
arr = [1,2,3,1,2,3,4,1,2,1,4,1,2,3,1]

freq = {}

for item in arr:
  # Fetch a value of 0 in case the key doesn't exist. Otherwise, fetch the stored value
  freq[item] = freq.get(item, 0) + 1
```

因此，`get(<key>, <defaultval>)`是从字典中检索任何给定键的默认值的一个方便的操作。当我们想要将可变数据结构作为值来处理时，例如`list`或`set`，这种方法就会出现问题。

```
dict_ = {}

# Some random key
random_key = "random"

dict_[random_key] = dict_.get(random_key, []).append("Hello World!")
print(dict_) # {'random': None}

dict_ = {}
dict_[random_key] = dict_.get(random_key, set()).add("Hello World!")
print(dict_) # {'random': None}
```

你看到问题了吗？

新的`set`或`list`不会被分配给字典的键。如果缺少值，我们应该给键分配一个新的`list`或`set`，然后分别分配`append`或`add`。让我们来看一个例子。

```
dict_ = {}
dict_[random_key] = dict_.get(random_key, set())
dict_[random_key].add("Hello World!")
print(dict_) # {'random': set(['Hello World!'])}. Yay!
```

### **避免按键错误:使用默认字典**

这在大多数情况下是有效的。然而，有一种更好的方法可以做到这一点。一种更`pythonic`的方式。`defaultdict`是内置 dict 类的子类。在缺少键的情况下,`defaultdict`简单地分配我们指定的缺省值。所以，这两步:

```
dict_[random_key] = dict_.get(random_key, set())
dict_[random_key].add("Hello World!")
```

现在可以合并成一个单一的步骤。例如

```
from collections import defaultdict

# Yet another random key
random_key = "random_key"

# list defaultdict
list_dict_ = defaultdict(list)

# set defaultdict
set_dict_ = defaultdict(set)

# integer defaultdict
int_dict_ = defaultdict(int)

list_dict_[random_key].append("Hello World!")
set_dict_[random_key].add("Hello World!")
int_dict_[random_key] += 1

"""
  defaultdict(<class 'list'>, {'random_key': ['Hello World!']}) 
  defaultdict(<class 'set'>, {'random_key': {'Hello World!'}}) 
  defaultdict(<class 'int'>, {'random_key': 1})
"""
print(list_dict_, set_dict_, int_dict_)
```

[正式文件](https://docs.python.org/2/library/collections.html)