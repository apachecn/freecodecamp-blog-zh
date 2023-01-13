# 如果你还在使用同步，你应该试试 Akka Actor 原因如下

> 原文：<https://www.freecodecamp.org/news/still-using-synchronized-try-akka-actor-instead-ac2f2b22a9ed/>

马丁·布迪

# 如果你还在使用同步，你应该试试 Akka Actor 原因如下

![2kMFYzDOxojZGCZ0RcaCjDJ2kHwa0SC1nF7w](img/d89c2b7a0a1feb08c7292729c375474a.png)

*Synchronized* 是 Java 的传统并发机制。虽然这可能不是我们这些天经常看到的东西，但它仍然是许多图书馆的燃料。问题是，synchronized 既阻塞又复杂。在本文中，我想用一种简单的方式来说明这个问题，以及我为什么要迁移到 Akka Actor 来实现更好、更容易的并发性。

考虑这个简单的代码:

```
 int x; 

 if (x > 0) {
   return true;
 } else {
   return false;
 }
```

所以如果 *x* 为正，则返回 true。简单。

接下来，考虑这个更简单的代码:

```
x++;
```

是的，一个柜台。很简单吧？

然而，在多线程环境中，所有这些代码都会爆炸。

在第一个例子中，真或假不是由 x 的值决定的，而是由 if 测试决定的。因此，如果另一个线程在第一个线程通过 if 测试后将 x 变为负值，即使 x 不再为正值，我们仍然会得到 true。

第二个例子很有欺骗性。虽然只是一行，但实际上有三个操作:读取 *x* ，递增，把更新后的值放回去。如果两个线程同时运行，更新可能会丢失。

当我们有不同的线程同时访问和修改一个变量，我们有一个竞争条件。如果我们只是想构建一个计数器，Java 提供了线程安全的原子变量，其中包括原子整数，我们可以用它来实现这个目的。然而，原子整数只对单个变量有效。我们如何使几个操作原子化？

通过使用*同步*块。首先，让我们看一个更详细的例子。

```
int x; 
public int withdraw(int deduct){
    int balance = x - deduct; 
    if (balance > 0) {
      x = balance;
      return deduct;
    } else {
      return 0;
    }
}
```

这是一个非常基本的提现方法。这也很危险。两个线程同时运行可能会导致银行发出两次提款，即使余额不再足够。现在让我们看看它如何与同步块一起工作:

```
volatile int x;
public int withdraw(int deduct){
  synchronized(this){
    int balance = x - deduct; 
    if (balance > 0) {
      x = balance;
      return deduct;
    } else {
      return 0;
    }
  }
}
```

同步块的思想很简单。一个线程进入并锁定它，其他线程在外面等待。锁是一个对象，在我们这里是*这个*。完成后，锁被释放并传递给另一个线程，该线程随后做同样的事情。此外，请注意深奥的关键字 *volatile* ，需要它来防止线程使用本地 CPU 缓存 *x.*

现在，随着线索的解开，银行将不会意外地发放无资金的提款。然而，随着更多的块和更多的锁，这种结构往往会变得复杂。处理多个锁的风险特别大。这些块可能会无意中相互持有密钥，并最终锁定整个应用程序。除此之外，我们还有一个效率问题。请记住，当一个线程在内部工作时，所有其他线程都在外部等待。等待线程很好…等待。他们除了等待什么都不做。

因此，与其使用这种机制，为什么不将作业放入队列中呢？为了更好地形象化，想象一个电子邮件系统。当你发送电子邮件时，你把邮件放在收件人的邮箱里。你不会一直等到别人读完它。

这些是演员模型和 Akka 框架的基础。

Actor 封装了状态和行为。然而，与 OOP 的封装不同，actors 根本不公开它们的状态和行为。演员之间唯一的交流方式就是交换信息。收到的信息被放入邮箱，并按照先入先出的顺序进行整理。这里有一个用 Akka 和 Scala 重新制作的样本。

```
case class Withdraw(deduct: Int)
class ReplicaActor extends Actor {
  var x = 10;
  def receive: Receive = {
    case Withdraw(deduct) => val r = withdraw(deduct)
  }
}
class BossActor extends Actor {
  var replica = context.actorOf(Props[ReplicaActor])
  replica ! Withdraw(6)
  replica ! Withdraw(9)   
}
```

我们有一个执行工作的 ReplicaActor 和命令副本的 BossActor。首先，请注意！签字或者*告诉*。这是一个参与者向另一个参与者异步发送消息的两种方法之一(另一种是 *ask* )。*特别告诉*不要等待回复。因此，老板告诉复制品做两个撤回订单，并立即离开。这些消息到达副本的 *receive* 中，其中每一条都被弹出并与相应的处理程序匹配。在这种情况下， *Withdraw* 执行上一个示例中的 *withdraw* 方法，并从状态 x 中扣除请求的金额。

那么我们在这里得到了什么？首先，我们不再需要担心锁定和使用原子/并发类型。Actor 的封装和排队机制已经保证了线程安全。因为线程只是丢弃消息并返回，所以不再需要等待。通过*询问*或*告知*可以在稍后交付结果。很简单也很理智。

Akka 基于 JVM，有 Scala 和 Java 两种版本。虽然这篇文章不是在讨论 Java 还是 Scala，但是 Scala 的模式匹配和函数式编程在管理 Actor 的数据消息传递方面非常有用。至少它可以通过避免 Java 的括号和分号来帮助你编写更短的代码。