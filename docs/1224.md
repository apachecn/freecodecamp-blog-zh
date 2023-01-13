# 搜索算法——线性搜索和二分搜索法码实现及复杂性分析

> 原文：<https://www.freecodecamp.org/news/search-algorithms-linear-and-binary-search-explained/>

搜索算法是一个基本的计算机科学概念，你作为一个开发者应该理解。它们通过使用一步一步的方法在数据集合中定位特定数据来工作。

在本文中，我们将通过查看搜索算法在 Java 和 Python 中的实现来了解它们是如何工作的。

## 什么是搜索算法？

根据维基百科，搜索算法是:

> ***解决搜索问题的任何算法，即检索存储在某种数据结构内的信息，或者在某个问题域的搜索空间中计算出来的，或者具有离散值或者连续值的信息。***

搜索算法被设计为从存储元素的任何数据结构中检查或检索元素。他们在搜索空间中搜索目标(关键字)。

## 搜索算法的类型

在这篇文章中，我们将讨论两种重要的搜索算法:

1.  **线性或顺序搜索**
2.  **二分搜索法**

让我们用例子、代码实现和时间复杂度分析来详细讨论这两个。

## 线性或顺序搜索

该算法的工作方式是从一端开始依次遍历整个数组或列表，直到找到目标元素。如果找到该元素，则返回其索引，否则为-1。

现在让我们看一个例子，并试着理解它是如何工作的:

```
arr = [2, 12, 15, 11, 7, 19, 45]
```

假设我们要搜索的目标元素是`7`。

### 线性或顺序搜索方法

*   从索引 0 开始，将每个元素与目标进行比较
*   如果发现目标等于元素，则返回其索引
*   如果没有找到目标，返回-1

### **代码实现**

现在让我们看看如何用几种不同的编程语言实现这种类型的搜索算法。

#### Java 中的线性或顺序搜索

```
package algorithms.searching;

public class LinearSearch {
    public static void main(String[] args) {
        int[] nums = {2, 12, 15, 11, 7, 19, 45};
        int target = 7;
        System.out.println(search(nums, target));

    }

    static int search(int[] nums, int target) {
        for (int index = 0; index < nums.length; index++) {
            if (nums[index] == target) {
                return index;
            }
        }
        return -1;
    }
} 
```

#### Python 中的线性或顺序搜索

```
def search(nums, target):
    for i in range(len(nums)):
        if nums[i] == target:
            return i
    return -1

if __name__ == '__main__':
    nums = [2, 12, 15, 11, 7, 19, 45]
    target = 7
    print(search(nums, target))
```

### **时间复杂度分析**

**最好的情况**发生在目标元素是数组的第一个元素的时候。在这种情况下，比较的次数是 1。所以，时间复杂度是`O(1)`。

**平均情况:**平均来说，目标元素会在数组中间的某个地方。在这种情况下，比较的次数将是 N/2。所以，时间复杂度会是`O(N)`(被忽略的常数)。

**最坏的情况**发生在目标元素是数组中的最后一个元素或者不在数组中的时候。在这种情况下，我们必须遍历整个数组，因此比较的次数将是 n。因此，时间复杂度将是`O(N)`。

## 二进位检索

这种类型的搜索算法用于在排序数组中查找包含**的特定值的位置。二分搜索法算法的工作原理是分而治之，它被认为是最好的搜索算法，因为它运行速度更快。**

现在让我们以一个排序数组为例，试着理解它是如何工作的:

```
arr = [2, 12, 15, 17, 27, 29, 45]
```

假设要搜索的目标元素是`17`。

### **接近二分搜索法**

*   将目标元素与数组的中间元素进行比较。
*   如果目标元素大于中间的元素，则在右半部分继续搜索。
*   否则，如果目标元素小于中间值，则在左半部分继续搜索。
*   重复这个过程，直到中间元素等于目标元素，或者目标元素不在数组中
*   如果找到目标元素，则返回其索引，否则返回-1。

### 代码实现

#### 爪哇的二分搜索法

```
package algorithms.searching;

public class BinarySearch {
    public static void main(String[] args) {
        int[] nums = {2, 12, 15, 17, 27, 29, 45};
        int target = 17;
        System.out.println(search(nums, target));
    }

    static int search(int[] nums, int target) {
        int start = 0;
        int end = nums.length - 1;

        while (start <= end) {
            int mid = start + (end - start) / 2;

            if (nums[mid] > target)
                end = mid - 1;
            else if (nums[mid] < target)
                start = mid + 1;
            else
                return mid;
        }
        return -1;
    }
} 
```

#### 蟒蛇皮二分搜索法

```
def search(nums, target):
    start = 0
    end = len(nums)-1

    while start <= end:
        mid = start + (end-start)//2

        if nums[mid] > target:
            end = mid-1
        elif nums[mid] < target:
            start = mid+1
        else:
            return mid

    return -1

if __name__ == '__main__':
    nums = [2, 12, 15, 17, 27, 29, 45]
    target = 17
    print(search(nums, target))
```

