# Java 随机数生成器——如何用 Math Random 生成整数

> 原文：<https://www.freecodecamp.org/news/generate-random-numbers-java/>

计算机生成的随机数分为两类:真随机数和伪随机数。

真正的随机数是基于外部因素生成的。例如，利用周围的噪声产生随机性。

但是生成这样的真随机数是一项耗时的任务。因此，我们可以利用使用算法和种子值生成的伪随机数。

这些伪随机数足以满足大多数目的。例如，您可以在密码学中使用它们，在构建游戏(如骰子或纸牌)中使用它们，以及在生成 OTP(一次性密码)数字中使用它们。

在本文中，我们将学习如何在 Java 中使用`Math.random()`生成伪随机数。

## 1.使用 Math.random()生成整数

`Math.random()`返回一个双精度型伪随机数，大于等于零且小于一。

让我们用一些代码来尝试一下:

```
 public static void main(String[] args) {
        double randomNumber = Math.random();
        System.out.println(randomNumber);
    }
    // output #1 = 0.5600740702032417
    // output #2 = 0.04906751303932033
```

每次执行都会给我们一个不同的随机数。

假设我们要生成一个特定范围内的随机数，比如 0 到 4。

```
 // generate random numbers between 0 to 4
    public static void main(String[] args) {
        // Math.random() generates random number from 0.0 to 0.999
        // Hence, Math.random()*5 will be from 0.0 to 4.999
        double doubleRandomNumber = Math.random() * 5;
        System.out.println("doubleRandomNumber = " + doubleRandomNumber);
        // cast the double to whole number
        int randomNumber = (int)doubleRandomNumber;
        System.out.println("randomNumber = " + randomNumber);
    }
    /* Output #1
    doubleRandomNumber = 2.431392914284627
    randomNumber = 2
    */
```

当我们将 double 类型转换为 int 类型时，int 值只保留整数部分。

比如上面的代码中，`doubleRandomNumber`就是`2.431392914284627`。`doubleRandomNumber`的整数部分为`2`，小数部分(小数点后的数字)为`431392914284627`。所以，`randomNumber`只会保存整数部分`2`。

你可以在 [Java 文档](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html)中阅读更多关于`Math.random()`方法的内容。

使用`Math.random()`并不是 Java 中生成随机数的唯一方式。接下来，我们将考虑如何使用 random 类生成随机数。

## 2.使用 Random 类生成整数

在 Random 类中，我们有许多提供随机数的实例方法。在本节中，我们将考虑两个实例方法，`nextInt(int bound)`和`nextDouble()`。

### 如何使用 nextInt(int bound)方法

`nextInt(int bound)`返回一个 int 类型的伪随机数，大于等于零且小于界限值。

`bound`参数指定范围。例如，如果我们指定界限为 4，`nextInt(4)`将返回一个 int 类型的值，大于或等于 0 且小于 4。0，1，2，3 是`nextInt(4)`的可能结果。

因为这是一个实例方法，我们应该创建一个随机对象来访问这个方法。让我们试一试。

```
 public static void main(String[] args) {
        // create Random object
        Random random = new Random();
        // generate random number from 0 to 3
        int number = random.nextInt(4);
        System.out.println(number);
    }
```

### 如何使用 nextDouble()方法

与`Math.random()`类似，`nextDouble()`返回双精度型伪随机数，大于等于零小于一。

```
 public static void main(String[] args) {
        // create Random object
        Random random = new Random();
        // generates random number from 0.0 and less than 1.0
        double number = random.nextDouble();
        System.out.println(number);
    }
```

要了解更多信息，您可以阅读 random 类的 [Java 文档](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html)。

## 那么应该用哪种随机数方法呢？

`[Math.random()](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html#random--)` [采用随机类](https://docs.oracle.com/javase/8/docs/api/java/lang/Math.html#random--)。如果我们的应用程序中只需要双精度伪随机数，那么我们可以使用`Math.random()`。

否则，我们可以使用 random 类，因为它提供了各种方法来生成不同类型的伪随机数，如`nextInt()`、`nextLong()`、`nextFloat()`和`nextDouble()`。

感谢您的阅读。

照片由[布雷特·乔丹](https://unsplash.com/@brett_jordan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/random-numbers?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

你可以通过[媒介](https://mvthanoshan.medium.com/)与我联系。

**编码快乐！**