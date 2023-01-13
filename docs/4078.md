# 什么是 Expotentiation？带有代码示例的算法指南

> 原文：<https://www.freecodecamp.org/news/what-is-expotentiation-an-algorith-guide/>

给定两个整数 a 和 n，写一个函数来计算 a^n.

#### 密码

算法范式:分治。

```
int power(int x, unsigned int y) { 
    if (y == 0) 
        return 1; 
    else if (y%2 == 0) 
        return power(x, y/2)*power(x, y/2); 
    else
        return x*power(x, y/2)*power(x, y/2); 
} 
```

时间复杂度:`O(n)` |空间复杂度:`O(1)`

#### 优化解决方案:O(logn)

```
int power(int x, unsigned int y) { 
    int temp; 
    if( y == 0) 
        return 1; 
    temp = power(x, y/2); 
    if (y%2 == 0) 
        return temp*temp; 
    else
        return x*temp*temp; 
} 
```

为什么这样更快？

假设我们有 x = 5，y = 4，我们知道我们的答案会是(5 * 5 * 5 * 5)。

如果我们把它分解，我们会发现我们可以把(5 * 5 * 5 * 5)写成(5 * 5) * 2，而且我们可以把(5 * 5)写成 5 * 2。

通过这种观察，我们可以通过只计算一次幂(x，y/2)并存储它来将我们的函数优化为 O(log n)。

## 模幂运算

给定三个数字 x、y 和 p，计算(x^y) % p

```
int power(int x, unsigned int y, int p) { 
    int res = 1;  
    x = x % p; 
    while (y > 0) {  
        if (y & 1) 
            res = (res*x) % p; 

        // y must be even now 
        y = y >> 1; 
        x = (x*x) % p;   
    } 
    return res; 
} 
```

时间复杂度:`O(Log y)`。