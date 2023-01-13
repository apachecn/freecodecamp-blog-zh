# Java 中的优先级队列举例说明

> 原文：<https://www.freecodecamp.org/news/priority-queue-implementation-in-java/>

优先级队列在实际应用中经常使用。在本文中，我们将学习什么是优先级队列，以及如何在 Java 中使用它们。

在我们讨论什么是优先级队列之前，让我们看看什么是常规队列。

常规队列遵循先进先出(FIFO)结构。这意味着，如果 3 条消息(m1、m2 和 m3)按此顺序进入队列，那么它们将以完全相同的顺序从队列中出来。

## 为什么我们需要排队？

假设我们有速度极快的数据生产者(例如，当用户点击网页时)。但是我们希望以后以较慢的速度消费这些数据。

在这种情况下，生产者会将所有消息推入队列，而消费者稍后会以较慢的速度使用队列中的这些消息。

## 什么是优先级队列？

如前所述，常规队列具有先进先出的结构。但是在某些情况下，我们希望根据消息的优先级而不是消息进入队列的时间来处理队列中的消息。

优先级队列帮助使用者首先使用优先级较高的消息，然后使用优先级较低的消息。

## Java 中的优先级队列

现在让我们看一些实际的 Java 代码，它将向我们展示如何使用优先级队列。

### 自然排序的优先级队列

下面的代码展示了如何为字符串创建一个简单的优先级队列

```
private static void testStringsNaturalOrdering() {
        Queue<String> testStringsPQ = new PriorityQueue<>();
        testStringsPQ.add("abcd");
        testStringsPQ.add("1234");
        testStringsPQ.add("23bc");
        testStringsPQ.add("zzxx");
        testStringsPQ.add("abxy");

        System.out.println("Strings Stored in Natural Ordering in a Priority Queue\n");
        while (!testStringsPQ.isEmpty()) {
            System.out.println(testStringsPQ.poll());
        }
    }
```

第一行告诉我们我们正在创建一个优先级队列:

```
Queue<String> testStringsPQ = new PriorityQueue<>();
```

java.util 包中提供了 PriorityQueue。

接下来，我们将随机添加 5 个字符串到优先级队列中。为此，我们使用如下所示的 **add()** 函数:

```
testStringsPQ.add("abcd");
testStringsPQ.add("1234");
testStringsPQ.add("23bc");
testStringsPQ.add("zzxx");
testStringsPQ.add("abxy");
```

为了从队列中获取最新的项目，我们使用了如下所示的 **poll()** 函数:

```
testStringsPQ.poll()
```

**poll()** 会给我们最新的项目，并将其从队列中删除。如果我们想获得队列中的最新条目而不删除它，我们可以使用 **peek()** 函数:

```
testStringsPQ.peek()
```

最后，我们使用 poll()函数打印出队列中的所有元素，如下所示:

```
while (!testStringsPQ.isEmpty()) {
   System.out.println(testStringsPQ.poll());
}
```

下面是上述程序的输出:

```
1234
23bc
abcd
abxy
zzxx
```

因为我们没有告诉优先级队列如何区分其内容的优先级，所以它使用了默认的自然排序。在这种情况下，它按照字符串的升序给我们返回数据。这与项目添加到队列的顺序不同。

### 定制怎么样？

这也是可能的，我们可以借助**比较器来实现。**

现在让我们创建一个整数优先级队列。但是这次让我们按照值的降序来得到结果。

为了实现这一点，首先我们需要创建一个整数比较器:

```
 static class CustomIntegerComparator implements Comparator<Integer> {

        @Override
        public int compare(Integer o1, Integer o2) {
            return o1 < o2 ? 1 : -1;
        }
    }
```

为了创建一个比较器，我们实现了**比较器**接口并覆盖了**比较**方法。

通过使用 **o1 < o2？1 : -1** 我们将按降序得到结果。如果我们用了 **o1 > o2？1 : -1，**那么我们将得到升序排列的结果

现在我们有了比较器，我们需要将这个比较器添加到优先级队列中。我们可以这样做:

```
Queue<Integer> testIntegersPQ = new PriorityQueue<>(new CustomIntegerComparator());
```

下面是将元素添加到优先级队列并打印它们的剩余代码:

```
 testIntegersPQ.add(11);
        testIntegersPQ.add(5);
        testIntegersPQ.add(-1);
        testIntegersPQ.add(12);
        testIntegersPQ.add(6);

        System.out.println("Integers stored in reverse order of priority in a Priority Queue\n");
        while (!testIntegersPQ.isEmpty()) {
            System.out.println(testIntegersPQ.poll());
        }
```

上述程序的输出如下所示:

```
12
11
6
5
-1
```

我们可以看到比较器很好地完成了它的工作。现在优先级队列给我们降序排列的整数。

### 带有 Java 对象的优先级队列

到目前为止，我们已经看到了如何在优先级队列中使用字符串和整数。

在现实生活的应用程序中，我们通常使用定制 Java 对象的优先级队列。

