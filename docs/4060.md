# QuickSelect:用代码示例解释的快速选择算法

> 原文：<https://www.freecodecamp.org/news/quickselect-algorithm-explained-with-examples/>

## 什么是 QuickSelect？

QuickSelect 是一种选择算法，用于在未排序的列表中查找第 K 个最小的元素。

### 算法解释道

找到主元(将列表分为两部分的位置:左边的每个元素都小于主元，右边的每个元素都大于主元)后，算法只对包含第 k 个最小元素的部分进行递归。

如果分区元素(pivot)的索引大于 k，则该算法对左侧部分进行递归。如果索引(pivot)与 k 相同，那么我们已经找到了第 k 个最小的元素并返回它。如果 index 小于 k，则该算法针对右侧部分进行递归。

#### 选择伪代码

```
Input : List, left is first position of list, right is last position of list and k is k-th smallest element.
Output : A new list is partitioned.

quickSelect(list, left, right, k)

   if left = right
      return list[left]

   // Select a pivotIndex between left and right

   pivotIndex := partition(list, left, right, 
                                  pivotIndex)
   if k = pivotIndex
      return list[k]
   else if k < pivotIndex
      right := pivotIndex - 1
   else
      left := pivotIndex + 1 
```

### 划分

分区就是上面说的找支点。(左边的每一个元素都小于主元，右边的每一个元素都大于主元)求划分的主元有两种算法。

*   洛穆托分区
*   霍尔分区

#### 洛穆托分区

该分区选择通常是数组中最后一个元素的枢轴。该算法在使用另一个索引 j 扫描数组时保持索引 I，使得元素 l0 到 I(包括端点)小于或等于主元，并且元素 i+1 到 j-1(包括端点)大于主元。

当阵列已经有序时，这种方案退化到`O(n^2)`。

```
algorithm Lomuto(A, lo, hi) is
    pivot := A[hi]
    i := lo    
    for j := lo to hi - 1 do
        if A[j] < pivot then
            if i != j then
                swap A[i] with A[j]
            i := i + 1
    swap A[i] with A[hi]
    return i 
```

#### 霍尔分区

Hoare 使用两个索引，这两个索引从被分区的数组的末端开始，然后朝着彼此移动，直到它们检测到反转:一对元素，一个大于或等于主元，一个小于或等于主元，它们相对于彼此的顺序是错误的。

然后交换反转的元素。当索引满足时，算法停止并返回最终索引。这种算法有许多变体。

```
algorithm Hoare(A, lo, hi) is
    pivot := A[lo]
    i := lo - 1
    j := hi + 1
    loop forever
        do
            i := i + 1
        while A[i] < pivot

        do
            j := j - 1
        while A[j] > pivot

        if i >= j then
            return j

        swap A[i] with A[j] 
```