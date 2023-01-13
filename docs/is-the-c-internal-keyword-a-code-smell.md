# C#内部关键字是不是代码味？

> 原文：<https://www.freecodecamp.org/news/is-the-c-internal-keyword-a-code-smell/>

在这篇文章中，我将展示为什么我认为内部关键字，当放在类成员上时，是一种代码味道，并建议更好的替代方法。

### 内部关键词是什么？

在 C#中，internal 关键字可以在类或其成员上使用。它是 C# [访问修饰符](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/access-modifiers) s 中的一个。内部类型或成员只能在同一程序集的 **文件中访问**。( [C#内部关键字文档](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/internal))。****

### 为什么我们需要内部关键字？

“内部访问的一个常见用途是在基于组件的开发中，因为它**使一组组件能够以私有方式协作，而不会暴露给应用程序代码的其余部分**。例如，用于构建图形用户界面的框架可以通过使用具有内部访问权限的成员来提供控件和窗体类。由于这些成员是内部成员，因此它们不会向使用该框架的代码公开。( [C#内部关键字文档](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/internal))

这些是我看到的在类成员上使用 internal 关键字的用例:

*   在同一程序集中调用类的私有函数。
*   为了测试一个私有函数，你可以将它标记为内部的，并通过 [InternalsVisibleTo](https://docs.microsoft.com/en-us/dotnet/api/system.runtime.compilerservices.internalsvisibletoattribute?view=netframework-4.8) 将 dll 暴露给测试 DLL。

这两种情况都可以看成是[代码味，](https://enterprisecraftsmanship.com/2017/10/23/unit-testing-private-methods/)说这个私有函数应该是公有的。

### 让我们看一些例子

这里有一个简单的例子。一个类中的函数想要访问另一个类的私有函数。

```
class A{
	public void func1(){
		func2();
	}
	private void func2(){}
}

class B{
	public void func(A a){
		a.func2(); //Compilation error 'A.func2()' is inaccessible due to its protection level
	}
}
```

解决方案很简单——只需将 A::func2 标记为 public。

让我们看一个稍微复杂一点的例子:

```
public class A{
	public void func1(){}
	private void func2(B b){}
}

internal class B{
	public void func3(A a){		
		a.func2(this); //Compilation error 'A.func2(B)' is inaccessible due to its protection level
	}
}
```

有什么问题？就像我们之前做的那样将 func2 标记为 public。

```
public class A{
	public void func1(){ ... }
	public void func2(B b){ ...} // Compilation error: Inconsistent accessibility: parameter type 'B' is less accessible than method 'A.func2(B)' 
}

internal class B{
	public void func3(A a){		
		a.func2(this); 
	}
}
```

但是我们不能？。b 是内部类，因此它不能是公共类的公共函数签名的一部分。

这些是我找到的解决方案，按容易程度排序:

1.  用 internal 关键字标记函数

```
public class A{
	public void func1(){ }
	internal void func2(B b){}
}

internal class B{
	public void func3(A a){		
		a.func2(this); 
	}

}
```

2.创建内部接口

```
internal interface IA2{
	void func2(B b);
}

public class A:IA2{
	public void func1(){
		var b = new B();
		b.func3(this);
	}
	void IA2.func2(B b){} //implement IA2 explicitly because func2 can't be public
}

internal class B{
	public void func3(A a){ 
		((IA2)a).func2(this); //use interface instead of class to access func2	
	}

}
```

3.将 A.func2 提取到另一个内部类中，用它代替 A.func2。

```
internal class C{
	public void func2(B b){
	 //extract A:func2 to here
	}
}

public class A{
	public void func1(){}
	private void func2(B b){
		new C().func2(b); 
	}
}

internal class B{
	public void func3(){	//a is no longer needed	
		new C().func2(this); //use internal class instead of private function
	}

}
```

4.将函数从内部类中分离出来，并将其公开。这在很大程度上取决于函数如何处理它的输入。内部类的解耦可能非常容易，非常困难，甚至是不可能的(不破坏设计)。

#### 但是我们没有公共类，我们使用接口…

让我们看一些更真实的例子:

```
public interface IA{
	void func1();
}

internal class A : IA {
	public void func1(){}
	private void func2(B b){}
}

internal class B{
	public void func3(IA a){		
		a.func2(this);  //Compilation error IA' does not contain a definition for 'func2' and no extension method 'func2' accepting a first argument of type 'IA' could be found

	}	
}
```

让我们看看前面的解决方案是如何适应本例的:

1.  用 Internal 标记函数。这意味着您将需要强制转换为 class 以调用函数，因此只有当 class A 是唯一实现接口的类时，这才会起作用，这意味着 IA 在测试中不会被模仿，并且没有其他实现 IA 的生产类。

```
public interface IA{
	void func1();
}

internal class A : IA {
	public void func1(){}
	internal void func2(B b){}
}

internal class B{
	public void func3(IA a){		
		((A)a).func2(this); //cast to A in order to accses func2

	}	
}
```

2.创建扩展公共接口的内部接口。

```
internal interface IExtendedA : IA{
	void func2(B b);
}

public interface IA{
	void func1();
}

internal class A : IExtendedA {
	public void func1(){}
	public void func2(B b){}
}

internal class B{
	public void func3(IExtendedA a){		
		a.func2(this);

	}	
}
```

3.将 A.func2 提取到另一个内部类中，用它代替 A.func2。

4.将函数从内部类中分离出来，并将其添加到公共接口中。

我们可以看到，**内部关键字是最简单的解决方案**，但是也有其他解决方案使用了 OOP 的**传统构建模块:类和接口**。我们可以看到第二个解决方案——添加一个内部接口并不比用 internal 关键字标记函数难多少。

### 为什么不使用内部关键字？

正如我在前面的例子中展示的，使用**内部关键字是最简单的解决方案**。但是如果你需要的话，你未来的日子会很艰难:

*   将公共类 A 移动到另一个 dll(因为 internal 关键字将不再适用于同一个 DLL)
*   创建另一个实现 IA 的生产类
*   测试中的模拟 IA

你可能会想“但这只是一行代码，如果需要，我或其他任何人都可以很容易地更改它”。**现在**你有了**的一行**代码，如下所示:

```
((MyClass)a).internalFunction
```

但是如果其他人也需要调用这个函数，这个**行将被复制粘贴到 DLL 中的**周围。

### 我的结论

我认为用内部关键字**标记类成员是一种代码气味**。在我上面展示的例子中，这是**最简单的**解决方案，但是在将来会引起问题。创建一个**内部接口几乎同样简单，而且更加明确。**

### 与 C++相比

c++“friend”关键字类似于 C#内部关键字。它允许类或函数访问类的私有成员。不同的是它允许访问**特定的**类或函数和**而不是** **同一个 DLL 中的所有类。**在我看来，这是比 C#内部关键字更好的解决方案。

### 进一步阅读

[**【内部】关键字在 C#中的实际用途**](https://stackoverflow.com/questions/165719/practical-uses-for-the-internal-keyword-in-c-sharp)
[**为什么 C#不提供 C++风格的‘朋友’关键字？**](https://stackoverflow.com/questions/203616/why-does-c-sharp-not-provide-the-c-style-friend-keyword)