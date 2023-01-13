# 算术序列的递归公式

> 原文：<https://www.freecodecamp.org/news/recursive-formulas-for-arithmetic-sequences/>

### **什么是等差数列？**

****序列**** 是对一个数字进行相同操作以得到下一个数字的数字列表。 ****等差数列**** 特指通过加减一个值构造的序列——称为 ****公差带**—**得到下一项。

为了有效地讨论一个序列，我们使用一个公式，当一个索引列表被放入时，这个公式构建这个序列。通常，这些公式的名称都是一个字母，后面跟一个括号中的参数，右边是构建序列的表达式。

`a(n) = n + 1`

上面是一个等差数列公式的例子。

### **例题**

顺序:1，2，3，4，… |公式:a(n) = n + 13

顺序:8，13，18，… |公式:b(n) = 5n - 2

### **一个递归公式**

注:数学家从 1 开始计数，所以按照惯例，`n=1`是第一项。所以我们必须定义第一项是什么。然后，我们必须找出并包括共同的差异。

再次看一下例子，

序列:1，2，3，4，… |公式:a(n) = n + 1 |递归公式:a(n) = a(n-1) + 1，a(1) = 1

序列:3，8，13，18，…|公式:b(n) = 5n - 2 |递归公式:b(n) = b(n-1) + 5，b(1) = 3

### **求公式(给定一个有第一项的数列)**

```
1\. Figure out the common difference
    Pick a term in the sequence and subtract the term that comes before it.         
2\. Construct the formula
    The formula has the form: `a(n) = a(n-1) + [common difference], a(1) = [first term]`
```

### **求公式(给定一个没有第一项的数列)**

```
1\. Figure out the common difference
    Pick a term in the sequence and subtract the term that comes before it. 
2\. Find the first term
    i. Pick a term in the sequence, call it `k` and call its index `h`
    ii. first term = k - (h-1)*(common difference)
3\. Construct the formula
    The formula has the form: `a(n) = a(n-1) + [common difference], a(1) = [first term]` 
```

#### **更多信息:**

有关该主题的更多信息，请访问

*   [维基百科](https://en.wikipedia.org/wiki/Arithmetic_progression)