# Java 中的数据类型

> 原文：<https://www.freecodecamp.org/news/data-types-in-java/>

Java 是一种强类型语言。这意味着，在 Java 中，每种数据类型都有自己严格的定义。当数据类型之间发生冲突时，没有隐式数据类型转换。数据类型的任何变化都应该由程序员明确声明。

Java 定义了 8 种原始数据类型:`byte`、`short`、`int`、`long`、`char`、`float`、`double`和`boolean`。

它们分为以下几类:

*   整数
*   浮点数
*   特性
*   布尔型

每种数据类型的详细信息如下:

## **整数:**

这些有四种类型:`byte`、`short`、`int`、`long`。重要的是要注意，这些是有符号的正值和负值。有符号整数使用 [2 的补码](http://www.ele.uri.edu/courses/ele447/proj_pages/divid/twos.html)存储在计算机中。它由负值和正值组成，但格式不同，如`(-1 to -128)`或`(0 to +127)`。无符号整数可以容纳更大的正值，而不像`(0 to 255)`那样可以容纳负值。与 C++不同，Java 中没有无符号整数。

### **字节:T1**

Byte 数据类型是 8 位有符号二进制补码整数。

```
Wrapper Class: Byte

Minimum value: -128 (-2^7)

Maximum value: 127 (2^7 -1)

Default value: 0

Example: byte a = 10 , byte b = -50;
```

### **简称:**

短数据类型是 16 位有符号二进制补码整数。

```
Wrapper Class: Short

Minimum value: -32,768 (-2^15)

Maximum value: 32,767 (2^15 -1)

Default value: 0.

Example: short s = 10, short r = -1000;
```

### **int:**

int 数据类型是 32 位有符号二进制补码整数。它通常用作整型值的默认数据类型，除非担心内存问题。

```
Wrapper Class: Integer

Minimum value: (-2^31)

Maximum value: (2^31 -1)

The default value: 0.

Example: int a = 50000, int b = -20
```

### **长:**

Long 数据类型是 64 位有符号二进制补码整数。

```
Wrapper Class: Long

Minimum value: (-2^63)

Maximum value: (2^63 -1)

Default value: 0L.

Example: long a = 100000L, long b = -600000L; 

By default all integer type variable is "int". So long num=600851475143  will give an error.
But it can be specified as long by appending the suffix L (or l)
```

## **浮点:**

这些也称为实数，用于涉及小数精度的表达式。这些有两种类型:`float`、`double`。在货币或研究数据等精确数据的情况下，浮动实际上是要避免的。

### **浮动:**

浮点数据类型是单精度 32 位 [IEEE 754 浮点](http://steve.hollasch.net/cgindex/coding/ieeefloat.html)。

```
Wrapper Class: Float

Float is mainly used to save memory in large arrays of floating point numbers.

Default value: 0.0f.

Example: float f1 = 24.5f;

The default data type of floating-point number is double. So float f = 24.5 will introduce an error.
However, we can append the suffix F (or f) to designate the data type as float.
```

### **double:**

double 数据类型是双精度 64 位 [IEEE 754 浮点](http://steve.hollasch.net/cgindex/coding/ieeefloat.html)。这种数据类型通常是默认选择。这种数据类型不应该用于精确值，如货币。

```
Wrapper Class: Double

This data type is generally used as the default data type for decimal values.

Default value: 0.0d.

Example: double d1 = 123.400778;
```

## **字符:**

我们使用这种数据类型来存储字符。这和 C/C++里的 char 不一样。Java 使用的是国际通用的字符集`UNICODE`。Java 中的 Char 是 16 位，而 C/C++中的 char 是 8 位。

```
Wrapper Class: Character

Minimum value: '\u0000' (or 0).

Maximum value: '\uffff' (or 65,535).

Default value: null ('\u0000').

Example: char letterA ='a';
```

## **布尔:**

这用于存储逻辑值。布尔类型的值可以是 true 或 false。这种类型通常由关系运算符返回。

```
There are only two possible values: true and false.

Wrapper Class: Boolean

This data type is used for simple flags that track true/false conditions.

Default value is false.

Example: boolean b = true, boolean b1 = 1(this is not possible in java and give incompatible type error), boolean b2;
```

## **引用数据类型:**

除了原始数据类型，还有使用不同类的构造函数创建的引用变量。引用变量用于任何类以及数组、字符串、扫描仪、随机、骰子等。使用 new 关键字初始化参考变量。

示例:

```
public class Box{

    int length, breadth, height;

    public Box(){
        length=5;
        breadth=3;
        height=2;
    }
}

class demo{

    public static void main(String args[]) {
        Box box1 = new Box();                //box1 is the reference variable  
        char[] arr = new char[10];           //arr is the reference variable
    }
}
```

## **字符串:**

String 不是基本数据类型，但是它允许您在一个数组中存储多个字符数据类型，并且有许多方法可以使用。当用户输入数据并且你必须操作它的时候，它经常被使用。

在下面的示例中，我们尝试从字符串中删除所有字母并输出它:

```
String input = "My birthday is 10 January 1984 and my favorite number is 42";
String output = "";

for(int i=0;i<input.length();i++){

//if the character at index i on the string is a letter or a space, move on to the next index
if(Character.isLetter(input.charAt(i)) || input.charAt(i)==' '){ 

    continue;
}

output = output + input.charAt(i); //the number is added onto the output

}

System.out.println(output);
```

输出:

```
10198442
```

## 关于 Java 的更多信息:

*   [Java 绝对初学者(全视频课程)](https://www.freecodecamp.org/news/java-for-absolute-beginners-full-course/)
*   [Java 编程语言基础知识](https://www.freecodecamp.org/news/java-programming-language-basics/)
*   [最好的 Java 例子](https://www.freecodecamp.org/news/java-example/)
*   [最好的 Java 8 教程](https://www.freecodecamp.org/news/best-java-8-tutorial/)
*   [Java 中的继承解释](https://www.freecodecamp.org/news/inheritance-in-java-explained/)
*   [Java 访问修饰符](https://guide.freecodecamp.org/java/access-modifiers/)