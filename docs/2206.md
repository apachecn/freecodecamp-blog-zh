# 字符串到字符数组 Java 教程

> 原文：<https://www.freecodecamp.org/news/string-to-char-array-java-tutorial/>

在本文中，我们将研究如何在 Java 中将字符串转换成字符数组。我还将简要地向你解释什么是字符串、字符和数组。

## Java 中的字符是什么？

字符是基本的数据类型。字符是用单引号括起来的单个字符。它可以是字母、数字、标点符号、空格或类似的东西。例如:

```
char firstVowel = 'a';
```

## Java 中的字符串是什么？

字符串是对象(引用类型)。字符串由一串字符组成。双引号内的任何东西。例如:

```
String vowels = "aeiou";
```

## Java 中的数组是什么？

数组是基本的数据结构，在 Java 中可以存储固定数量的相同数据类型的元素。例如，让我们声明一个字符数组:

```
char[] vowelArray = {'a', 'e', 'i', 'o', 'u'};
```

现在，我们对什么是字符串、字符和数组有了基本的了解。

## 让我们把字符串转换成字符数组

### 1.使用 toCharArray()实例方法

`toCharArray()`是`String`类的一个实例方法。它基于当前字符串对象返回一个新的字符数组。

让我们来看一个例子:

```
// define a string
String vowels = "aeiou";

// create an array of characters 
char[] vowelArray = vowels.toCharArray();

// print vowelArray
System.out.println(Arrays.toString(vowelArray));
```

输出:`[a, e, i, o, u]`

当我们将一个字符串转换为一个字符数组时，长度保持不变。让我们检查一下`vowels`和`vowelArray`的长度:

```
System.out.println("Length of \'vowels\' is " + vowels.length());
System.out.println("Length of \'vowelArray\' is " + vowelArray.length);
```

输出:

```
Length of 'vowels' is 5
Length of 'vowelArray' is 5
```

我们可以用各种方法打印一个数组。我使用了来自`Arrays`实用程序类的`toString()`静态方法。

你可以在 [Java 文档](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#toCharArray--)中阅读更多关于`toCharArray()`实例方法的内容。

### 2.使用 charAt()实例方法

`charAt()`是`String`类的一个实例方法。它返回当前字符串的指定索引处的字符。

**注意:**字符串是基于零索引的，类似于数组。

让我们看看如何使用`charAt()`将字符串转换成字符数组:

```
// define a string
String vowels = "aeiou";

// create an array of characters. Length is vowels' length
char[] vowelArray = new char[vowels.length()];

// loop to iterate each characters in the 'vowels' string
for (int i = 0; i < vowels.length(); i++) {
    // add each character to the character array
    vowelArray[i] = vowels.charAt(i);
}

// print the array
System.out.println(Arrays.toString(vowelArray));
```

输出:`[a, e, i, o, u]`

你可以在 [Java 文档](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html#charAt-int-)中阅读更多关于`charAt()`实例方法的内容。

我刚刚向您展示了将字符串转换为字符数组的另一种方法，但是我们可以使用`toCharArray()`方法来轻松转换，而不是创建循环并迭代它们。

如果你有任何建议或问题，请随时告诉我。

亚历克斯·阿尔瓦雷斯在 [Unsplash](https://www.freecodecamp.org/news/s/photos/happy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片。

**请支持 freeCodeCamp 的[数据科学课程承诺活动](https://www.freecodecamp.org/news/building-a-data-science-curriculum-with-advanced-math-and-machine-learning/)。**

通过[媒体](https://mvthanoshan.medium.com/)与我联系。

谢谢你😇

********快乐编码❤️********

### **关于 Java 编程的更多信息**

1.  Java 中面向对象的编程原则:面向初学者的 OOP 概念
2.  [Java 数组方法——如何用 Java 打印数组](https://www.freecodecamp.org/news/java-array-methods-how-to-print-an-array-in-java/)
3.  [Java String to Int–如何将字符串转换成整数](https://www.freecodecamp.org/news/java-string-to-int-how-to-convert-a-string-to-an-integer/)
4.  [Java 随机数生成器——如何用 Math Random 生成整数](https://www.freecodecamp.org/news/generate-random-numbers-java/)