# 简明英语中的算法:时间复杂性和 Big-O 符号

> 原文：<https://www.freecodecamp.org/news/time-is-complex-but-priceless-f0abd015063c/>

迈克尔·奥洛伦尼索拉

# 简明英语中的算法:时间复杂性和 Big-O 符号

![6qO7Iovep7g13dZfw5DCOTJheMb1rjtdvdK7](img/82d64189a7c5dc01848d1b5168a36729.png)

每个优秀的开发人员都有自己的时间。他们想给用户更多的信息，这样他们就可以做他们喜欢的事情。他们通过最小化时间复杂度来做到这一点。

在你理解编程中的时间复杂性之前，你必须理解它最常见的应用:在**算法**的设计中。

### 那么到底什么是算法呢？

简单地说，算法是一系列包含的步骤，你遵循这些步骤是为了实现某些目标，或者产生某些输出。让我们以你奶奶烤蛋糕的食谱为例。等等，那算不算算法？当然了！

```
function BakeCake(flavor, icing){
"
 1\. Heat Oven to 350 F
 2\. Mix flour, baking powder, salt
 3\. Beat butter and sugar until fluffy
 4\. Add eggs.
 5\. Mix in flour, baking powder, salt
 6\. Add milk and " + flavor + "
 7\. Mix further
 8\. Put in pan
 9\. Bake for 30 minutes
10." + if(icing === true) return 'add icing' + "
10\. Stuff your face
"
}

BakeCake('vanilla', true) => deliciousness
```

算法在我们研究时间复杂性时很有用，因为它们有各种形状和大小。

就像你可以用 100 种不同的方法切馅饼一样，你可以用许多不同的算法来解决一个问题。有些解决方案比其他方案更有效，花费的时间更少，需要的空间也更少。

因此，主要问题是:我们如何着手分析哪些解决方案是最有效的？

数学拯救世界！编程中的时间复杂性分析只是一种极其简化的数学方法，用于分析给定输入数(n)的算法完成任务需要多长时间。它通常使用 **Big-O 符号**来定义。

### 你会问，大 O 符号是什么？

如果你保证不放弃，不读书，我就告诉你。

Big-O 符号是一种将算法的所有步骤转换成代数项的方法，然后排除对问题的整体复杂性没有太大影响的低阶常数和系数。

数学家们可能会对我的“整体影响”假设感到有点畏缩，但对于开发人员来说，为了节省时间，这样简化事情更容易:

```
Regular       Big-O

2             O(1)   --> It's just a constant number

2n + 10       O(n)   --> n has the largest effect

5n^2          O(n^2) --> n^2 has the largest effect
```

简而言之，这个例子只是说:我们只关注表达式中对表达式返回的值有潜在最大影响的因素。(随着常数变得非常大，n 变得很小，这种情况会发生变化，但我们现在不用担心这个)。

下面是一些带有简单定义的常见时间复杂性。不过，要获得更深入的定义，请随意查看维基百科。

*   O(1) —恒定时间:给定大小为 n 的输入，算法只需一步即可完成任务。
*   O(log n) —对数时间:给定大小为 n 的输入，完成任务所需的步骤数会随着每一步的进行而减少。
*   O(n) —线性时间:给定大小为 n 的输入，所需的步数是直接相关的(1 比 1)
*   O(n ) —二次时间:给定大小为 n 的输入，完成一项任务所需的步骤数是 n 的平方。
*   o(c^n)——指数时间:给定一个大小为 n 的输入，完成一项任务所需的步骤数是一个常数的 n 次方(相当大的数字)。

有了这些知识，让我们看看每一个时间复杂性需要多少步骤:

```
let n = 16;

O (1) = 1 step "(awesome!)"

O (log n) = 4 steps  "(awesome!)" -- assumed base 2

O (n) = 16 steps "(pretty good!)"

O(n^2) = 256 steps "(uhh..we can work with this?)"

O(2^n) = 65,536 steps "(...)"
```

如你所见，根据算法的复杂程度，事情很容易变得复杂几个数量级。幸运的是，计算机足够强大，仍然可以相对较快地处理非常复杂的问题。

那么我们如何着手用 Big-O 符号分析我们的代码呢？

这里有一些简单快捷的例子，告诉你如何将这些知识应用到你可能在野外遇到的算法中，或者自己编写代码。

对于我们的示例，我们将使用以下数据结构:

```
var friends = {
 'Mark' : true,
 'Amy' : true,
 'Carl' : false,
 'Ray' :  true,
'Laura' : false,
}
var sortedAges = [22, 24, 27, 29, 31]
```

#### **O(1) —常数时间**

当你知道键(对象)或索引(数组)总是走一步，并因此是常数时间时，值查找。

```
//If I know the persons name, I only have to take one step to check:

function isFriend(name){ //similar to knowing the index in an Array 
  return friends[name]; 
};

isFriend('Mark') // returns True and only took one step

function add(num1,num2){ // I have two numbers, takes one step to return the value
 return num1 + num2
}
```

