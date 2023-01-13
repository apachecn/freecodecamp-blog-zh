# 搜索算法:指数搜索解释

> 原文：<https://www.freecodecamp.org/news/search-algorithms-exponential-search-explained/>

## **指数搜索**

指数搜索也称为指针搜索，通过在每次迭代中跳转`2^i`元素来搜索排序数组中的元素，其中 I 表示循环控制变量的值，然后验证搜索元素是否出现在上次跳转和当前跳转之间。

## 复杂性最坏情况

O(log(N))经常因为名字而混淆，算法之所以这么命名并不是因为时间复杂度。这个名字是由于算法以等于指数 2 的步长跳跃元素而产生的

## 它是如何工作的

1.  一次跳转数组`2^i`元素，搜索条件`Array[2^(i-1)] < valueWanted < Array[2^i]`。如果`2^i`大于数组长度，则设置数组长度的上限。
2.  在`Array[2^(i-1)]`和`Array[2^i]`之间做二分搜索法

## 代码

```
// C++ program to find an element x in a
// sorted array using Exponential search.
#include <bits/stdc++.h>
using namespace std;

int binarySearch(int arr[], int, int, int);

// Returns position of first ocurrence of
// x in array
int exponentialSearch(int arr[], int n, int x)
{
    // If x is present at firt location itself
    if (arr[0] == x)
        return 0;

    // Find range for binary search by
    // repeated doubling
    int i = 1;
    while (i < n && arr[i] <= x)
        i = i*2;

    //  Call binary search for the found range.
    return binarySearch(arr, i/2, min(i, n), x);
}

// A recursive binary search function. It returns
// location of x in  given array arr[l..r] is
// present, otherwise -1
int binarySearch(int arr[], int l, int r, int x)
{
    if (r >= l)
    {
        int mid = l + (r - l)/2;

        // If the element is present at the middle
        // itself
        if (arr[mid] == x)
            return mid;

        // If element is smaller than mid, then it
        // can only be present n left subarray
        if (arr[mid] > x)
            return binarySearch(arr, l, mid-1, x);

        // Else the element can only be present
        // in right subarray
        return binarySearch(arr, mid+1, r, x);
    }

    // We reach here when element is not present
    // in array
    return -1;
}

int main(void)
{
   int arr[] = {2, 3, 4, 10, 40};
   int n = sizeof(arr)/ sizeof(arr[0]);
   int x = 10;
   int result = exponentialSearch(arr, n, x);
   (result == -1)? printf("Element is not present in array")
                 : printf("Element is present at index %d", result);
   return 0;
}
```