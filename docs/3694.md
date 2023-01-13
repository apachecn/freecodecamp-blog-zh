# Java 中的字符串

> 原文：<https://www.freecodecamp.org/news/strings-in-java/>

# **琴弦**

字符串是字符序列。在 Java 中，`String`是一个`Object`。字符串不应该与`char`混淆，因为字符实际上是一个值，而不是一个字符序列。您仍然可以在一个字符串中使用 1 个值，但是当您检查 1 个字符时，最好使用`char`。

```
String course = "FCC";
System.out.println(course instanceof Object);
```

输出:

```
true
```

您可以通过以下方式创建 String 对象:

1.  `String str = "I am a String"; //as a String literal`
2.  `String str = "I am a " + "String"; //as a constant expression`
3.  `String str = new String("I am a String"); //as a String Object using the constructor`

你可能会想:这三者有什么区别？

嗯，使用`new`关键字保证了一个新的`String`对象将被创建，一个新的内存位置将被分配到`Heap`内存[(点击此处了解更多)](https://docs.oracle.com/cd/E13150_01/jrockit_jvm/jrockit/geninfo/diagnos/garbage_collect.html)。字符串文字和常量字符串表达式在编译时被缓存。编译器将它们放在字符串池中，以防止重复并减少内存消耗。对象分配开销很大，这种技巧在实例化字符串时提高了性能。如果再次使用相同的文字，JVM 将使用相同的对象。使用上面的构造器几乎总是一个糟糕的选择。

在这段代码中，创建了多少个字符串对象？

```
String str = "This is a string";
String str2 = "This is a string";
String str3 = new String("This is a string");
```

答案是:创建了 2 个字符串对象。`str`和`str2`都是指同一个物体。`str3`有相同的内容，但是使用`new`强制创建一个新的、独特的对象。

当您创建一个字符串文字时，JVM 会进行内部检查，这就是所谓的`String pool`，看看它是否能找到一个类似的(内容方面的)字符串对象。如果找到它，它将返回相同的引用。否则，它会在池中创建一个新的 String 对象，以便将来可以执行相同的检查。

您可以使用吞咽、快速对象比较`==`和实现的`equals()`来测试这一点。

```
System.out.println(str == str2); // This prints 'true'
System.out.println(str == str3); // This prints 'false'
System.out.println(str.equals(str3)); // This prints 'true'
```

下面是另一个关于如何使用不同的方法在 Java 中创建字符串的例子:

```
public class StringExample{  

   public static void main(String args[]) {  
      String s1 = "java";  // creating string by Java string literal  
      char ch[] = {'s','t','r','i','n','g','s'};  
      String s2 = new String(ch);  // converting char array to string  
      String s3 = new String("example");  // creating Java string by new keyword  
      System.out.println(s1);  
      System.out.println(s2);  
      System.out.println(s3);  
   }
}
```

#### **比较字符串**

如果要比较两个字符串变量的值，不能用==。这是因为这将比较变量的引用，而不是链接到它们的值。要比较字符串的存储值，可以使用 equals 方法。

```
boolean equals(Object obj)
```

如果两个对象相等，则返回 true，否则返回 false。

```
String str = "Hello world";
String str2 = "Hello world";

System.out.println(str == str2); // This prints false
System.out.println(str.equals(str2); // This prints true
```

第一个比较为假，因为“==”查看引用，它们不相同。

第二个比较为真，因为变量存储相同的值。在这种情况下是“Hello world”。

我们在 String 中有几个内置的方法。下面是 String Length()方法的一个示例。

```
public class StringDemo {

   public static void main(String args[]) {
      String palindrome = "Dot saw I was Tod";
      int len = palindrome.length();
      System.out.println( "String Length is : " + len );
   }
}
```

这将导致- `String Length is : 17`

****答案是:2**** 创建字符串对象。 ****音符****

1.  字符串方法使用从零开始的索引，除了第二个参数`substring()`。
2.  String 类是最终的——它的方法不能被覆盖。
3.  当 JVM 找到字符串时，它被添加到字符串池中。
4.  字符串类拥有一个方法名`length()`，而数组有一个属性命名长度。
5.  在 java 中，字符串对象是不可变的。不可变仅仅意味着不可修改或不可改变。一旦 string 对象被创建，它的数据或状态不能被改变，但是一个新的 string 对象被创建。

字符串长度

字符串的“长度”就是其中字符的数量。所以“嗨”的长度是 2，“你好”的长度是 5。字符串的 length()方法返回其长度，如下所示:

```
String a = "Hello";
int len = a.length();  // len is 5
```

#### **也可以在字符串上使用的其他比较方法有:**

1.  equalsIgnoreCase() :-比较字符串，不考虑大小写。

```
String a = "HELLO";
String b = "hello";
System.out.println(a.equalsIgnoreCase(b));   // It will print true
```

1.  compareTo :-按字典顺序比较值，并返回一个整数。

```
String a = "Sam";
String b = "Sam";
String c = "Ram";
System.out.println(a.compareTo(b));       // 0
System.out.prinltn(a.compareTo(c));       // 1 since (a>b)
System.out.println(c.compareTo(a));       // -1 since (c<a)
```