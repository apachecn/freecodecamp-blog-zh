# 数据结构举例说明——链表

> 原文：<https://www.freecodecamp.org/news/data-structures-explained-with-examples-linked-list/>

就像花环是用花做的一样，链表是由节点组成的。我们称这个特殊花环上的每一朵花为一个节点。并且每个节点指向这个列表中的下一个节点，并且它有数据(这里是花的类型)。

## **类型**

### 单向链表

单链表包含的节点有一个`data`字段和一个`next`字段，指向序列中的下一个节点。可以在单链表上执行的操作有插入、删除和遍历。

```
 head
    |
    |
  +-----+--+      +-----+--+      +-----+------+
  |  1  |o----->  |  2  |o----->  |  3  | NULL |
  +-----+--+      +-----+--+      +-----+------+
```

CPython 的内部实现，帧和计算的变量保存在一个堆栈中。

为此，我们只需向前迭代或获取头部，因此使用了单向链表。

### 双向链表

双向链表包含的节点有`data`字段、`next`字段和另一个链接字段`prev`，指向序列中的前一个节点。

```
 head
           |
           |
  +------+-----+--+    +--+-----+--+       +-----+------+
  |      |     |o------>  |     |o------>  |     |      |
  | NULL |  1  |          |  2  |          |  3  | NULL |
  |      |     |  <------o|     |  <------o|     |      |
  +------+-----+--+    +--+-----+--+       +-----+------+
```

允许你点击后退和前进按钮的浏览器缓存。这里我们需要维护一个双向链表，用`URLs`作为数据字段，允许双向访问。要转到上一个 URL，我们将使用`prev`字段，要转到下一页，我们将使用`next`字段。

### 循环链表

循环链表是一个单链表，其中最后一个节点，`next`字段指向序列中的第一个节点。

```
 head
      |
      |
    +-----+--+      +-----+--+      +-----+--+
    —> | 1 |o-----> | 2 |o----->    | 3 |o----| 
    +-----+--+      +-----+--+      +-----+--+
```

操作系统解决的分时问题。

在分时环境中，操作系统必须维护当前用户的列表，并且必须交替地允许每个用户使用一小部分 CPU 时间，一次一个用户。操作系统将挑选一个用户，让他/她使用少量的 CPU 时间，然后继续移动到下一个用户。

对于这个应用程序，应该没有空指针，除非绝对没有人请求 CPU 时间，即列表为空。

## **基本操作**

### 插入

向列表中添加新元素。

在开头插入:

*   用给定的数据创建一个新节点。
*   将新节点的`next`指向旧节点的`head`。
*   将`head`指向这个新节点。

在中间/末端插入。

在节点 x 后插入。

*   用给定的数据创建一个新节点。
*   将新节点的`next`指向旧节点的`next`。
*   将 X 的`next`指向这个新节点。

****时间复杂度:O(1)****

### 删除

从列表中删除现有元素。

开头删除

*   获取`head`指向的节点作为临时节点。
*   将`head`指向温度的`next`。
*   临时节点使用的空闲内存。

中间/结尾删除。

节点 x 后删除。

*   获取`X`指向的节点作为临时节点。
*   点 X 的`next`指向温度的`next`。
*   临时节点使用的空闲内存。

****时间复杂度:O(1)****

### 横越

遍历列表。

横越

*   将`head`指向的节点作为当前节点。
*   检查 Current 是否不为空并显示它。
*   将 Current 指向 Current 的`next`并移至上述步骤。

****时间复杂度:O(n) //这里 n 是链表的大小****

## **实施**

### **单链表的 C++实现**

```
// Header files
#include <iostream>

struct node
{
    int data;
    struct node *next;
};

// Head pointer always points to first element of the linked list
struct node *head = NULL;
```

#### **打印各节点的数据**

```
// Display the list
void printList()
{
    struct node *ptr = head;

    // Start from the beginning
while(ptr != NULL)
{
    std::cout << ptr->data << " ";
    ptr = ptr->next;
}

std::cout << std::endl;
}
```

#### **在开头插入**

```
// Insert link at the beginning
void insertFirst(int data)
{
    // Create a new node
    struct node *new_node = new struct node;

    new_node->data = data;

// Point it to old head
new_node->next = head;

// Point head to new node
head = new_node;

std::cout << "Inserted successfully" << std::endl;
}
```