### **时间复杂度分析**

**最好的情况**发生在目标元素是数组的中间元素的时候。在这种情况下，比较的次数是 1。所以，时间复杂度是`O(1)`。

**平均情况:**平均来说，目标元素会在数组的某个地方。所以，时间复杂度会是`O(logN)`。

**最坏的情况**发生在目标元素不在列表中或者远离中间元素的时候。所以，时间复杂度会是`O(logN)`。

### 如何计算时间复杂度:

假设二分搜索法的迭代在 **k** 次迭代后终止。

在每次迭代中，数组被除以二。所以让我们假设在任何迭代中数组的长度是 **N** 。

在**迭代 1，**

```
Length of array = N
```

在**迭代 2** ，

```
Length of array = N/2
```

在**迭代 3** ，

```
Length of array = (N/2)/2 = N/2^2
```

在**迭代 k** 时，

```
Length of array = N/2^k
```

还有，我们知道经过 k 次除法后，数组的**长度变成了 1** :数组长度=**N‖[2]k= 1**=>**N = 2^k**

如果我们在两边都应用一个 log 函数:**log[2](N)= log[2](2^k)**=>**log[2](N)= k log[2](2)**

由于 **(log [a] (a) = 1)**
因此，= > **k = log [2] (N)**

所以现在我们可以看到为什么二分搜索法的时间复杂度是 log [2] (N)。

您也可以使用由 [Dipesh Patil](https://www.linkedin.com/in/dipesh-patil/) - [算法可视化器](https://dipeshpatil.github.io/algorithms-visualiser/#/searching)构建的简单工具来可视化上述两种算法。

## 顺序不可知的二分搜索法

假设，我们必须在一个排序数组中找到一个目标元素。我们知道数组是排序的，但不知道是升序还是降序。

### **阶不可知二分搜索法的方法**

实现类似于二分搜索法，除了我们需要确定数组是按升序还是降序排序。这让我们决定是在数组的左半部分还是右半部分继续搜索。

*   我们首先将目标与中间元素进行比较
*   如果数组按升序排序，目标小于中间元素**或**，数组按降序排序，目标大于中间元素，那么我们通过设置`end=mid-1`继续在数组的下半部分搜索。
*   否则，我们通过设置`start=mid+1`在数组的上半部分执行搜索

我们唯一需要做的就是弄清楚数组是按升序排序还是降序排序。通过比较数组的第一个和最后一个元素，我们可以很容易地找到这一点。

```
if arr[0] < arr[arr.length-1]
    array is sorted in ascending order 
else
    array is sorted in descending order
```

### **代码实现**

#### Java 中顺序不可知的二分搜索法

```
package algorithms.searching;

public class OrderAgnosticBinarySearch {
    public static void main(String[] args) {
        int[] nums1 = {-1, 2, 4, 6, 7, 8, 12, 15, 19, 32, 45, 67, 99};
        int[] nums2 = {99, 67, 45, 32, 19, 15, 12, 8, 7, 6, 4, 2, -1};
        int target = -1;
        System.out.println(search(nums1, target));
        System.out.println(search(nums2, target));
    }

    static int search(int[] arr, int target) {
        int start = 0;
        int end = arr.length - 1;

        boolean isAscending = arr[start] < arr[end];

        while (start <= end) {
            int mid = start + (end - start) / 2;

            if (target == arr[mid])
                return mid;

            if (isAscending) {
                if (target < arr[mid]) {
                    end = mid - 1;
                } else {
                    start = mid + 1;
                }
            } else {
                if (target < arr[mid]) {
                    start = mid + 1;
                } else {
                    end = mid - 1;
                }
            }
        }
        return -1;
    }

} 
```

#### Python 中顺序不可知的二分搜索法

```
def search(nums, target):
    start = 0
    end = len(nums)-1

    is_ascending = nums[start] < nums[end]

    while start <= end:
        mid = start + (end-start)//2

        if target == nums[mid]:
            return mid

        if is_ascending:
            if target < nums[mid]:
                end = mid-1
            else:
                start = mid+1
        else:
            if target < nums[mid]:
                start = mid+1
            else:
                end = mid-1

    return -1

if __name__ == '__main__':
    nums1 = [-1, 2, 4, 6, 7, 8, 12, 15, 19, 32, 45, 67, 99]
    nums2 = [99, 67, 45, 32, 19, 15, 12, 8, 7, 6, 4, 2, -1]
    target = -1
    print(search(nums1, target))
    print(search(nums2, target)) 
```

### **时间复杂度分析**

时间复杂度不会有变化，所以和二分搜索法一样。

# **结论**

在本文中，我们讨论了两个最重要的搜索算法，以及它们在 Python 和 Java 中的代码实现。我们也看了他们的时间复杂度分析。

感谢阅读！

[Subscribe to my newsletter](https://newsletter.ashutoshkrris.tk)