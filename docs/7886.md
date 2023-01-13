# 您应该如何以及为什么使用 Python 生成器

> 原文：<https://www.freecodecamp.org/news/how-and-why-you-should-use-python-generators-f6fb56650888/>

by Radu Raicea

# 您应该如何以及为什么使用 Python 生成器

![9XoRexf1834eq75C2IKDayve0Fdf9ule4MF3](img/31885726d1300dca2e9980b0c0a5a9c1.png)

Image Credit: [Beat Health Recruitment](https://beathealth.com.au/wp-content/uploads/2015/11/bigstock-Robotic-arm-concept-75270622.jpg)

自从与 [PEP 255](https://www.python.org/dev/peps/pep-0255/) 一起引入以来，生成器一直是 Python 的重要组成部分。

生成器函数允许你声明一个行为类似迭代器的函数。

它们允许程序员以快速、简单和干净的方式创建迭代器。

你可能会问，迭代器是什么？

一个 [**迭代器**](https://en.wikipedia.org/wiki/Iterator) 是一个可以被迭代(循环)的对象。它用于抽象数据容器，使其行为像一个可迭代的对象。您可能每天都在使用一些可迭代的对象:字符串、列表和字典等等。

迭代器由实现 [**迭代器协议**](https://docs.python.org/3/c-api/iter.html) 的类定义。这个协议在类中寻找两个方法:`__iter__`和`__next__`。

哇，退后。为什么你会想做迭代器呢？

#### 节省存储空间

迭代器在实例化时不会计算每一项的值。他们只在你要求的时候计算。这就是所谓的[懒评](https://en.wikipedia.org/wiki/Lazy_evaluation)。

当您有一个非常大的数据集要计算时，惰性求值非常有用。它允许您在计算整个数据集时立即开始使用数据。

假设我们想得到所有小于最大数的质数。

我们首先定义检查一个数是否是质数的函数:

```
def check_prime(number):    for divisor in range(2, int(number ** 0.5) + 1):        if number % divisor == 0:            return False    return True
```

然后，我们定义迭代器类，它将包含`__iter__`和`__next__`方法:

```
class Primes:    def __init__(self, max):        self.max = max        self.number = 1
```

```
 def __iter__(self):        return self
```

```
 def __next__(self):        self.number += 1        if self.number >= self.max:            raise StopIteration        elif check_prime(self.number):            return self.number        else:            return self.__next__()
```

`Primes`用最大值实例化。如果下一个素数大于或等于`max`，迭代器将引发一个`StopIteration`异常，结束迭代器。

当我们请求迭代器中的下一个元素时，它会将`number`加 1，并检查它是否是一个质数。如果不是，它会再次调用`__next__`，直到`number`变成质数。一旦是，迭代器返回数字。

通过使用迭代器，我们不会在内存中创建一个质数列表。相反，每次我们请求它时，我们都在生成下一个质数。

让我们试一试:

```
primes = Primes(100000000000)
```

```
print(primes)
```

```
for x in primes:    print(x)
```

```
---------
```

```
<__main__.Primes object at 0x1021834a8>235711...
```

`Primes`对象的每次迭代都会调用`__next__`来生成下一个素数。

**迭代器只能迭代一次。**如果你再次尝试迭代`primes`，将不会返回任何值。它将表现得像一个空列表。

现在我们知道了什么是迭代器以及如何构造迭代器，我们将继续讨论生成器。

### 发电机

回想一下，生成器函数允许我们以更简单的方式创建迭代器。

生成器向 Python 引入了`yield`语句。它的工作有点像`return`，因为它返回一个值。

不同的是**它保存了函数的状态**。下一次调用该函数时，从**停止**的地方继续执行，而**的变量值**与让步前相同。

如果我们将我们的`Primes`迭代器转换成一个生成器，它将看起来像这样:

```
def Primes(max):    number = 1    while number < max:        number += 1        if check_prime(number):            yield number
```

```
primes = Primes(100000000000)
```

```
print(primes)
```

```
for x in primes:    print(x)
```

```
---------
```

```
<generator object Primes at 0x10214de08>235711...
```

这才是真正的蟒蛇皮！我们能做得更好吗？

是啊！我们可以使用**生成器表达式**，用 [PEP 289](https://www.python.org/dev/peps/pep-0289/) 引入。

这是生成器的列表理解等价物。它与列表理解的工作方式完全相同，但是表达式用`()`包围，而不是用`[]`包围。

下面的表达式可以代替我们上面的生成器函数:

```
primes = (i for i in range(2, 100000000000) if check_prime(i))
```

```
print(primes)
```

```
for x in primes:    print(x)
```

```
---------
```

```
<generator object <genexpr> at 0x101868e08>235711...
```

这就是 Python 中生成器的妙处。

### 总之…

*   生成器允许您以非常 pythonic 化的方式创建迭代器。
*   迭代器允许惰性求值，只在被请求时生成可迭代对象的下一个元素。这对于非常大的数据集非常有用。
*   迭代器和生成器只能迭代一次。
*   生成器函数比迭代器好。
*   生成器表达式比迭代器好(仅针对简单情况)。

你也可以查看我的[解释](https://medium.freecodecamp.org/how-i-used-python-to-find-interesting-people-on-medium-be9261b924b0)关于我如何使用 Python 在 Medium 上寻找有趣的人。

更多更新，请在 Twitter 上关注我。