# Ruby 数字方法和数字运算(附示例)

> 原文：<https://www.freecodecamp.org/news/ruby-number-methods-integer-and-float-methods-with-examples/>

## Ruby 中的数字方法

Ruby 提供了多种您可以在 numbers 上使用的内置方法。以下是[整数](https://ruby-doc.org/core-2.2.0/Integer.html)和[浮点](https://ruby-doc.org/core-2.2.0/Float.html#method-i-ceil)方法的不完整列表。

### [甚至](https://ruby-doc.org/core-2.2.0/Integer.html#method-i-even-3F):

使用`.even?`检查一个 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 是否为偶数。返回一个`true`或`false` ****布尔**** 。

```
 15.even? #=> false
    4.even?  #=> true
```

### [奇数](https://ruby-doc.org/core-2.2.0/Integer.html#method-i-odd-3F):

使用`.odd?`检查一个 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 是否为奇数。返回一个`true`或`false` ****布尔**** 。

```
 15.odd? #=> true
    4.odd?  #=> false
```

### [天花板](https://ruby-doc.org/core-2.2.0/Float.html#method-i-ceil):

`.ceil`方法将 [****浮点数****](https://ruby-doc.org/core-2.2.0/Float.html#method-i-ceil) ****向上**** 舍入到最接近的数字。返回一个 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 。

```
 8.3.ceil #=> 9
    6.7.ceil #=> 7
```

### [楼层](https://ruby-doc.org/core-2.2.0/Float.html#method-i-floor):

`.floor`方法将 [****浮点数****](https://ruby-doc.org/core-2.2.0/Float.html#method-i-ceil) ****向下**** 舍入到最接近的数字。返回一个 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 。

```
 8.3.floor #=> 8
    6.7.floor #=> 6
```

### [下一个](https://ruby-doc.org/core-2.2.0/Integer.html#method-i-next):

使用`.next`返回下一个连续的 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 。

```
 15.next #=> 16
    2.next  #=> 3
    -4.next #=> -3
```

### [Pred](https://ruby-doc.org/core-1.8.7/Integer.html#method-i-pred) :

使用`.pred`返回上一个连续的 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 。

```
 15.pred #=> 14
    2.pred  #=> 1
    (-4).pred #=> -5
```

### [到串](https://ruby-doc.org/core-2.4.2/Object.html#method-i-to_s):

用`.to_s`对一个数( [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) ， [****浮点数****](https://ruby-doc.org/core-2.2.0/Float.html#method-i-ceil) 等。)返回该数字的[字符串](https://ruby-doc.org/core-2.2.0/String.html)。

```
 15.to_s  #=> "15"
    3.4.to_s #=> "3.4"
```

### [最大公约数](https://ruby-doc.org/core-2.2.0/Integer.html#method-i-gcd):

`.gcd`方法提供两个数的最大公约数(总是正的)。返回一个 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 。

```
 15.gcd(5) #=> 5
    3.gcd(-7) #=> 1
```

### [回合](http://ruby-doc.org/core-2.2.0/Integer.html#method-i-round):

使用`.round`返回一个四舍五入的 [****整数****](https://ruby-doc.org/core-2.2.0/Integer.html) 或 [****浮点数****](https://ruby-doc.org/core-2.2.0/Float.html) 。

```
 1.round        #=> 1
    1.round(2)     #=> 1.0
    15.round(-1)   #=> 20
```

### [次](http://ruby-doc.org/core-2.2.0/Integer.html#method-i-times):

使用`.times`迭代给定块`int`次。

```
 5.times do |i|
      print i, " "
    end
    #=> 0 1 2 3 4
```

## Ruby 中的数学运算

在 Ruby 中，你可以对数字执行所有标准的数学运算，包括:加法`+`、减法`-`、乘法`*`、除法`/`、求余数`%`，以及处理指数`**`。

### 添加:

可以使用`+`运算符将数字相加。

```
15 + 25 #=> 40
```

### 减法:

使用`-`运算符可以将数字相减。

```
25 - 15 #=> 10
```

### 乘法:

可以使用`*`运算符将数字相乘。

```
10 * 5 #=> 50
```

### 部门:

使用`/`运算符可以将数字相除。

```
10 / 5 #=> 2
```

### 剩余订单:

余数可以使用模数`%`运算符找到。

```
10 % 3 #=> 1 # because the remainder of 10/3 is 1
```

### 指数:

可以使用`**`运算符计算指数。

```
2 ** 3 #=> 8 # because 2 to the third power, or 2 * 2 * 2 = 8
```