#### **O(对数 n) —对数时间**

如果你知道在数组的哪一边寻找一个项目，你就可以通过去掉另一半来节省时间。

```
//You decrease the amount of work you have to do with each step

function thisOld(num, array){
  var midPoint = Math.floor( array.length /2 );
  if( array[midPoint] === num) return true;
  if( array[midPoint] < num ) --> only look at second half of the array
  if( array[midpoint] > num ) --> only look at first half of the array
  //recursively repeat until you arrive at your solution

}

thisOld(29, sortedAges) // returns true 

//Notes
 //There are a bunch of other checks that should go into this example for it to be truly functional, but not necessary for this explanation.
 //This solution works because our Array is sorted
 //Recursive solutions are often logarithmic
 //We'll get into recursion in another post!
```

#### **O(n) —线性时间**

你必须查看数组或列表中的每一项来完成任务。单循环的**几乎都是线性时间。另外像 **indexOf** 这样的数组方法也是线性时间的。你只是从循环过程中抽象出来。**

```
//The number of steps you take is directly correlated to the your input size

function addAges(array){
  var sum = 0;
  for (let i=0 ; i < array.length; i++){  //has to go through each value
    sum += array[i]
  }
 return sum;
}

addAges(sortedAges) //133
```

#### **O(n ) —二次时间**

循环的嵌套**是二次时间，因为你在一个线性操作中运行另一个线性操作(或者 n*n = n)。**

```
//The number of steps you take is your input size squared

function addedAges(array){
  var addedAge = [];
    for (let i=0 ; i < array.length; i++){ //has to go through each value
      for(let j=i+1 ; j < array.length ; j++){ //and go through them again
        addedAge.push(array[i] + array[j]);
      }
    }
  return addedAge;
}

addedAges(sortedAges); //[ 46, 49, 51, 53, 51, 53, 55, 56, 58, 60 ]

//Notes
 //Nested for loops. If one for loop is linear time (n)
 //Then two nested for loops are (n * n) or (n^2) Quadratic!
```

#### **O(2^n) —指数时间**

指数时间通常是在你不知道那么多的情况下，你必须尝试每一种可能的组合或排列。

```
//The number of steps it takes to accomplish a task is a constant to the n power

//Thought example
 //Trying to find every combination of letters for a password of length n
```

你应该在任何时候编写需要快速运行的代码时进行时间复杂度分析。

当你有多种途径来解决一个问题时，首先创造一个可行的解决方案无疑是更明智的。但是从长远来看，你会想要一个尽可能快速有效的解决方案。

为了帮助您解决问题，以下是一些简单的问题:

> 1.这能解决问题吗？**是** = >
> 
> 2。你有时间做这个
> 
> **吗？有** = >去第 3 步
> 
> **不** = >以后再来，现在去第 6 步。
> 3。它是否涵盖所有边缘病例？**是** = >
> 
> 4。我的复杂性尽可能低吗？
> 
> **否** = >重写或修改成新的解决方案—>回到步骤 1
> 
> **是** = >回到步骤 5
> 
> 5。我的代码是 D.R.Y 吗？**是** = >
> 
> 6。欢呼吧！
> 
> **不要** = >好好享受吧！

在你试图解决问题的任何时候都要分析时间复杂性。它会让你在日志运行中成为一个更好的开发者。你的队友和用户会因此而爱你。

同样，作为程序员，你将面临的大多数问题——无论是算法上的还是程序上的——都有数十种甚至数百种解决方法。他们可能在解决问题的方式上有所不同，但他们都解决了那个问题。

您可能正在决定是使用集合还是图形来存储数据。您可以决定是否在团队项目中使用 Angular、React 或 Backbone。所有这些解决方案都以不同的方式解决了相同的问题。

鉴于此，很难说这些问题有单一的“正确”或“最佳”答案。但是对于一个给定的问题，可以说有“更好”或“更坏”的答案。

使用我们之前的一个例子，如果您的团队中有一半人有使用 React 的经验，那么对于团队项目来说使用 React 可能会更好，这样启动和运行它会花费更少的时间。

描述一个更好的解决方案的能力通常来源于某种时间复杂性分析的表象。

简而言之，如果你要解决一个问题，那就好好解决它。用一些大 O 来帮你弄明白。

这里是最后的总结:

*   **O(1) —** 常数时间:算法完成任务只需要一步。
*   **O(log n) —** 对数时间:完成一项任务所需的步骤数随着每一步的进行而减少。
*   **O(n) —** 线性时间:所需步数直接相关(1 对 1)。
*   **O(n ) —** 二次时间:完成一项任务所需的步数是 n 的平方。
*   **o(c^n)——**指数:完成一项任务所需的步骤数是一个常数的 n 次方(相当大的数字)。

以下是一些有助于了解更多信息的资源:

*   [维基百科](https://en.wikipedia.org/wiki/Time_complexity)
*   Big O Cheat Sheet 是一个很好的资源，具有常见的算法时间复杂性和图形表示。看看吧！