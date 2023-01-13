# 如何开始在 Java 中使用 Lambda 表达式

> 原文：<https://www.freecodecamp.org/news/learn-these-4-things-and-working-with-lambda-expressions-b0ab36e0fffc/>

路易斯·圣地亚哥

# **如何开始在 Java 中使用 Lambda 表达式**

![1*_Ascfro4BseNIKWOJ06MwQ](img/02b801bd4d4a609fe7ba6195c1abcaa5.png)

在 JDK 8 加入 Lambda 表达式支持之前，我只在 C#和 C++这样的语言中使用过它们的例子。

一旦这个特性被添加到 Java 中，我就开始更深入地研究它们。

lambda 表达式的加入增加了语法元素，增强了 Java 的表达能力。在本文中，我想把重点放在您需要熟悉的基本概念上，这样您就可以从今天开始向代码中添加 lambda 表达式。

#### 快速介绍

Lambda 表达式利用了多核环境的并行处理能力，如在流 API 中对数据的管道操作的支持所见。

它们是匿名方法(没有名字的方法),用于实现由函数接口定义的方法。在接触 lambda 表达式之前，了解什么是函数接口是很重要的。

#### 功能接口

函数接口是包含且仅包含一个抽象方法的接口。

如果你看一下 Java 标准 [Runnable 接口](https://docs.oracle.com/javase/7/docs/api/java/lang/Runnable.html)的定义，你会注意到它是如何落入函数接口的定义的，因为它只定义了一个方法:`run()`。

在下面的代码示例中，方法`computeName`是隐式抽象的，并且是唯一定义的方法，这使得 MyName 成为一个函数接口。

```
interface MyName{
  String computeName(String str);
}
```

Functional Interface

#### 箭头运算符

Lambda 表达式在 Java 中引入了新的箭头运算符`->`。它将 lambda 表达式分为两部分:

```
(n) -> n*n
```

左侧指定表达式所需的参数，如果不需要参数，也可以为空。

右边是 lambda 主体，它指定了 lambda 表达式的动作。把这个操作符想成“变成”可能会有帮助。比如“n 变成 n*n”，或者“n 变成 n 的平方”。

记住函数接口和箭头操作符的概念后，您可以构建一个简单的 lambda 表达式:

```
interface NumericTest {
	boolean computeTest(int n); 
}

public static void main(String args[]) {
	NumericTest isEven = (n) -> (n % 2) == 0;
	NumericTest isNegative = (n) -> (n < 0);

	// Output: false
	System.out.println(isEven.computeTest(5));

	// Output: true
	System.out.println(isNegative.computeTest(-5));
}
```

Numeric Test

```
interface MyGreeting {
	String processName(String str);
}

public static void main(String args[]) {
	MyGreeting morningGreeting = (str) -> "Good Morning " + str + "!";
	MyGreeting eveningGreeting = (str) -> "Good Evening " + str + "!";

  	// Output: Good Morning Luis! 
	System.out.println(morningGreeting.processName("Luis"));

	// Output: Good Evening Jessica!
	System.out.println(eveningGreeting.processName("Jessica"));	
}
```

Greeting Lambda

上面例子中第 6 行和第 7 行的变量`morningGreeting`和`eveningGreeting`，引用了`MyGreeting`接口，定义了不同的问候表达式。

编写 lambda 表达式时，也可以像这样在表达式中显式指定参数的类型:

```
MyGreeting morningGreeting = (String str) -> "Good Morning " + str + "!";
MyGreeting eveningGreeting = (String str) -> "Good Evening " + str + "!";
```

Lambda with Type

#### 阻止 Lambda 表达式

到目前为止，我已经介绍了单个表达式 lambdas 的示例。当箭头运算符右侧的代码包含多个语句时，使用另一种类型的表达式，称为 **block lambdas** :

```
interface MyString {
	String myStringFunction(String str);
}

public static void main (String args[]) {
	// Block lambda to reverse string
	MyString reverseStr = (str) -> {
		String result = "";

		for(int i = str.length()-1; i >= 0; i--)
			result += str.charAt(i);

		return result;
	};

	// Output: omeD adbmaL
	System.out.println(reverseStr.myStringFunction("Lambda Demo")); 
}
```

Block Lambda

#### 通用功能接口

lambda 表达式不能是泛型的。但是与 lambda 表达式相关联的函数接口可以。可以编写一个通用接口并处理不同的返回类型，如下所示:

```
interface MyGeneric<T> {
	T compute(T t);
}

public static void main(String args[]){

	// String version of MyGenericInteface
	MyGeneric<String> reverse = (str) -> {
		String result = "";

		for(int i = str.length()-1; i >= 0; i--)
			result += str.charAt(i);

		return result;
	};

	// Integer version of MyGeneric
	MyGeneric<Integer> factorial = (Integer n) -> {
		int result = 1;

		for(int i=1; i <= n; i++)
			result = i * result;

		return result;
	};

	// Output: omeD adbmaL
	System.out.println(reverse.compute("Lambda Demo")); 

	// Output: 120
	System.out.println(factorial.compute(5)); 

}
```

Generic Functional Interface

#### 作为参数的 Lambda 表达式

lambdas 的一个常见用途是将它们作为参数传递。

它们可以用在任何提供目标类型的代码中。我发现这很令人兴奋，因为它让我可以将可执行代码作为参数传递给方法。

要将 lambda 表达式作为参数传递，只需确保函数接口类型与所需的参数兼容。

```
interface MyString {
	String myStringFunction(String str);
}

public static String reverseStr(MyString reverse, String str){
  return reverse.myStringFunction(str);
}

public static void main (String args[]) {
	// Block lambda to reverse string
	MyString reverse = (str) -> {
		String result = "";

		for(int i = str.length()-1; i >= 0; i--)
			result += str.charAt(i);

		return result;
	};

	// Output: omeD adbmaL
	System.out.println(reverseStr(reverse, "Lambda Demo")); 
}
```

Lambda Expression as an Argument

这些概念将为您开始使用 lambda 表达式打下良好的基础。看看您的代码，看看您可以在哪些地方增加 Java 的表达能力。