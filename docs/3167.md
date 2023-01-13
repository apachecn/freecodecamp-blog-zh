# 如何在 JavaScript 中实现链表

> 原文：<https://www.freecodecamp.org/news/implementing-a-linked-list-in-javascript/>

如果你正在学习数据结构，链表是你应该知道的一种数据结构。如果您不真正理解它或者它是如何在 JavaScript 中实现的，这篇文章可以帮助您。

在本文中，我们将讨论什么是链表，它与数组有何不同，以及如何用 JavaScript 实现它。让我们开始吧。

## 什么是链表？

链表是一种类似数组的线性数据结构。但是，与数组不同，元素并不存储在特定的内存位置或索引中。相反，每个元素都是一个单独的对象，包含一个指向列表中下一个对象的指针或链接。

每个元素(通常称为节点)包含两项:存储的数据和到下一个节点的链接。数据可以是任何有效的数据类型。您可以在下图中看到这一点。

![Image of a linked list](img/2a970004d1230c7ffff832153def00e6.png)

链表的入口点叫做头部。head 是对链表中第一个节点的引用。列表中的最后一个节点指向 null。如果列表为空，则头部为空引用。

在 JavaScript 中，链表看起来像这样:

```
const list = {
    head: {
        value: 6
        next: {
            value: 10                                             
            next: {
                value: 12
                next: {
                    value: 3
                    next: null	
                    }
                }
            }
        }
    }
};
```

## 链表的一个优点

*   可以很容易地从链表中删除或添加节点，而无需重新组织整个数据结构。这是它优于数组的一个优势。

## 链表的缺点

*   链表中的搜索操作很慢。与数组不同，不允许随机访问数据元素。从第一个节点开始按顺序访问节点。
*   由于指针的存储，它比数组使用更多的内存。

## 链接列表的类型

有三种类型的链表:

*   **单链表**:每个节点只包含一个指向下一个节点的指针。这是我们到目前为止一直在谈论的。
*   **双向链表**:每个节点包含两个指针，一个指向下一个节点，一个指向上一个节点。
*   **循环链表**:循环链表是链表的一种变体，其中最后一个节点指向第一个节点或它之前的任何其他节点，从而形成一个循环。

## 在 JavaScript 中实现列表节点

如前所述，列表节点包含两项:数据和指向下一个节点的指针。我们可以用 JavaScript 实现一个列表节点，如下所示:

```
class ListNode {
    constructor(data) {
        this.data = data
        this.next = null                
    }
}
```

## 用 JavaScript 实现一个链表

下面的代码展示了一个带有构造函数的链表类的实现。注意，如果头节点没有被传递，头被初始化为空。

```
class LinkedList {
    constructor(head = null) {
        this.head = head
    }
}
```

## 把所有的放在一起

让我们用刚刚创建的类创建一个链表。首先，我们创建两个列表节点，`node1`和`node2`以及一个从节点 1 指向节点 2 的指针。

```
let node1 = new ListNode(2)
let node2 = new ListNode(5)
node1.next = node2
```

接下来，我们将用`node1`创建一个链表。

```
let list = new LinkedList(node1)
```

让我们尝试访问刚刚创建的列表中的节点。

```
console.log(list.head.next.data) //returns 5
```

## 一些链接列表方法

接下来，我们将为链表实现四个帮助器方法。它们是:

1.  大小()
2.  清除()
3.  getLast()
4.  getFirst()

### 1.大小()

这个方法返回链表中节点的数量。

```
size() {
    let count = 0; 
    let node = this.head;
    while (node) {
        count++;
        node = node.next
    }
    return count;
} 
```

### 2.清除()

这个方法清空列表。

```
clear() {
    this.head = null;
}
```

### 3\. getLast()

这个方法返回链表的最后一个节点。

```
getLast() {
    let lastNode = this.head;
    if (lastNode) {
        while (lastNode.next) {
            lastNode = lastNode.next
        }
    }
    return lastNode
}
```

### 4.getFirst()

这个方法返回链表的第一个节点。

```
getFirst() {
    return this.head;
}
```

## 摘要

在本文中，我们讨论了什么是链表以及如何在 JavaScript 中实现它。我们还讨论了不同类型的链表以及它们的优缺点。

我希望你喜欢读它。

当我发表新文章时，想得到通知吗？[点击这里](https://mailchi.mp/69ea601a3f64/join-sarahs-mailing-list)。