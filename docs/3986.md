# 二叉查找树数据结构举例说明

> 原文：<https://www.freecodecamp.org/news/binary-search-tree-what-is-it/>

树是由节点组成的数据结构，具有以下特征:

1.  每棵树都有一个具有某个值的根节点(在顶部)。
2.  根节点有零个或多个子节点。
3.  每个子节点有零个或多个子节点，依此类推。这将在树中创建一个子树。每个节点都有自己的子树，由他的孩子和他们的孩子组成，等等。这意味着每个节点本身都可以是一棵树。

二叉查找树(BST)增加了这两个特征:

1.  每个节点最多有两个子节点。
2.  对于每个节点，其左侧后代节点的值小于当前节点的值，而当前节点的值又小于右侧后代节点(如果有)的值。

BST 建立在二分搜索法 T2 算法的基础上，允许快速查找、插入和删除节点。它们的设置方式意味着，平均而言，每次比较允许操作跳过大约一半的树，因此每次查找、插入或删除所花费的时间与存储在树中的项目数量的对数`O(log n)`成比例。

然而，有时最糟糕的情况会发生，当树不平衡时，所有三个函数的时间复杂度都是`O(n)`。这就是为什么自平衡树(AVL，红黑树等。)比基本的 BST 有效得多。

****最坏情况示例:**** 当您添加的节点*总是*大于之前的节点(它的父节点)时，会发生这种情况，当您添加的节点值总是小于它们的父节点时，也会发生这种情况。

## **BST 上的基本操作**

*   创建:创建空树。
*   插入:在树中插入一个节点。
*   搜索:在树中搜索节点。
*   删除:从树中删除一个节点。

### 创造

最初，创建一个没有任何节点的空树。必须指向根节点的变量/标识符用一个`NULL`值初始化。

### 搜索

您总是从根节点开始搜索树，并从那里向下搜索。您将每个节点中的数据与您正在寻找的数据进行比较。如果被比较的节点不匹配，那么你要么前进到右边的子节点，要么前进到左边的子节点，这取决于下面比较的结果:如果你正在搜索的节点比你与之比较的节点低，那么你前进到左边的子节点，否则(如果它更大)，你前进到右边的子节点。为什么？因为 BST 是结构化的(按照它的定义)，右边的子元素总是比父元素大，左边的子元素总是比父元素小。

### 插入

它非常类似于搜索功能。您再次从树的根开始，向下递归，搜索插入新节点的正确位置，与搜索函数中解释的方式相同。如果树中已经存在具有相同值的节点，您可以选择是否插入副本。有些树允许重复，有些不允许。这取决于特定的实现。

### 删除

当您尝试删除节点时，有三种情况可能发生。如果有，

1.  没有子树(没有孩子):这是最简单的一个。您可以简单地删除节点，不需要任何额外的操作。
2.  一个子树(一个子树):您必须确保在节点被删除后，它的子节点连接到被删除节点的父节点。
3.  两个子树(两个子树):您必须找到要删除的节点，并用它的后继节点(右边子树中的 letfmost 节点)替换它。

创建一棵树的时间复杂度为`O(1)`。搜索、插入或删除节点的时间复杂度取决于树的高度`h`，所以最坏的情况是`O(h)`。

### 节点的前任

前趋节点可以被描述为紧接在当前节点之前的节点。要查找当前节点的前任，请查看左侧子树中最右边/最大的叶节点。

### 节点的后继者

后继节点可以描述为紧接在当前节点之后的节点。要查找当前节点的后继节点，请查看右边子树中最左边/最小的叶节点。

## **特殊类型的 BT**

*   许多
*   红黑树
*   B 树
*   八字树
*   N-ary tree
*   基数树

## 运行时间

### ****数据结构:数组****

*   最差情况性能:`O(log n)`
*   最佳性能:`O(1)`
*   平均表现:`O(log n)`
*   最坏情况空间复杂度:`O(1)`

其中`n`是 BST 中的节点数。

## BST 的实现

这里有一个 BST 节点的定义，它有一些数据，引用它的左右子节点。

```
struct node {
   int data;
   struct node *leftChild;
   struct node *rightChild;
};
```

### 搜索操作

每当要搜索一个元素时，就从根节点开始搜索。然后，如果数据小于键值，则在左子树中搜索元素。否则，在右边的子树中搜索元素。对每个节点遵循相同的算法。

