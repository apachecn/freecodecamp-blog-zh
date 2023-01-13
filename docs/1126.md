# Java 数组——如何在 Java 示例中声明和初始化数组

> 原文：<https://www.freecodecamp.org/news/java-array-how-to-declare-and-initialize-an-array-in-java-example/>

在本文中，我们将讨论 Java 中的数组。我们将通过一些例子来帮助你理解什么是数组，如何声明它们，以及如何在你的 Java 代码中使用它们。

## 什么是数组？

在 Java 中，使用数组将同一数据类型的多个值存储在一个变量中。您也可以将其视为相同数据类型的值的集合。这意味着，例如，如果你要在数组中存储字符串，那么数组中的所有值都应该是字符串。

## 如何在 Java 中声明数组

我们使用方括号`[]`来声明一个数组。那就是:

```
String[] names;
```

我们已经声明了一个名为`names`的变量，它将保存一个字符串数组。

如果我们要为整数声明一个变量，我们会这样做:

```
int[] myIntegers;
```

因此，要创建数组，您需要指定将存储在数组中的数据类型，后跟方括号，然后是数组的名称。

## 如何在 Java 中初始化数组

初始化一个数组仅仅意味着给数组赋值。让我们初始化我们在上一节中声明的数组:

```
String[] names = {"John", "Jade", "Love", "Allen"};
```

```
int[] myIntegers = {10, 11, 12};
```

我们通过传入相同数据类型的值来初始化数组，每个值用逗号分隔。

如果我们想要访问数组中的元素/值，我们将引用它们在数组中的索引号。第一个元素的索引是 0。这里有一个例子:

```
String[] names = {"John", "Jade", "Love", "Allen"};

System.out.println(names[0]);
// John

System.out.println(names[1]);
// Jade

System.out.println(names[2]);
// Love

System.out.println(names[3]);
// Allen
```

既然我们现在知道如何访问每个元素，让我们改变第三个元素的值。看起来像这样:

```
String[] names = {"John", "Jade", "Love", "Allen"};
names[2] = "Victor";

System.out.println(names[2]);
// Victor
```

我们还可以使用`length`属性来检查数组的长度。这里有一个例子:

```
String[] names = {"John", "Jade", "Love", "Allen"};
System.out.println(names.length);
// 4
```

## 如何在 Java 中循环遍历数组

我们可以使用`for`循环遍历数组中的元素。

```
String[] names = {"John", "Jade", "Love", "Allen"};
for (int i = 0; i < names.length; i++) {
  System.out.println(names[i]);
}

// John
// Jade
// Love
// Allen
```

上面的循环将打印数组的元素。我们已经使用了`length`属性来指定循环应该运行的次数。

## 结论

在本文中，我们学习了如何在 Java 代码中声明和初始化数组。我们还看到了如何访问数组中的每个元素，以及如何遍历这些元素。

编码快乐！