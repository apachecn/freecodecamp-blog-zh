# Java 中的线程是什么？如何用例子创建一个线程

> 原文：<https://www.freecodecamp.org/news/what-are-threads-in-java-how-to-create-one/>

Java 中的线程是预定义的类，当您编写程序时，可以在 java.package 中找到它们。通常，每个程序都有一个由 java.package 提供的线程

所有这些线程使用相同的内存，但是它们是独立的。这意味着一个线程中的任何异常都不会影响其他线程的工作，尽管它们共享相同的内存。

## 您将学到的内容:

*   在这篇文章中，我们将学习如何创建一个线程
*   我们将学习多任务的概念。
*   我们将学习线程的生命周期和线程类。

## Java 中的线程是什么？

线程允许我们在 Java 中更快地做事情。也就是说，它们帮助我们同时完成多项任务。

您使用线程来执行复杂的操作，而不会干扰主程序。

当同时执行多个线程时，这个过程称为多线程。

[多线程](https://www.freecodecamp.org/news/how-to-get-started-with-multithreading-in-java/)主要用于游戏和类似的程序。既然我们现在对多线程有了一些了解，让我们也了解一下多任务的概念。

## Java 中的多任务是什么？

[多任务处理](https://www.techtarget.com/whatis/definition/multitasking)是让用户同时执行多项任务的过程。在 Java 中有两种方法可以实现多任务处理:

1.  **基于进程的多任务处理**:这种类型的多任务处理中进程繁重，消耗大量时间。这是因为程序在不同进程之间切换需要很长时间。
2.  **基于线程的多任务处理**:与基于进程的多任务处理相比，线程是轻量级的，在它们之间切换所需的时间也更短。

现在我们来学习一下线程的工作模式。

## Java 中的线程生命周期

### Java 中的线程生命周期是什么？

在 Java 中，一个线程总是保持在几种不同状态中的一种(我们将在下面读到)。

线程在其生命周期中经历不同的阶段。例如，一个线程首先诞生，然后开始，并经历这些不同的阶段，直到它死亡。

线程模型由各种状态组成。让我们更详细地了解一下它们:

1.  **New** :代码尚未运行时，模型处于新状态。
2.  **运行状态或活动阶段**:程序正在执行或准备执行的状态。
3.  **Suspended state** :如果你想在某件特定的事情发生时暂停活动(并让你暂时停止执行)，你可以使用这个状态。
4.  **阻塞状态**:线程在等待资源时处于阻塞状态。在阻塞状态下，线程调度器通过拒绝出现的不需要的线程来清除队列。
5.  **终止状态**:该状态立即停止线程的执行。一个被终止的线程意味着它已经死了，不能再使用了。

## 线程方法

### Java 中的线程方法有哪些？

在处理多线程应用程序时，Java 中的线程方法非常重要。线程类有一些重要的方法，这些方法由线程自己描述。

现在让我们来了解一下这些方法:

1.  **public void start()** :你使用这个方法在一个单独的执行路径中启动线程。然后，它调用线程对象上的 run()方法。
2.  **public void run()** :这个方法是线程的起点。线程的执行从这个过程开始。
3.  **public final void setName()** :这个方法改变了线程对象的名称。还有一个`getName()`方法用于检索当前上下文的名称。
4.  **public final void set priority()**:使用这个方法来设置线程对象的值。
5.  **public void sleep()** :使用这个方法将线程挂起一段特定的时间。
6.  **public void interrupt()** :你使用这个方法来中断一个特定的线程。如果由于任何原因被阻止，它还会继续执行。
7.  **public final boolean is alive()**:如果线程是活动的，这个方法返回 true。

现在让我们来学习如何创建一个线程。

## 如何在 Java 中创建线程

创建线程有两种方法:

首先，您可以使用 thread 类(扩展语法)创建一个线程。这为您提供了创建和操作线程的构造函数和方法。

thread 类扩展了 object 类并实现了一个 runnable 接口。Java 中的 thread 类是 Java 多线程系统所基于的主要类。

其次，您可以使用 runnable 接口创建一个线程。当您知道带有实例的类将由线程本身执行时，可以使用此方法。

runnable 接口是 Java 中的一个接口，用于执行并发线程。runnable 接口只有一个方法，就是`run()`。

现在让我们看看它们的语法:

#### 如何使用扩展语法:

```
public class Main extends thread {
  public void test() {
    System.out.println("Threads are very helpful in java");
  }
} 
```

这里有一个`extend`方法的例子:

```
public class Main extends test {
  public static void main(String[] args) {
    Main test = new Main();
    test.start();
    System.out.println("Threads are very much helpful in java");
  }
  public void run() {
    System.out.println("Threads are very helpful in java");
  }
} 
```

使用扩展类的语法，我们刚刚在这个例子中实现了它。在编辑器中运行上面的代码，看看它是如何工作的。

#### 如何使用可运行的接口:

```
public class Main implements runnable {
  public void test() {
    System.out.println("Threads are very helpful in java");
  }
}
```

这里有一个使用[可运行接口](https://docs.oracle.com/javase/7/docs/api/java/lang/Runnable.html)的例子:

```
public class cal implements test {
  public static void main(String[] args) {
    cal obj = new cal();
    Thread thread = new Thread(obj);
    thread.start();
    System.out.println("Threads are very much helpful in java");
  }
  public void run() {
    System.out.println("Threads are very much helpful in java");
  }
} 
```

因为我们扩展了 thread 类，所以我们的类对象不会被当作 thread 对象。在您的编译器中运行上面的代码，看看它是如何工作的。

## 如何在 Java 中实现线程–示例

让我们再看几个在 Java 中实现[线程的例子:](https://www.scaler.com/topics/thread-in-java/)

```
class First
{
public static void main (String [ ]args) throws IOException
{
Thread t =Thread.currentThread( );
System.out.println(“CURRENTTHREAD = ” + t);
t.setName(“NewThread”);
t.setPriority(t.getPriority( ) – 1);
System.out.println(“CURRENTTHREAD = ” + t);
System.out.println(“NAME = ” + t.getName( ));
}
} 
```

#### 输出

```
CURRENTTHREAD =THREAD [main, 5, main]
CURRENTTHREAD =THREAD [New Thread, 4, main]
NAME = New Thread 
```

这里我们已经创建了一个线程，然后打印当前线程。然后我们将线程的名称设置为一个新的线程，最后我们打印了线程的名称。在编辑器中运行上面的代码，看看它是如何工作的。

这是另一个例子:

```
class First implements Runnable
{
Thread t;
First( ){
t = new Thread(this,"NEW");
System.out.println(“CHILD :” + t);
t.start();
}
public void run( ) {
try{ for(int i = 5; i>0, i- -) {
System.out.println("CHILD :" + i);
Thread.sleep(500); }
} //END OFTRY BLOCK
catch(InterruptedException e){ }
System.out.println("EXITING CHILD");
} }
class Second
{
public static void main(String [ ]args) throws IOException
{
new First();
try{
for(int i = 5; i>0, i- -)
{
System.out.println("MAIN :"  + i);
Thread.sleep(1000);
}
} //END OFTRY BLOCK
catch(InterruptedException e){ }
System.out.println("Exiting man");
}
} 
```

#### 输出

```
CHILD = THREAD [NEW, 5, main]
MAIN : 5
CHILD : 5
CHILD : 4
MAIN : 4
CHILD : 3
CHILD : 2
MAIN : 3
CHILD : 1
EXITING CHILD
MAIN : 2
MAIN : 1
EXITING MAIN 
```

这里我们已经创建了一个线程，然后打印子线程。然后，我们在 run 函数中运行 for 循环，并打印子对象。在编辑器中运行代码，看看它是如何工作的。

现在让我们了解更多关于多线程的知识。

## Java 中的多线程

正如我上面简要解释的，Java 中的多线程指的是同时执行几个线程。

多线程很有用，因为线程是独立的——在多线程中，我们可以同时执行几个线程，而不会阻塞用户。

这也有助于我们节省时间，因为我们可以同时进行几个操作。Java 中多线程的一个很好的实时例子是文字处理。这个程序在我们写文档的时候检查我们输入的拼写。在这种情况下，每个任务将由不同的线程提供。

### Java 中多线程的用例

现在您已经知道多线程如何通过允许您一起执行多个操作来节省时间，让我们了解一些多线程的实际使用案例:

1.  文字处理，我们在上面讨论过。
2.  游戏。
3.  提高服务器的响应能力。
4.  使用线程同步函数来提供增强的进程间通信。

现在让我们看一个示例程序来了解如何实现多线程:

```
class First implements Runnable
{
Thread t; String S;
First(String Name){
S=Name;
t = new Thread(this,S);
System.out.println("CHILD :" + t);
t.start();
}
public void run( ) {
try{ for(int i = 5; i>0, i- -) {
System.out.println(S + " :" + i);
Thread.sleep(1000); }
} //END OF TRY BLOCK
catch(InterruptedException e){ }
System.out.println("EXITING " + S);
} 
}
class Second
{
public static void main(String [ ]args) throws IOException
{
new First("ONE");
new First("TWO");
new First("THREE");
try{
Thread.sleep(20000);
} //END OFTRY BLOCK
catch(InterruptedException e){ }
System.out.println("EXITING MAIN"); 
```

#### 输出

```
CHILD =THREAD [ONE, 5, main]
CHILD =THREAD [TWO, 5, main]
CHILD =THREAD [THREE, 5, main]
ONE : 5 ONE : 2
TWO : 5 TWO : 2
THREE : 5 THREE:2
ONE : 4 ONE : 1
TWO : 4 TWO : 1
THREE : 4 THREE : 1
ONE : 3 EXITING ONE
TWO : 3 EXITINGTWO
THREE : 3 EXITINGTHREE
EXITING MAIN 
```

在上面的代码中，我们通过使用 run 方法实现了多线程。然后，我们通过使用构造函数启动了一个线程，因为线程是从它创建的。然后我们可以通过调用 start()方法开始。在编辑器中运行代码，看看它是如何工作的。

## 结论

线程是 Java 中的一个轻量级进程。它是流程中的执行路径。在 Java 中创建线程只有两种方法。

在浏览器中，多个选项卡可以是多个线程。一旦线程被创建，它就可以以我们上面讨论的任何状态出现。

感谢您的阅读。