```
struct node* search(int data){
   struct node *current = root;
   printf("Visiting elements: ");

   while(current->data != data){

      if(current != NULL) {
         printf("%d ",current->data);

         //go to left tree
         if(current->data > data){
            current = current->leftChild;
         }//else go to right tree
         else {                
            current = current->rightChild;
         }

         //not found
         if(current == NULL){
            return NULL;
         }
      }			
   }
   return current;
}
```

### 插入操作

每当要插入一个元素时，首先确定它的正确位置。从根节点开始搜索，然后如果数据小于键值，则在左子树中搜索空位置并插入数据。否则，在右边的子树中搜索空的位置并插入数据。

```
void insert(int data) {
   struct node *tempNode = (struct node*) malloc(sizeof(struct node));
   struct node *current;
   struct node *parent;

   tempNode->data = data;
   tempNode->leftChild = NULL;
   tempNode->rightChild = NULL;

   //if tree is empty
   if(root == NULL) {
      root = tempNode;
   } else {
      current = root;
      parent = NULL;

      while(1) {                
         parent = current;

         //go to left of the tree
         if(data < parent->data) {
            current = current->leftChild;                
            //insert to the left

            if(current == NULL) {
               parent->leftChild = tempNode;
               return;
            }
         }//go to right of the tree
         else {
            current = current->rightChild;

            //insert to the right
            if(current == NULL) {
               parent->rightChild = tempNode;
               return;
            }
         }
      }            
   }
} 
```

二分搜索法树(BST)也让我们快速访问前任和继任者。前趋节点可以被描述为紧接在当前节点之前的节点。

*   要查找当前节点的前任，请查看左侧子树中最右边/最大的叶节点。后继节点可以描述为紧接在当前节点之后的节点。
*   要查找当前节点的后继节点，请查看右边子树中最左边/最小的叶节点。

## 让我们看几个在树上操作的过程。

由于树是递归定义的，所以编写对自身递归的树进行操作的例程是很常见的。

举个例子，如果我们想计算一棵树的高度，也就是一个根节点的高度，我们可以通过遍历树来递归地计算。所以我们可以说:

*   例如，如果我们有一棵零树，那么它的高度是 0。
*   否则，我们就是 1 加上左子树和右子树的最大值。

如果我们看一片叶子，它的高度是 1，因为左边孩子的高度是 0，右边孩子的高度也是 0。所以最大值是 0，然后 1 加 0。

### 高度(树)算法

```
if tree = nil:
return 0
return 1 + Max(Height(tree.left),Height(tree.right))
```

### 以下是用 C++编写的代码

```
int maxDepth(struct node* node)
{
    if (node==NULL)
        return 0;
   else
   {
       int rDepth = maxDepth(node->right);
       int lDepth = maxDepth(node->left);

       if (lDepth > rDepth)
       {
           return(lDepth+1);
       }
       else
       {
            return(rDepth+1);
       }
   }
} 
```

我们还可以计算一棵树的大小，也就是节点的数量。

*   同样，如果我们有一棵零树，我们有零个节点。

否则，我们将得到左侧子节点的数量加上 1，再加上右侧子节点的数量。所以 1 加上左边树的大小加上右边树的大小。

### 尺寸(树)算法

```
if tree = nil
return 0
return 1 + Size(tree.left) + Size(tree.right)
```

### 以下是用 C++编写的代码

```
int treeSize(struct node* node)
{
    if (node==NULL)
        return 0;
    else
        return 1+(treeSize(node->left) + treeSize(node->right));
}
```

### **freeCodeCamp YouTube 频道相关视频**

*   [二叉查找树](https://youtu.be/5cU1ILGy6dM)
*   [二叉查找树:遍历与高度](https://youtu.be/Aagf3RyK3Lw)

## 以下是常见的二叉树类型:

完全二叉树/严格二叉树:如果每个节点只有 0 或 2 个子节点，则二叉树是完全的或严格的。

```
 18
       /       \  
     15         30  
    /  \        /  \
  40    50    100   40
```

在完全二叉树中，叶节点数等于内部节点数加 1。

完全二叉树:一棵二叉树是完全二叉树，如果除了可能的最后一层，所有层都被完全填充，并且最后一层尽可能地保留了所有的键

```
 18
       /       \  
     15         30  
    /  \        /  \
  40    50    100   40
 /  \   /
8   7  9 
```