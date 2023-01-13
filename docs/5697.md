# 如何用 JavaScript 从一个给定的数字中形成最小的数字

> 原文：<https://www.freecodecamp.org/news/forming-the-smallest-possible-number-from-the-given-number-in-javascript-bda790655c8e/>

普拉尚·亚达夫

# 如何用 JavaScript 从一个给定的数字中形成最小的数字

在本教程中，我们将实现一个算法，用 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) 形成尽可能小的数。

![w8OnyWg2LPtjbRckRL41TKnrqr6ObvUOZbEW](img/530008d6732881722e0fef9682566af1.png)

Source: Pixabay

```
Input: 55010 7652634
```

```
Output: 10055 2345667
```

**注意**:转换后的数字如果至少有一个非零字符，就不能以 0 开头。

我们将使用两种不同的方法来解决这个问题。一切都会写在 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro) 里。

*   在第一种方法中，我们将假设所提供的数字是字符串格式，并使用排序来解决它，排序将花费 O(nlogn)。
*   在第二种方法中，我们将用 O(d)时间求解数值，其中 d 是数字的个数。

### 使用排序 O(nlogn)。

#### 履行

*   我们将把数字转换成字符数组，然后对该数组进行排序。
*   排序后，我们将检查数组中的第一个字符是否为 0。
*   如果它不是 0，那么我们将加入数组并返回它。
*   如果它是 0，那么我们将找到第一个非零数字，与 0 交换并返回它。

```
function smallestPossibleNumber(num){
```

```
//Create a character array and sort it in ascending orderlet sorted = num.split('').sort();
```

```
//Check if first character is not 0 then join and return it if(sorted[0] != '0'){    return sorted.join('');}
```

```
//find the index of the first non - zero character let index = 0; for(let i = 0; i < sorted.length; i++){  if(sorted[i] > "0"){    index = i;    break;  } }
```

```
//Swap the indexes  let temp = sorted[0];  sorted[0] = sorted[index];  sorted[index] = temp;
```

```
//return the string after joining the characters of array return sorted.join(''); }
```

运行程序

```
Input:console.log(smallestPossibleNumber('55010'));console.log(smallestPossibleNumber('7652634'));console.log(smallestPossibleNumber('000001'));console.log(smallestPossibleNumber('000000'));
```

```
Output:100552345667100000000000
```

```
/*How it works  let sorted = num.split('').sort();   = ['5','5','0','1','0'].sort() = ['0','0','1','5','5']  if(sorted[0] != '0'){   // '0' != '0' condition fails     return sorted.join('');  }    //Find the index of the first non - zero character  let index = 0;  for(let i = 0; i < sorted.length; i++){     if(sorted[i] > '0'){  // '1' > '0'       index = i;      // index = 2;       break;          // break;     }  }    //swap the index  var temp = sorted[0];        sorted[0] = sorted[index];  sorted[index] = temp;    //return the string  return sorted.join('');*/
```

#### 它是如何工作的

我们首先创建了类似于`['5', '5', '0', '1', 0]`的字符数组。然后我们将它排序到`['0', '0', '1', '5', '5']`，在这之后，我们找到第一个非零元素，并将其与第一个零元素交换，就像`['1', '0', '0', '5', '5']`。现在我们已经准备好了最小的数，我们只需要把它们连接起来并返回它。

了解更多关于 [split()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) ， [sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) ， [join()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) 。

时间复杂度:O(nlogn)。空间复杂度:O(n)。

#### 时间和空间复杂性解释

我们正在创建一个字符数组，需要 O(n)时间。那么对数组排序将花费 O(nlogn)。

在那之后，我们寻找最小非零数字的索引，在最坏的情况下可以取 O(n ),加入数组来创建字符串将取 O(n)。因为所有这些操作都是一个接一个地运行的。所以时间复杂度是 O(n + nlogn + n + n) = O(nlogn)。

我们从字符串中创建一个字符数组，所以空间复杂度是 O(n)。

### 使用数值 O(logn)。

这种方法有一个缺点:如果数字只包含零，那么它将返回一个零。

#### 履行

*   我们将创建一个从 0 到 9 的数字数组。
*   然后，我们将通过增加数组中的位数来跟踪数字中的位数。
*   之后，我们将找到最小的非零数字，并将其计数减 1。
*   最后，我们将通过按升序排列来重新创建数字，并返回结果。
*   这个解决方案基于计数排序。

```
function smallestPossibleNumber(num) {    // initialize frequency of each digit to Zero   let freq = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];          // count frequency of each digit in the number   while (num > 0){      let d = parseInt(num % 10); // extract last digit     freq[d]++; // increment counting     num = parseInt(num / 10); //remove last digit   }
```

```
// Set the LEFTMOST digit to minimum expect 0   let result = 0;    for (let i = 1 ; i <= 9 ; i++) {       if (freq[i] != 0) {          result = i;          freq[i]--;          break;      }    }           // arrange all remaining digits   // in ascending order   for (let i = 0 ; i <= 9 ; i++) {      while (freq[i]-- != 0){          result = result * 10 + i;       }   }        return result; }
```

运行程序

```
Input:console.log(smallestPossibleNumber('55010'));console.log(smallestPossibleNumber('7652634'));console.log(smallestPossibleNumber('000001'));console.log(smallestPossibleNumber('000000'));
```

```
Output:10055234566710
```

```
/* How it works   // initialize frequency of each digit to Zero   let freq = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0];      // count frequency of each digit in the number   while (num > 0){      let d = parseInt(num % 10); // extract last digit     freq[d]++; // increment counting             num = parseInt(num / 10); //remove last digit   }    //After incrementing count   //freq = [2, 1, 0, 0, 0, 2, 0, 0, 0, 0]      // Set the LEFTMOST digit to minimum expect 0   let result = 0;    for (let i = 1 ; i <= 9 ; i++) {       if (freq[i] != 0) {          result = i;          freq[i]--;          break;      }    }    // result = 1     // arrange all remaining digits   // in ascending order   for (let i = 0 ; i <= 9 ; i++) {      while (freq[i]-- != 0){          result = result * 10 + i;       }   }
```

```
 //10   //100   //1005   //10055   //10055      return result*/
```

时间复杂度:O(nlogn)。
空间复杂度:O(1)。

#### 时间和空间复杂性解释

我们从数字中删除每个数字，并在一个取 O(logn)的数组中增加其各自的计数。然后我们从 O(10)中的数组中找到最小的非零数字。

之后，我们重新排列这些数字，以创建 O(10 * logn)中最小的数字。时间复杂度为 O(logn + 10+ 10logn) = O(logn)或 O(d)，其中 d 为位数

我们使用的是常数空间(10 个数的数组)，所以空间复杂度是 O(1)。

如果你喜欢这篇文章，请给它一些？并分享一下！如果你有任何与此相关的问题，请随时问我。

*更多类似的和 Javascript 中的算法解决方案，请关注我的* [**Twitter**](https://twitter.com/LearnersBucket) *。*我写关于 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) ，React，Nodejs，[数据结构](https://learnersbucket.com/tutorials/topics/data-structures/)，以及[算法](https://learnersbucket.com/examples/topics/algorithms/)关于[*learnersbucket.com*](https://learnersbucket.com/)*。*