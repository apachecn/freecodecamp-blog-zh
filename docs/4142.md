# 二分搜索法树:BST 举例说明

> 原文：<https://www.freecodecamp.org/news/binary-search-trees-bst-explained-with-examples/>

## 什么是二叉查找树？

树是由节点组成的数据结构，具有以下特征:

1.  每个树的顶部都有一个包含某些值(可以是任何数据类型)的根节点(也称为父节点)。
2.  根节点有零个或多个子节点。
3.  每个子节点有零个或多个子节点，依此类推。这将在树中创建一个子树。每个节点都有自己的子树，由它的孩子和他们的孩子组成，等等。这意味着每个节点本身都可以是一棵树。

二叉查找树(BST)增加了这两个特征:

1.  每个节点最多有两个子节点。
2.  对于每个节点，其左侧后代节点的值小于当前节点的值，而当前节点的值又小于右侧后代节点(如果有)的值。

BST 建立在[二分搜索法](https://guide.freecodecamp.org/algorithms/search-algorithms/binary-search)算法的基础上，该算法允许快速查找、插入和删除节点。它们的设置方式意味着，平均而言，每次比较允许操作跳过大约一半的树，因此每次查找、插入或删除所花费的时间与存储在树中的项目数量的对数`O(log n)`成比例。然而，有时最糟糕的情况会发生，当树不平衡时，这三个函数的时间复杂度都是`O(n)`。这就是为什么自平衡树(AVL，红黑树等。)比基本的 BST 有效得多。

**最坏情况示例:**当您添加的节点的*总是*大于之前的节点(其父节点)时，会发生这种情况；当您添加的节点的值总是小于其父节点时，也会发生这种情况。

### BST 上的基本操作

*   创建:创建空树。
*   插入:在树中插入一个节点。
*   搜索:在树中搜索节点。
*   删除:从树中删除一个节点。
*   Inorder:树的有序遍历。
*   前序:树的前序遍历。
*   后序:树的后序遍历。

#### 创造

最初，创建一个没有任何节点的空树。必须指向根节点的变量/标识符用一个`NULL`值初始化。

#### 搜索

您总是从根节点开始搜索树，并从那里向下搜索。您将每个节点中的数据与您正在寻找的数据进行比较。如果被比较的节点不匹配，那么你要么前进到右边的子节点，要么前进到左边的子节点，这取决于下面比较的结果:如果你正在搜索的节点比你与之比较的节点低，那么你前进到左边的子节点，否则(如果它更大)，你前进到右边的子节点。为什么？因为 BST 是结构化的(按照它的定义)，右边的子元素总是比父元素大，左边的子元素总是比父元素小。

###### 广度优先搜索(BFS)

广度优先搜索是一种用于遍历 BST 的算法。它从根节点开始，以横向方式(从一边到另一边)移动，搜索所需的节点。假设每个节点被访问一次，并且树的大小与搜索的长度直接相关，那么这种类型的搜索可以被描述为 O(n)。

###### 深度优先搜索

使用深度优先搜索方法，我们从根节点开始，沿着单个分支向下搜索。如果沿着那个分支找到了想要的节点，很好，但是如果没有，继续向上搜索未访问的节点。这种类型的搜索也有一个很大的 O(n)符号。

#### 插入

它非常类似于搜索功能。您再次从树的根开始，向下递归，搜索插入新节点的正确位置，与搜索函数中解释的方式相同。如果树中已经存在具有相同值的节点，您可以选择是否插入副本。有些树允许重复，有些不允许。这取决于特定的实现。

#### 删除

当您尝试删除节点时，有三种情况可能发生。如果有，

1.  没有子树(没有孩子):这是最简单的一个。您可以简单地删除节点，不需要任何额外的操作。
2.  一个子树(一个子树):您必须确保在节点被删除后，它的子节点连接到被删除节点的父节点。
3.  两个子树(两个子树):您必须找到想要删除的节点，并用它的有序后继节点(右边子树中最左边的节点)替换它。

创建一棵树的时间复杂度为`O(1)`。搜索、插入或删除节点的时间复杂度取决于树的高度`h`，因此在偏斜树的情况下，最坏的情况是`O(h)`。

#### 节点的前任

前趋节点可以被描述为紧接在当前节点之前的节点。要查找当前节点的前任，请查看左侧子树中最右边/最大的叶节点。

#### 节点的后继者

后继节点可以描述为紧接在当前节点之后的节点。要查找当前节点的后继节点，请查看右边子树中最左边/最小的叶节点。

### 特殊类型的 BT

*   许多
*   红黑树
*   B 树
*   八字树
*   N-ary tree
*   基数树

### 运行时间

**数据结构:BST**

*   最差情况性能:`O(n)`
*   最佳性能:`O(1)`
*   平均表现:`O(log n)`
*   最坏情况空间复杂度:`O(1)`

其中`n`是 BST 中的节点数。最差情况是 O(n ),因为 BST 可能不平衡。

### BST 的实现

下面是一个包含一些数据的 BST 节点的定义，引用了它的左右子节点。

```
struct node {
   int data;
   struct node *leftChild;
   struct node *rightChild;
}; 
```

#### 搜索操作

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

#### 插入操作

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

#### 删除操作

```
void deleteNode(struct node* root, int data){

    if (root == NULL) root=tempnode; 

    if (data < root->key) 
        root->left = deleteNode(root->left, key); 

    else if (key > root->key) 
        root->right = deleteNode(root->right, key); 

    else
    { 
        if (root->left == NULL) 
        { 
            struct node *temp = root->right; 
            free(root); 
            return temp; 
        } 
        else if (root->right == NULL) 
        { 
            struct node *temp = root->left; 
            free(root); 
            return temp; 
        } 

        struct node* temp = minValueNode(root->right); 

        root->key = temp->key; 

        root->right = deleteNode(root->right, temp->key); 
    } 
    return root; 

} 
```

二分搜索法树(BST)也让我们快速访问前任和继任者。前趋节点可以被描述为紧接在当前节点之前的节点。

*   要查找当前节点的前任，请查看左侧子树中最右边/最大的叶节点。后继节点可以描述为紧接在当前节点之后的节点。
*   要查找当前节点的后继节点，请查看右边子树中最左边/最小的叶节点。

### 让我们看几个在树上操作的过程。

由于树是递归定义的，所以编写对自身递归的树进行操作的例程是很常见的。

举个例子，如果我们想计算一棵树的高度，也就是一个根节点的高度，我们可以通过遍历树来递归地计算。所以我们可以说:

*   例如，如果我们有一棵零树，那么它的高度是 0。
*   否则，我们就是 1 加上左子树和右子树的最大值。
*   如果我们看一片叶子，它的高度是 1，因为左边孩子的高度是 0，右边孩子的高度也是 0。所以最大值是 0，然后 1 加 0。

#### 高度(树)算法

```
if tree = nil:
    return 0
return 1 + Max(Height(tree.left),Height(tree.right)) 
```

#### 以下是用 C++编写的代码

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
*   否则，我们将得到左侧子节点的数量加上 1，再加上右侧子节点的数量。所以 1 加上左边树的大小加上右边树的大小。

#### 尺寸(树)算法

```
if tree = nil
    return 0
return 1 + Size(tree.left) + Size(tree.right) 
```

#### 以下是用 C++编写的代码

```
int treeSize(struct node* node)
{
    if (node==NULL)
        return 0;
    else
        return 1+(treeSize(node->left) + treeSize(node->right));
} 
```

#### 横越

在二叉查找树上有三种典型的穿越方式。所有这些遍历都有一个共同的方式来遍历树的节点。

##### 按顺序

这个遍历首先遍历根节点的左子树，然后访问当前节点，接着是当前节点的右子树。代码也代表了基本情况，即空树也是二叉查找树。

```
void inOrder(struct node* root) {
        // Base case
        if (root == null) {
                return;
        }
        // Travel the left sub-tree first.
        inOrder(root.left);
        // Print the current node value
        printf("%d ", root.data);
        // Travel the right sub-tree next.
        inOrder(root.right);
} 
```

### 预购

这个遍历首先访问当前节点值，然后分别遍历左边和右边的子树。

```
void preOrder(struct node* root) {
        if (root == null) {
                return;
        }
        // Print the current node value
        printf("%d ", root.data);
        // Travel the left sub-tree first.
        preOrder(root.left);
        // Travel the right sub-tree next.
        preOrder(root.right);
} 
```

### 后期订单

这个遍历把根值放在最后，先遍历左右子树。左右子树的相对顺序保持不变。在所有上述遍历中，只有根的位置发生变化。

```
void postOrder(struct node* root) {
        if (root == null) {
                return;
        }
        // Travel the left sub-tree first.
        postOrder(root.left);
        // Travel the right sub-tree next.
        postOrder(root.right);
        // Print the current node value
        printf("%d ", root.data);
} 
```

### freeCodeCamp YouTube 频道上的相关视频

[https://www.youtube.com/embed/5cU1ILGy6dM?feature=oembed](https://www.youtube.com/embed/5cU1ILGy6dM?feature=oembed)

## 和二叉查找树:穿越和高度

[https://www.youtube.com/embed/Aagf3RyK3Lw?feature=oembed](https://www.youtube.com/embed/Aagf3RyK3Lw?feature=oembed)

### 以下是常见的二叉树类型:

完全二叉树/严格二叉树:如果每个节点恰好有 0 或 2 个子节点，则二叉树是完全或严格的。

```
 18
         /   \
       /       \  
     15         30  
    /  \       /  \
  40    50   100   40 
```

在完全二叉树中，叶节点数等于内部节点数加 1。

完全二叉树:一棵二叉树是完全二叉树，如果除了可能的最后一层，所有层都被完全填充，并且最后一层尽可能地保留了所有的键

```
 18
         /    \
       /        \  
     15         30  
    /  \       /  \
  40    50   100   40
 /  \   /
8    7 9 
```

完全二叉树一棵二叉树是完全二叉树，其中所有的内部节点都有两个子节点，并且所有的叶子都在同一层。

```
 18
         /  \
       /      \  
     15        30  
    /  \      /  \
  40    50  100   40 
```

### 扩充 BST

有时我们需要用传统的数据结构存储一些额外的信息，以使我们的任务更容易。例如，考虑这样一个场景，你应该在一个集合中找到第 I 个最小的数。您可以在这里使用蛮力，但是我们可以通过增加一棵红黑树或任何自平衡树(其中 n 是集合中元素的数量)来将问题的复杂性减少到`O(lg n)`。我们也可以计算任何元素在`O(lg n)`时间内的秩。让我们考虑这样一种情况，我们正在扩充一棵红黑树来存储所需的附加信息。除了通常的属性之外，我们可以在以 x 为根的子树中存储内部节点的数量(以 x 为根的子树的大小，包括节点本身)。设 x 是树的任意节点。

`x.size = x.left.size + x.right.size + 1`

在扩充树的时候，我们应该记住，我们应该能够维护增加的信息，以及在`O(lg n)`时间内进行其他操作，如插入、删除、更新。

因为我们知道 x.left.size 的值将给出在树的顺序遍历中进行 x 的节点数。因此，`x.left.size + 1`是 x 在以 x 为根的子树中的秩。