让我们首先创建一个名为 CustomerOrder 的类，它用于存储客户订单的详细信息:

```
public class CustomerOrder implements Comparable<CustomerOrder> {
    private int orderId;
    private double orderAmount;
    private String customerName;

    public CustomerOrder(int orderId, double orderAmount, String customerName) {
        this.orderId = orderId;
        this.orderAmount = orderAmount;
        this.customerName = customerName;
    }

    @Override
    public int compareTo(CustomerOrder o) {
        return o.orderId > this.orderId ? 1 : -1;
    }

    @Override
    public String toString() {
        return "orderId:" + this.orderId + ", orderAmount:" + this.orderAmount + ", customerName:" + customerName;
    }

    public double getOrderAmount() {
        return orderAmount;
    }
}
```

这是一个简单的 Java 类，用于存储客户订单。这个类实现了 **comparable 接口，**,这样我们就可以决定这个对象在优先级队列中的排序依据。

排序由上面代码中的 **compareTo** 函数决定。这条线 **o.orderId > this.orderId？1 : -1** 指示根据 **orderId** 字段的降序对订单进行排序

下面是为 CustomerOrder 对象创建优先级队列的代码:

```
CustomerOrder c1 = new CustomerOrder(1, 100.0, "customer1");
CustomerOrder c2 = new CustomerOrder(3, 50.0, "customer3");
CustomerOrder c3 = new CustomerOrder(2, 300.0, "customer2");

Queue<CustomerOrder> customerOrders = new PriorityQueue<>();
customerOrders.add(c1);
customerOrders.add(c2);
customerOrders.add(c3);
while (!customerOrders.isEmpty()) {
	System.out.println(customerOrders.poll());
}
```

在上面的代码中，创建了三个客户订单，并将其添加到优先级队列中。

当我们运行这段代码时，我们得到以下输出:

```
orderId:3, orderAmount:50.0, customerName:customer3
orderId:2, orderAmount:300.0, customerName:customer2
orderId:1, orderAmount:100.0, customerName:customer1
```

不出所料，结果按照 **orderId** 的降序排列。

### 如果我们想根据订单量进行优先级排序呢？

这又是一个真实的生活场景。假设默认情况下，CustomerOrder 对象按 orderId 排列优先级。但是，我们需要一种方法来根据订单量进行优先级排序。

您可能会立即想到，我们可以将 **CustomerOrder c** 类中的 **compareTo** 函数修改为基于订单金额的订单。

但是**customer order c**class 可能会在应用程序的多个地方使用，如果我们直接修改 **compareTo** 函数，它会干扰应用程序的其余部分。

这个问题的解决方案非常简单:我们可以为 CustomerOrder 类创建一个新的自定义比较器，并将其与优先级队列一起使用

下面是自定义比较器的代码:

```
 static class CustomerOrderComparator implements Comparator<CustomerOrder> {

        @Override
        public int compare(CustomerOrder o1, CustomerOrder o2)
        {
            return o1.getOrderAmount() < o2.getOrderAmount() ? 1 : -1;
        }
    }
```

这与我们之前看到的自定义整数比较器非常相似。

行`o1.getOrderAmount() < o2.getOrderAmount() ? 1 : -1;`表示我们需要根据**订单金额**的降序排列优先级。

下面是创建优先级队列的代码:

```
 CustomerOrder c1 = new CustomerOrder(1, 100.0, "customer1");
        CustomerOrder c2 = new CustomerOrder(3, 50.0, "customer3");
        CustomerOrder c3 = new CustomerOrder(2, 300.0, "customer2");
        Queue<CustomerOrder> customerOrders = new PriorityQueue<>(new CustomerOrderComparator());
        customerOrders.add(c1);
        customerOrders.add(c2);
        customerOrders.add(c3);
        while (!customerOrders.isEmpty()) {
            System.out.println(customerOrders.poll());
        }
```

在上面的代码中，我们在下面的代码行中将比较器传递给优先级队列:

```
Queue<CustomerOrder> customerOrders = new PriorityQueue<>(new CustomerOrderComparator());
```

下面是我们运行这段代码的结果:

```
orderId:2, orderAmount:300.0, customerName:customer2
orderId:1, orderAmount:100.0, customerName:customer1
orderId:3, orderAmount:50.0, customerName:customer3
```

我们可以看到数据是按照订单金额的降序排列的。

## 密码

本文讨论的所有代码都可以在 [this GitHub repo](https://github.com/aditya-sridhar/java-priority-queue-example) 中找到。

## 恭喜**？**

您现在知道了如何在 Java 中使用优先级队列。

## **关于作者**

我热爱技术，关注该领域的进步。我也喜欢用我的技术知识帮助别人。

请随时通过我的 LinkedIn 账户与我联系[https://www.linkedin.com/in/aditya1811/](https://www.linkedin.com/in/aditya1811/)

你也可以在推特上关注我[https://twitter.com/adityasridhar18](https://twitter.com/adityasridhar18)

欢迎在我的 adityasridhar.com 博客上阅读更多我的文章。