# 布尔定义

> 原文：<https://www.freecodecamp.org/news/boolean-definition/>

在计算机科学中，布尔值指的是一个或真或假的值。

布尔因英国数学家乔治·布尔而得名。

布尔创立了代数的一个新分支，现在被称为布尔代数，其中 true 的值是 1，false 的值是 0。在布尔代数中，主要有三种逻辑运算:**与**、**或**、**非**。

布尔代数为信息时代和计算机科学奠定了基础。所有计算机都使用布尔代数的基本原理运行，其中 1 或真为开，0 或假为关。

因此，许多编程语言都包含布尔数据类型和运算符。

例如，在 JavaScript 中，常见的布尔数据类型有`true`和`false`:

```
const isCat = true; 
```

JavaScript 还有用于**和**的逻辑操作符:

```
const isCat = true;
const isCute = true;

if (isCat && isCute) { // isCat AND isCute are both true
  console.log("There's a cute cat :D"); // logs "There's a cute cat :D" to the console
} 
```

**或**:

```
const isCat = true;
const isFluffy = false;

if (isCat || isFluffy) { // either isCat OR isFluffy are true
  console.log("There's an animal that might be a cat, fluffly, or both"); // logs "There's an animal that might be a cat, fluffly, or both" to the console
} 
```

而**不是**:

```
const isCat = true;
const isFluffy = false;

if (!isFluffy) { // isFluffy is false, or NOT true
  console.log("Whatever this animal is, it's not fluffy"); // logs "Whatever this animal is, it's not fluffy" to the console
} 
```

此外，像许多其他编程语言一样，JavaScript 还有其他返回布尔值的运算符:

```
const catName = 'Boomer';

if (catName === 'Boomer') { // evaluates to true
  console.log('BOOMER LIVES!'); // logs 'BOOMER LIVES!' to the console
} 
```

## 相关技术术语:

*   [二进制定义](https://www.freecodecamp.org/news/binary-definition/)
*   [位定义](https://www.freecodecamp.org/news/bit-definition/)