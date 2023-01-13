# 使用示例循环语法在 C++中执行 While 循环

> 原文：<https://www.freecodecamp.org/news/do-while-loops-in-c-plus-plus-example-loop-syntax/>

循环是控制流语句，允许代码基于给定的条件重复执行。

`do while`循环是`while`循环的变体，它在检查条件之前执行一次代码块。那么只要条件为真，它就会重复循环。

## 句法

以下是 do while 循环的基本语法:

```
do {
    // body of the loop
}
while (condition);
```

注意，终止条件的测试是在每次循环执行后进行的。这意味着循环将总是至少执行一次，即使开始时条件为假。

这与普通的`while`循环形成对比，在普通的循环中，条件在循环之前被测试，并且不保证代码块的执行。

下面是一个常规的 while 循环:

```
while (condition) {
    // body of the loop
 }
```

## do while 循环的示例

让我们看一个工作示例:

```
#include <iostream>

int main () {

    int number = 1;

    do {
        std::cout << number << std::endl;
        number++;
    }
    while (number < 5);

    return 0;
}
```

输出:

```
1
2
3
4
```

在这个例子中，我们初始化一个整数变量`number = 1`。然后，我们重复执行循环。

在循环内部，我们打印变量并将变量加 1。只要`number`小于 5，就执行循环。因此，从 1 到 4 的数字被打印出来。

## 示例 2

下面是另一个例子及其输出:

```
10
```

```
#include <iostream>

int main () {

    int number = 10;

    do {
        std::cout << number << std::endl;
        number++;
    }
    while (number < 5);

    return 0;
}
```

在这个例子中，我们使用与第一个例子相同的代码。但是现在我们用`number = 10`初始化变量。

既然 10 不小于 5，我们的条件就已经是假的了。该循环仍将执行一次，10 是唯一的打印输出。

## 什么时候应该使用 Do While 循环？

如果您需要重复执行代码，那么`do while`循环是一个很好的工具。如上所述，无论何时需要循环，您都希望使用这种语法，并且您还需要保证至少执行一次代码块。

想象一些像例子 2 中的代码，但是我们没有用硬编码的值初始化变量。相反，我们使用用户输入。

我们不能保证用户输入足够小，但是我们仍然希望在输出控制台中看到至少一条打印语句。这是`do while`循环的完美用例。

```
// Pseudo code where do while is useful:

int number = getUserInput();

do {
    std::cout << number << std::endl;
    number = someUpdateCalculation();
}
while (number < 5);
```