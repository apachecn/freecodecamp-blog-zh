# 跳转搜索算法讲解

> 原文：<https://www.freecodecamp.org/news/jump-search-algorithm-explained/>

## **跳转搜索**

跳转搜索通过跳转 k 个条目来定位排序数组中的条目，然后验证想要的条目是否在前一个跳转和当前跳转之间。

### 复杂性最坏情况

o(≥n)

## 它是如何工作的

1.  定义 k 的值，即跳转次数:最佳跳转大小是√N，其中 N 是数组的长度
2.  根据条件`Array[i] < valueWanted < Array[i+k]`逐 k 跳数组搜索
3.  在`Array[i]`和`Array[i + k]`之间进行线性搜索

![Jumping Search 1](img/e1bb52d606958ce67d5b9fdc68b2d98c.png)

## 密码

要查看此方法的代码实现示例，请访问下面的链接:

[跳跃搜索- OpenGenus/cosmos](https://github.com/OpenGenus/cosmos/tree/master/code/search/jump_search)

### 信用

[逻辑的数组图像](http://theoryofprogramming.com/2016/11/10/jump-search-algorithm/)