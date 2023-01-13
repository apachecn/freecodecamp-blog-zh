# C#基础——你的第一个 C#程序，类型和变量，以及流控制语句

> 原文：<https://www.freecodecamp.org/news/c-basics-your-first-c-program-types-and-variables-and-flow-control-statements/>

## **设置**

[LinqPad](http://www.linqpad.net/) 是一个。NET scratchpad 来快速测试您的 C#代码片段。标准版是免费的，是初学者执行语言语句、表达和程序的完美工具。

或者，你也可以下载[Visual Studio Community 2015](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx)，这是一个可扩展的 [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) ，大多数专业人士使用它来创建企业应用。

## **你的第一个 C#程序**

```
//this is the single line comment

/** This is multiline comment,
compiler ignores any code inside comment blocks.
**/

//This is the namespace, part of the standard .NET Framework Class Library
using System;
// namespace defines the scope of related objects into packages
namespace Learning.CSharp
{  
  // name of the class, should be same as of .cs file
  public class Program
  {
    //entry point method for console applications
   public static void Main()
    {
      //print lines on console
      Console.WriteLine("Hello, World!");
      //Reads the next line of characters from the standard input stream.Most common use is to pause program execution before clearing the console.
      Console.ReadLine();
    }
  }
}
```

每个 C#控制台应用程序都必须有一个 [Main 方法](https://msdn.microsoft.com/en-gb/library/acy3edy3.aspx)，它是程序的入口点。

在中编辑 [HelloWorld](https://dotnetfiddle.net/kY7QRm) 。NET Fiddle，这是一个受 [JSFiddle](http://jsfiddle.net/) 启发的工具，你可以修改代码片段并亲自检查输出。注意，这只是为了分享和测试代码片段，不用于开发应用程序。

如果您正在使用 visual Studio，请按照本[教程](https://msdn.microsoft.com/en-us/library/k1sx6ed2.aspx)创建控制台应用程序并理解您的第一个 C#程序。

## **类型和变量**

C#是一种强类型语言。每个变量都有一个类型。每个表达式或语句都计算一个值。C#中有两种类型:

*   值类型
*   引用类型。

****值类型**** :值类型的变量直接包含值。将一个值类型变量赋给另一个值类型变量会复制所包含的值。

[编辑在。网络小提琴](https://dotnetfiddle.net/JCkTxb)

```
int a = 10;
int b = 20;
a=b;
Console.WriteLine(a); //prints 20
Console.WriteLine(b); //prints 20
```

请注意，在其他动态语言中，这可能不同，但在 C#中，这始终是一个值副本。当值类型被创建时，一个很可能在堆栈[中的单个空间](http://gribblelab.org/CBootcamp/7_Memory_Stack_vs_Heap.html#orgheadline2)被创建，这是一个“后进先出”的数据结构。堆栈有大小限制，内存操作是高效的。内置数据类型的几个例子是`int, float, double, decimal, char and string`。

| 类型 | 例子 | 描述 |
| --- | --- | --- |
| *整数* | `int fooInt = 7;` | **带符号的 32 位**整数 |
| *龙* | `long fooLong = 3000L;` | **有符号 64 位**整数。 **L 用于指定该变量值的类型为 long/ulong** |
| *Double* | `double fooDouble = 20.99;` | 精度: **15-16 位数字** |
| *浮动* | `float fooFloat = 314.5f;` | 精度: **7 位数字**。 **F 用于指定该变量值的类型为 float** |
| *Decimal* | `decimal fooDecimal = 23.3m;` | 精度: **28-29 位数字**。其更高的精度和更小的范围使其适用于**金融和货币计算** |
| *字符* | `char fooChar = 'Z';` | 单个 **16 位 Unicode 字符** |
| *布尔型* | `bool fooBoolean = false;` | 布尔- **真&假** |
| *字符串* | `string fooString = "\"escape\" quotes and add \n (new lines) and \t (tabs);` | **一串 Unicode 字符。** |

有关所有内置数据类型的完整列表，请参见此处的

[****引用类型****](https://msdn.microsoft.com/en-us/library/490f96s2.aspx) :引用类型的变量存储对其对象的引用，这意味着它们将地址存储到数据在[栈](http://gribblelab.org/CBootcamp/7_Memory_Stack_vs_Heap.html#orgheadline2)上的位置，也称为指针。实际数据存储在[堆](http://gribblelab.org/CBootcamp/7_Memory_Stack_vs_Heap.html#orgheadline3)内存中。将引用类型分配给另一个引用类型并不会复制数据，相反，它会创建引用的第二个副本，该副本指向堆上的相同位置。

在堆中，对象以随机顺序分配和释放，这就是为什么这需要内存管理和[垃圾收集](https://msdn.microsoft.com/en-us/library/hh156531(v=vs.110)的开销。aspx)。

除非你正在编写[不安全代码](https://msdn.microsoft.com/en-us/library/t2yzs44b.aspx)或者处理[非托管代码](https://msdn.microsoft.com/en-us/library/sd10k43k(v=vs.100)。aspx)，你不需要担心你的内存位置的寿命。。NET 编译器和 CLR 会处理好这一点，但是为了优化应用程序的性能，最好还是记住这一点。

更多信息[此处](http://www.c-sharpcorner.com/UploadFile/rmcochran/csharp_memory01122006130034PM/csharp_memory.aspx?ArticleID=9adb0e3c-b3f6-40b5-98b5-413b6d348b91)。

## **流量控制语句**

### [If else](https://msdn.microsoft.com/en-us/library/5011f09h.aspx) 语句

```
int myScore = 700;
if (myScore == 700) {
 Console.WriteLine("I get printed on the console");
} else if (myScore > 10) {
 Console.WriteLine("I don't");
} else {
 Console.WriteLine("I also don't");
}

/** Ternary operators
 A simple if/else can also be written as follows
 <condition> ? <true> : <false> **/
int myNumber = 10;
string isTrue = myNumber == 10 ? "Yes" : "No";
```

[编辑在。网络小提琴](https://dotnetfiddle.net/IFVB33)

### [开关](https://msdn.microsoft.com/en-GB/library/06tc147t.aspx)声明

```
using System;
public class Program {
 public static void Main() {
   int myNumber = 0;
   switch (myNumber) { // A switch section can have more than one case label. 
    case 0:
    case 1:
     {
      Console.WriteLine(“Case 0 or 1”);
      break;
     }

   }
```

[编辑在。网络小提琴](https://dotnetfiddle.net/lPZftO)

### [为](https://msdn.microsoft.com/en-us/library/ch45axte.aspx) &为[为](https://msdn.microsoft.com/en-gb/library/ttw7t8t6.aspx)

```
for (int i = 0; i < 10; i++) {
 Console.WriteLine(i); //prints 0-9 }
 Console.WriteLine(Environment.NewLine);
 for (int i = 0; i <= 10; i++) {
  Console.WriteLine(i); //prints 0-10 }
  Console.WriteLine(Environment.NewLine);
  for (int i = 10 - 1; i >= 0; i—) //decrement loop 
  {
   Console.WriteLine(i); //prints 9-0 }
   Console.WriteLine(Environment.NewLine); //for (; ; ) { // All of the expressions are optional. This statement //creates an infinite loop.* //}
```

[编辑在。网络小提琴](https://dotnetfiddle.net/edxtvq)

### [While](https://msdn.microsoft.com/en-us/library/2aeyhxcd.aspx)&[do-While](https://msdn.microsoft.com/en-us/library/370s1zax.aspx)

```
// Continue the while-loop until index is equal to 10\. 
int i = 0;
while (i < 10) {
 Console.Write(“While statement”);
 Console.WriteLine(i); // Write the index to the screen. i++;// Increment the variable. }
 int number = 0; // do work first, until condition is satisfied i.e Terminates when number equals 4\. do 
 {
  Console.WriteLine(number); //prints the value from 0-4 
  number++; // Add one to number. 
 }
 while (number <= 4);
```

[编辑在。网络小提琴](https://dotnetfiddle.net/O5hOF1)