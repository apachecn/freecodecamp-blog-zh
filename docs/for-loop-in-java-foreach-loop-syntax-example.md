# Java 中的 For 循环+ forEach 循环语法示例

> 原文：<https://www.freecodecamp.org/news/for-loop-in-java-foreach-loop-syntax-example/>

编程中的循环是连续运行的指令序列，直到满足某个条件。

在本文中，我们将学习 Java 中的`for`和`forEach`循环。

## Java 中`for`循环的语法

下面是创建`for`循环的语法:

```
for (initialization; condition; increment/decrement) {
   // code to be executed
}
```

我们来分解一下上面的一些关键词。

的**指定我们将要创建一个循环。后面是括号，嵌套了循环工作所需的所有内容。**

**初始化**定义一个初始变量作为循环的起点，通常是整数(整数)。

**条件**指定循环应该运行的次数。

**增量/减量**每次循环运行时，增加/减少初始变量的值。随着增量/减量的发生，变量值趋向于指定的**条件**。

请注意，每个关键字由分号(；).

这里有几个例子:

```
for(int x = 1; x <=5; x++) {
  System.out.println(x);
}

/*
1
2
3
4
5
*/
```

在上面的例子中，初始变量是值为 1 的`x`。只要`x`的值小于或等于 5，循环就会一直运行——这是条件。`x++`每次运行后增加`x`的值。

我们继续打印`x`的值，它在 5 之后停止，因为条件已经满足。不可能递增到 6，因为它大于且不等于 5。

在下一个例子中，我们将使用`for`循环来打印一个数组的所有值。

```
int[] randomNumbers = {2, 5, 4, 7};
for (int i = 0; i < randomNumbers.length; i++) {
  System.out.println(randomNumbers[i]);
}

// 2
// 5
// 4
// 7
```

这和上一个例子差不多。这里，我们使用数组的长度作为条件，初始变量的值为零，因为数组的第一个元素的索引号为零。

## Java 中`forEach`循环的语法

您可以使用一个`forEach`循环来遍历数组中的元素。下面是语法的样子:

```
for (dataType variableName : arrayName) {
  // code to be executed
}
```

您会注意到这里的语法比`for`循环的语法短。`forEach`循环也以关键字的**开始。**

我们没有用值初始化变量，而是首先指定了**数据类型**(这必须与数组的数据类型匹配)。后面是我们的**变量名**和数组的**名，用冒号隔开。**

下面是一个帮助您更好地理解语法的示例:

```
int[] randomNumbers = {2, 5, 4, 7};
for (int x : randomNumbers) {
  System.out.println(x + 1);
}

/*
3
6
5
8
*/
```

在这个例子中，我们遍历了每个元素，并将它们的初始值增加了 1。

默认情况下，一旦遍历完数组中的所有元素，循环就会停止。这意味着我们不需要向变量传递任何值，也不需要指定任何条件来终止循环。

## 结论

在本文中，我们学习了什么是循环，以及在 Java 中创建`for`和`forEach`循环的语法。我们还看到了一些帮助我们理解何时以及如何使用它们的例子。

编码快乐！