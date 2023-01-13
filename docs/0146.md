# TypeError:“int”对象不可订阅[已解决 Python 错误]

> 原文：<https://www.freecodecamp.org/news/python-typeerror-int-object-not-subscriptable-solved/>

当您试图将整数视为可下标对象时，会出现 Python 错误“type error:' int ' object is not subscribed”。

在 Python 中，可下标对象是可以“下标”或迭代的对象。

## 为什么会出现“type Error:“int”对象不是可订阅的错误”

您可以迭代字符串、列表、元组甚至字典。但是不可能对一个整数或一组数进行迭代。

所以，如果你得到这个错误，这意味着你试图迭代一个整数，或者你把一个整数当作一个数组。

在下面的例子中，我用 ddmmyy 格式写了出生日期(`dob`变量)。我试图获得出生月份，但没有成功。它抛出错误“type error:“int”对象不可订阅”:

```
dob = 21031999
mob = dob[2:4]

print(mob)

# Output: Traceback (most recent call last):
#   File "int_not_subable..py", line 2, in <module>
#     mob = dob[2:4]
# TypeError: 'int' object is not subscriptable 
```

## 如何修复“type Error:“int”对象不可订阅”错误

若要修复此错误，您需要将整数转换为可迭代的数据类型，例如字符串。

如果你得到错误是因为你把一些东西转换成了一个整数，那么你需要把它变回原来的样子。例如，字符串、元组、列表等等。

在上面抛出错误的代码中，我能够通过将`dob`变量转换成一个字符串来让它工作:

```
dob = "21031999"
mob = dob[2:4]

print(mob)

# Output: 03 
```

如果你在把一些东西转换成整数后得到错误，这意味着你需要把它转换回字符串或者保持原样。

在下面的例子中，我编写了一个 Python 程序，以 ddmmyy 格式打印出生日期。但是它会返回一个错误:

```
name = input("What is your name? ")
dob = int(input("What is your date of birth in the ddmmyy order? "))
dd = dob[0:2]
mm = dob[2:4]
yy = dob[4:]
print(f"Hi, {name}, \nYour date of birth is {dd} \nMonth of birth is {mm} \nAnd year of birth is {yy}.")

#Output: What is your name? John Doe
# What is your date of birth in the ddmmyy order? 01011970
# Traceback (most recent call last):
#   File "int_not_subable.py", line 12, in <module>
#     dd = dob[0:2]
# TypeError: 'int' object is not subscriptable 
```

浏览代码时，我记得 input 返回一个字符串，所以我不需要将用户的出生日期输入的结果转换为整数。这修复了错误:

```
name = input("What is your name? ")
dob = input("What is your date of birth in the ddmmyy order? ")
dd = dob[0:2]
mm = dob[2:4]
yy = dob[4:]
print(f"Hi, {name}, \nYour date of birth is {dd} \nMonth of birth is {mm} \nAnd year of birth is {yy}.")

#Output: What is your name? John Doe
# What is your date of birth in the ddmmyy order? 01011970
# Hi, John Doe,
# Your date of birth is 01
# Month of birth is 01
# And year of birth is 1970. 
```

## 结论

在本文中，您了解了 Python 中“type error:' int ' object is not subscribed”错误的原因以及如何修复它。

如果你得到这个错误，这意味着你把一个整数当作可迭代的数据。整数是不可迭代的，因此您需要使用不同的数据类型或将整数转换为可迭代的数据类型。

如果错误的发生是因为你把一些东西转换成了一个整数，那么你需要把它变回那个可迭代的数据类型。

感谢您的阅读。