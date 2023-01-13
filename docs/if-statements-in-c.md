# 如果...解释了 C 语言中的 Else 语句

> 原文：<https://www.freecodecamp.org/news/if-statements-in-c/>

条件代码流是基于特定条件改变一段代码行为方式的能力。在这种情况下，你可以使用`if`语句。

`if`语句也称为决策语句，因为它根据给定的条件或表达式做出决策。如果条件评估为真，则执行`if`语句中的代码块。然而，如果条件评估为 false，则跳过花括号内的代码，并执行`if`语句之后的代码。

### `if`语句的语法

```
if (testCondition) {
   // statements
}
```

### 简单的例子

让我们来看一个实际例子:

```
#include <stdio.h>
#include <stdbool.h>

int main(void) {
    if(true) {
        printf("Statement is True!\n");
    }

    return 0;
}
```

**输出:**

```
Statement is True!
```

如果`if`语句括号内的代码为真，则执行花括号内的所有内容。在这种情况下，`true`的计算结果为真，因此代码运行`printf`函数。

### `if..else`报表

在`if...else`语句中，如果`if`语句括号中的代码为真，则执行括号中的代码。但是如果括号内的语句为假，则执行`else`语句括号内的所有代码。

当然，上面的例子在这种情况下不是很有用，因为`true`总是计算为真。这是另一个更实际的例子:

```
#include <stdio.h>

int main(void) {
    int n = 2;

    if(n == 3) { // comparing n with 3
        printf("Statement is True!\n");
    } 
    else { // if the first condition is not true, come to this block of code
        printf("Statement is False!\n");
    }

    return 0;
}
```

**输出:**

```
Statement is False!
```

这里有几个重要的区别。第一，`stdbool.h`还没收录。这没关系，因为`true`和`false`没有像第一个例子中那样被使用。在 C 语言中，像在其他编程语言中一样，你可以使用评估为真或假的语句，而不是直接使用布尔值`true`或`false`。

还要注意`if`语句括号中的条件:`n == 3`。这个条件比较`n`和数字 3。`==`是比较运算符，是 c 语言中几种比较运算之一。

### 嵌套`if...else`

`if...else`语句允许在两种可能性之间做出选择。但有时你需要在三种或更多的可能性中做出选择。

例如，如果参数小于零，数学中的 sign 函数返回-1，如果参数大于零，返回+1，如果参数为零，则返回零。

下面的代码实现了这个函数:

```
if (x < 0)
   sign = -1;
else
   if (x == 0)
      sign = 0;
   else
      sign = 1;
```

如您所见，第二个`if...else`语句嵌套在第一个`if..else`的`else`语句中。

如果`x`小于 0，那么`sign`被设置为-1。但是，如果`x`不小于 0，则执行第二条`if...else`语句。在那里，如果`x`等于 0，`sign`也被设置为 0。但是如果`x`大于 0，则`sign`被设置为 1。

初学者通常使用一串`if`语句，而不是嵌套的`if...else`语句:

```
if (x < 0) {
   sign = -1;
}

if (x == 0) {
   sign = 0;
}

if (x > 0) {
   sign = 1;
}
```

虽然这可行，但不推荐，因为不清楚是否只有一个赋值语句(`sign = ...`)要根据`x`的值来执行。它的效率也很低——每次代码运行时，所有三个条件都会被测试，即使有一两个不是必须的。

### 其他...if 语句

`if...else`语句是一串`if`语句的替代。请考虑以下情况:

```
#include <stdio.h>

int main(void) {
    int n = 5;

    if(n == 5) {
        printf("n is equal to 5!\n");
    } 
    else if (n > 5) {
        printf("n is greater than 5!\n");
    }

    return 0;
}
```

**输出:**

```
n is equal to 5!
```

如果`if`语句的条件评估为假，则检查`else...if`语句的条件。如果该条件评估为真，则运行`else...if`语句花括号内的代码。

### 比较运算符

| 操作员名 | 使用 | 结果 |
| --- | --- | --- |
| 等于 | `a == b` | 如果`a`等于`b`则为真，否则为假 |
| 不等于 | `a != b` | 如果`a`不等于`b`则为真，否则为假 |
| 大于 | `a > b` | 如果`a`大于`b`则为真，否则为假 |
| 大于或等于 | `a >= b` | 如果`a`大于或等于`b`，则为真，否则为假 |
| 不到 | `a < b` | 如果`a`小于`b`则为真，否则为假 |
| 小于或等于 | `a <= b` | 如果`a`小于或等于`b`，则为真，否则为假 |

## **逻辑运算符**

如果某事不为真，或者两件事为真，我们可能需要运行一些代码。为此，我们有逻辑运算符:

| 操作员名 | 使用 | 结果 |
| --- | --- | --- |
| 不(`!`) | `!(a == 3)` | 如果`a`等于**而**不等于 3，则为真 |
| 和(`&&`) | `a == 3 && b == 6` | 如果`a`等于 3 **并且**等于 6，则为真 |
| 或者(`&#124;&#124;`) | `a == 2 &#124;&#124; b == 4` | 如果`a`等于 2 **或**等于 4，则为真 |

例如:

```
#include <stdio.h>

int main(void) {
    int n = 5;
    int m = 10;

    if(n > m || n == 15) {
        printf("Either n is greater than m, or n is equal to 15\n");
    } 
    else if( n == 5 && m == 10 ) {
        printf("n is equal to 5 and m is equal to 10!\n");
    } 
    else if ( !(n == 6)) {
        printf("It is not true that n is equal to 6!\n");
    }
    else if (n > 5) {
        printf("n is greater than 5!\n");
    }

    return 0;
}
```

**输出:**

```
n is equal to 5 and m is equal to 10!
```

### 关于 C 比较的一个重要注意事项

虽然我们之前提到过，每次比较都是检查某个东西是真还是假，但这只是对的一半。c 语言非常轻便，并且离运行它的硬件很近。有了硬件，很容易检查某个东西是 0 还是假，但其他任何东西都要困难得多。

相反，更准确的说法是，比较实际上是检查某个值是否为 0 / false，或者是否为任何其他值。

例如，他的 if 语句是真实有效的:

```
if(12452) {
    printf("This is true!\n")
}
```

按照设计，0 是假的，按照惯例，1 是真的。事实上，这里有一个`stdbool.h`库:

```
#define false   0
#define true    1
```

虽然还有更多，但这是布尔如何工作以及库如何操作的核心。这两行指令编译器将单词`false`替换为 0，将`true`替换为 1。