# 如何用 Java 实现中国剩余定理

> 原文：<https://www.freecodecamp.org/news/how-to-implement-the-chinese-remainder-theorem-in-java-db88a3f1ffe0/>

作者:Anuj Pahade

# 如何用 Java 实现中国剩余定理

![piavM78chsZ7tCSFEnXthhULKXOJRoRYqXAA](img/b47cc31b660c7da7fccdd65b38467704.png)

“black bunting flags” by [Adi Goldstein](https://unsplash.com/@adigold1?utm_source=medium&utm_medium=referral)

这篇文章假设你知道什么是中国剩余定理(CRT ),并着重于它在 Java 中的实现。如果你不知道，我建议你在这里阅读。

你可以在这篇文章的末尾找到完整代码的链接。所以让我们开始吧。

#### 我们需要找到什么？

我们需要找到 x？

声明如下:

存在一个*最小正数* X，使得:

```
X % number[0]    =  remainder[0], X % number[1]    =  remainder[1], ...............X % number[k-1]  =  remainder[k-1]
```

这里我们有两个数组。

1.  **数字数组:**这个数组中的所有数字都是两两互质的。也就是说，从数组中选取任意两个数，你会发现它们的最大公约数是 1。
2.  **余数数组:**从上面的表达式中可以看出，当 X 被来自*数*数组的一个数 *n* 除时，它会从*余数*数组中留下相应的余数。

### 实施 CRT 的步骤

这些就是实现 CRT 的步骤，或者如我们工程师所说的“算法”。

#### 第一步:找出第一个数组中所有数字的乘积。

```
for(int i=0; i<number.length; i++ ){   product *= number[i];}
```

#### 第二步:求每个数的部分积。

*n* 的部分乘积=乘积/ *n*

```
for(int i=0; i<num.length; i++){   partialProduct[i] = product/number[i];}
```

#### 3.求数[i]的模乘逆模部分积[i]。

这里我们使用[扩展的欧几里德算法](https://www.geeksforgeeks.org/euclidean-algorithms-basic-and-extended/)来求逆。所以，我们称*为 computeInverse(partial product[I]，num[i])*

```
public static int computeInverse(int a, int b){         int m = b, t, q;         int x = 0, y = 1;               if (b == 1)             return 0;               // Apply extended Euclid Algorithm         while (a > 1)         {             // q is quotient             q = a / b;             t = b;                   // now proceed same as Euclid's algorithm             b = a % b;a = t;             t = x;             x = y - q * x;             y = t;         }               // Make x1 positive         if (y < 0)          y += m;               return y;     }
```

#### 第四步:最终金额

```
sum += partialProduct[i] * inverse[i] * rem[i];
```

#### 第五步:返回最小的 X

为了找到所有解中的最小值，我们将步骤 4 的和除以步骤 2 的积。

```
return sum % product;
```

因此，我们已经找到了我们的 x。我建议您在查看下面链接中的代码之前，尝试自己实现代码。

感谢您阅读帖子。希望对你有帮助。请在下面的评论中留下你的建议，或者用更好的版本联系我，或者在 gmail[dot]com 上查询 anujp5678，或者在 LinkedIn 上联系我[。](https://www.linkedin.com/in/anuj-pahade/)

请鼓掌。？掌声激励人心。

开心快乐编码！:)