#### **开头删除**

```
// Delete first item
void deleteFirst()
{
    // Save reference to head
    struct node *temp = head;

    // Point head to head's next
head = head->next;

// Free memory used by temp
temp = NULL:
delete temp;

std::cout << "Deleted successfully" << std::endl;
}
```

#### **尺寸**

```
// Find no. of nodes in link list
void size()
{
    int length = 0;
    struct node *current;

    for(current = head; current != NULL; current = current->next)
{
    length++;
}

std::cout << "Size of Linked List is " << length << std::endl;
}
```

#### **搜索**

```
// Find node with given data
void find(int data){

    // Start from the head
struct node* current = head;

// If list is empty
if(head == NULL)
{
    std::cout << "List is empty" << std::endl;
    return;
}

// Traverse through list
while(current->data != data){

    // If it is last node
    if(current->next == NULL){
        std::cout << "Not Found" << std::endl;
        return;
    }
    else{
        // Go to next node
        current = current->next;
    }
}

// If data found
std::cout << "Found" << std::endl;
}
```

#### **删除一个节点后**

```
// Delete a node with given data
void del(int data){

    // Start from the first node
struct node* current = head;
struct node* previous = NULL;

// If list is empty
if(head == NULL){
    std::cout << "List is empty" << std::endl;
    return ;
}

// Navigate through list
while(current->data != data){

    // If it is last node
    if(current->next == NULL){
        std::cout << "Element not found" << std::endl;
        return ;
    }
    else {
        // Store reference to current node
        previous = current;
        // Move to next node
        current = current->next;
    }

}

// Found a match, update the node
if(current == head) {
    // Change head to point to next node
    head = head->next;
}
else {
    // Skip the current node
    previous->next = current->next;
}

// Free space used by deleted node
current = NULL;
delete current;
std::cout << "Deleted succesfully" << std::endl;
}
```

### **单链表的 Python 实现**

```
class Node(object):
    # Constructor
    def __init__(self, data=None, next=None):
        self.data = data
        self.next = next

    # Function to get data
def get_data(self):
    return self.data

# Function to get next node
def get_next(self):
    return self.next

# Function to set next field
def set_next(self, new_next):
    self.next = new_next
class LinkedList(object):
    def __init__(self, head=None):
        self.head = head
```

#### **插入**

```
 # Function to insert data
def insert(self, data):
    # new_node is a object of class Node
    new_node = Node(data)
    new_node.set_next(self.head)
    self.head = new_node
    print("Node with data " + str(data) + " is created succesfully")
```

#### **尺寸**

```
 # Function to get size
def size(self):
    current = self.head
    count = 0
    while current:
        count += 1
        current = current.get_next()
    print("Size of link list is " + str(count))
```

#### **搜索**

```
 # Function to search a data
def search(self, data):
    current = self.head
    found = False
    while current and found is False:
        if current.get_data() == data:
            found = True
        else:
            current = current.get_next()
    if current is None:
        print("Node with data " + str(data) + " is not present")
    else:
        print("Node with data " + str(data) + " is found")
```

#### **删除一个节点后**

```
 # Function to delete a node with data
def delete(self, data):
    current = self.head
    previous = None
    found = False
    while current and found is False:
        if current.get_data() == data:
            found = True
        else:
            previous = current
            current = current.get_next()
    if current is None:
        print("Node with data " + str(data) + " is not in list")
    elif previous is None:
        self.head = current.get_next()
        print("Node with data " + str(data) + " is deleted successfully")
    else:
        previous.set_next(current.get_next())
        print("Node with data " + str(data) + " is deleted successfully")
```

****优点****

1.  链表是一个动态的数据结构，它可以在程序运行时增长和收缩，分配和释放内存。
2.  在链表中的任何位置插入和删除节点都很容易实现。

****缺点****

1.  它们比数组使用更多的内存，因为它们的指针(`next`和`prev`)使用内存。
2.  在链表中随机访问是不可能的。我们必须按顺序访问节点。
3.  它比数组更复杂。如果一种语言自动支持数组边界检查，数组会更好地为您服务。

#### **注**

我们必须使用 C 中的 free()和 C++中的 delete 来释放被删除的节点所使用的空间，而在 Python 和 Java 中，自由空间是由垃圾收集器自动收集的。