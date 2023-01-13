# 我如何使用算法来解决我现实生活中的随身背包的背包问题

> 原文：<https://www.freecodecamp.org/news/how-i-used-algorithms-to-solve-the-knapsack-problem-for-my-real-life-carry-on-knapsack-5f996b0e6895/>

我是个流浪者，靠一个随身携带的包过活。这意味着我所有的个人物品的总重量必须低于飞机自带行李的重量限制——通常是 10 公斤。然而，在一些较小的航空公司，这一重量限制降至 7 公斤。偶尔，我不得不决定不带一些东西来适应更小的重量限制。

作为一项实践，决定留下什么(或者一起扔掉)需要把我所有的东西都摆出来，并选择保留哪些。这个决定是基于物品对我的有用性(它的价值)和它的重量。

![0*IJPBUwDAGDhz13i2](img/207853f6b3353134537cb145f520bad2.png)

*This is all my stuff, and my Minaal Carry-on bag.*

作为一名程序员，我意识到这样的决定可以由计算机更有效地做出。事实上，这种情况发生得如此频繁，如此普遍，以至于许多人会将这种情况视为经典的*打包问题*或*背包问题。*我该如何告诉电脑，当我的行李重量限制在 7 公斤或以下时，在我的包里放尽可能多的重要物品？用算法！耶！

我将讨论解决背包问题的两种常见方法:一种称为**贪婪算法** *、*，另一种称为**动态规划**(有点难，但更好、更快、更强……)。

我们开始吧。

### 该设置

我以 CSV 文件的形式准备了我的数据，该文件有三列:物品的名称(一个字符串)、其价值的表示(一个整数)和其重量(以克为单位)(一个整数)。总共有 40 项。我用从 40 到 1 对每一项进行排名来表示价值，40 是最重要的，1 相当于“为什么我又有了这个？”(如果你从未列出你所有的财产，并按照它们对你的有用程度进行排序，我强烈建议你尝试一下。这可能是一个非常有启发性的练习。)

**所有物品和袋子的总重量:**9003 克

**袋重:**1415 克

**航空公司限额:**7000 克

**我可以打包的物品最大重量:**5585 克

**物品总价值:** 820 英镑

挑战:在最大化总价值的同时，尽可能多地打包物品。

### 数据结构

#### 读入文件

在我们开始考虑如何解决背包问题之前，我们必须解决读入和存储数据的问题。幸运的是，Go 标准库的`io/ioutil`包使得第一部分变得简单明了。

```
package main

import (
    "fmt"
    "io/ioutil"
)

func check(e error) {
    if e != nil {
        panic(e)
    }
}

func readItems(path string) {
	dat, err := ioutil.ReadFile(path)
	check(err)
    fmt.Print(string(dat))
}
```

`ReadFile()`函数接受一个文件路径，并返回文件内容和一个错误(如果调用成功，则返回`nil`),所以我们还创建了一个`check()`函数来处理任何可能返回的错误。在现实世界的应用程序中，我们可能想要做一些比`panic`更复杂的事情，但是现在这并不重要。

#### 创建结构

既然我们已经得到了数据，我们也许应该对它做些什么。因为我们正在处理现实生活中的物品和一个现实生活中的包，所以让我们创建一些类型来表示它们，并使我们的程序更容易概念化。Go 中的一个`struct`是字段的类型化集合。这里有两种类型:

```
type item struct {
	name          string
	worth, weight int
}

type bag struct {
	bagWeight, currItemsWeight, maxItemsWeight, totalWeight int
	items                                                   []item
}
```

使用描述性很强的字段名很有帮助。你可以看到这些结构是按照我们描述的它们所代表的东西来建立的。一个`item`有一个`name`(字符串)，还有一个`worth`和`weight`(整数)。一个`bag`有几个代表其属性的`int`类型的字段，并且还能够保存`items`，在结构中表示为一片`item`类型的 thingamabobbers。

#### 解析和存储我们的数据

有几个全面的 Go 包可以用来解析我们的 CSV 数据…但是这有什么意思呢？让我们从一些字符串分割和 for 循环开始。这是我们更新后的`readItems()`函数:

```
func readItems(path string) []item {

	dat, err := ioutil.ReadFile(path)
	check(err)

	lines := strings.Split(string(dat), "\n")

	itemList := make([]item, 0)

	for i, v := range lines {
		if i == 0 {
			continue
		}
		s := strings.Split(v, ",")
		newItemWorth, _ := strconv.Atoi(s[1])
		newItemWeight, _ := strconv.Atoi(s[2])
		newItem := item{name: s[0], worth: newItemWorth, weight: newItemWeight}
		itemList = append(itemList, newItem)
	}
	return itemList
}
```

使用`strings.Split`，我们在换行符上分割我们的`dat`。然后我们创建一个空的`itemList`来存放我们的项目。

在 for 循环中，我们跳过 CSV 文件的第一行(文件头),然后遍历每一行。我们使用`strconv.Atoi`(读作“A 到 I”)将每件物品的价值和重量转换成整数。然后，我们用这些字段值创建一个`newItem`，并将其附加到`itemList`。最后，我们返回`itemList`。

到目前为止，我们的设置如下:

```
package main

import (
	"io/ioutil"
	"strconv"
	"strings"
)

type item struct {
	name          string
	worth, weight int
}

type bag struct {
	bagWeight, currItemsWeight, maxItemsWeight, totalWeight, totalWorth int
	items                                                               []item
}

func check(e error) {
	if e != nil {
		panic(e)
	}
}

func readItems(path string) []item {

	dat, err := ioutil.ReadFile(path)
	check(err)

	lines := strings.Split(string(dat), "\n")

	itemList := make([]item, 0)

	for i, v := range lines {
		if i == 0 {
			continue // skip the headers on the first line
		}
		s := strings.Split(v, ",")
		newItemWorth, _ := strconv.Atoi(s[1])
		newItemWeight, _ := strconv.Atoi(s[2])
		newItem := item{name: s[0], worth: newItemWorth, weight: newItemWeight}
		itemList = append(itemList, newItem)
	}
	return itemList
}
```

现在我们已经建立了数据结构，让我们开始打包(？)关于第一种方法。

### 贪婪算法

贪婪算法是解决背包问题的最直接的方法，因为它是一个构建单个最终解的单程算法。在问题的每个阶段，贪婪算法都会选择局部最优的选项，这意味着它看起来是目前最合适的选项。当它在我们的数据集中前进时，它不会修改它以前的选择。

#### 构建我们的贪婪算法

我们将用来解决背包问题的算法步骤是:

1.  按价值降序排列项目。
2.  从价值最高的物品开始。把物品放进袋子里，直到单子上的下一件物品装不下为止。
3.  尝试用列表中的下一个项目来填充剩余的容量。

如果你读了我关于解决问题和做肉菜饭的文章，你会知道我总是从找出下一个最重要的问题开始。在这种情况下，有三个主要操作我们需要弄清楚如何做:

*   按价值排序项目。
*   把一件东西放进包里。
*   检查一下袋子是否满了。

第一个只是一个文档查找。下面是我们如何在 Go 中对切片进行排序:

```
sort.Slice(is, func(i, j int) bool {
    return is[i].worth > is[j].worth
})
```

`sort.Slice()`函数根据我们提供的较少的函数对我们的项目进行排序。在这种情况下，它会将价值最高的项目排在价值最低的项目之前。

考虑到我们不想把不合适的东西放进包里，我们将反向完成最后两个任务。首先，我们要看看这件商品是否合适。如果是这样的话，那就放在袋子里。

```
func (b *bag) addItem(i item) error {
	if b.currItemsWeight+i.weight <= b.maxItemsWeight {
		b.currItemsWeight += i.weight
		b.items = append(b.items, i)
		return nil
	}
	return errors.New("could not fit item")
}
```

注意我们第一行的`*`。这表明`bag`是一个指针接收器(与值接收器相反)。如果你是新手，这个概念可能会有点混乱。这里有一些需要考虑的事情，可能会帮助你决定什么时候使用值接收器，什么时候使用指针接收器。为了我们的`addItem()`功能，这种情况适用于:

> *如果方法需要变异接收方，接收方必须是指针。*

我们使用的指针接收器告诉我们的函数，我们希望对这个特定的包进行操作，特别是对这个包进行操作，而不是对一个新的包进行操作。这很重要，因为没有它，每一件物品都会被装进一个全新的包里！像这样的一个小细节可以决定代码是否有效，或者让你熬夜到凌晨 4 点，一边喝着红牛一边喃喃自语。(即使你的代码不起作用，也要按时睡觉——以后你会感谢我的。)

现在我们已经得到了我们的组件，让我们把我们的贪婪算法:

```
func greedy(is []item, b bag) {
	sort.Slice(is, func(i, j int) bool {
		return is[i].worth > is[j].worth
	})

	for i := range is {
		b.addItem(is[i])
	}

	b.totalWeight = b.bagWeight + b.currItemsWeight

	for _, v := range b.items {
		b.totalWorth += v.worth
	}
}
```

然后在我们的`main()`函数中，我们将创建我们的包，读入我们的数据，并调用我们的贪婪算法。这是它看起来的样子，所有的设置和准备就绪:

```
func main() {

	minaal := bag{bagWeight: 1415, currItemsWeight: 0, maxItemsWeight: 5585}
	itemList := readItems("objects.csv")

	greedy(itemList, minaal)
}
```

#### 贪婪算法结果

那么，在有效打包我们的行李以使其总价值最大化时，这种算法是如何工作的呢？结果如下:

**包和物品的总重量:**6987 克

**打包物品的总价值:** 716

以下是我们的贪婪算法选择的物品，按价值排序:

![1*jb4ug2Rd4T46XUdcYSgIJQ](img/9e9b0c6a03281629825c29ee84ac0099.png)

This is a screenshot because Medium doesn’t like tables.

很明显，贪婪算法是一种快速找到可行解的简单方法。对于小数据集，可能会接近最优解。该算法打包了总价值为 716 英镑的物品(比最大可能值少 104 分)，而只剩下 13 克。

正如我们之前了解到的，贪婪算法并没有改进它返回的解。它只是把它能找到的下一个最高价值的物品放进包里。

让我们看看解决背包问题的另一种方法，这种方法将给出最优解——在重量限制下的最高可能总价值。

### 动态规划

“动态编程”这个名字可能有点误导。这并不是一种编程风格，正如它的名字可能会让你推断的那样，而仅仅是另一种方法。

动态编程在几个关键方面不同于简单的贪婪算法。首先，动态规划包包装解决方案列举了整个解决方案空间，以及可用于包装我们的包的物品组合的所有可能性。贪婪算法选择最优的**局部**解，动态规划算法能够找到最优的**全局**解。

其次，动态编程使用内存化来存储先前计算的操作的结果，并在操作再次发生时返回缓存的结果。这允许它“记住”以前的组合。这比重新计算答案花费的时间要少。

#### 构建我们的动态规划算法

要使用动态规划找到打包行李的最佳方案，我们需要:

1.  创建一个代表所有物品子集(解决方案空间)的矩阵，其中行代表物品，列代表行李的剩余重量
2.  在矩阵中循环，计算在包容量的每个阶段，每种物品组合所能获得的价值
3.  检查完成的矩阵，以确定哪些物品要添加到包中，从而使包的总价值最大化

将我们的解空间形象化将是最有帮助的。下面是我们用代码构建的一个示例:

![0*s0ASnto3_AQSGjs1](img/372a67922cd8ba7ca65280367ea24767.png)

The empty knapsackian multiverse.

在围棋中，我们可以将这个矩阵创建为切片中的切片。

```
matrix := make([][]int, numItems+1) // rows representing items
for i := range matrix {
	matrix[i] = make([]int, capacity+1) // columns representing grams of weight
}
```

我们已经用`1`填充了行和列，以便标记与物品和重量数字相匹配。

现在我们已经创建了矩阵，我们将通过循环遍历行和列来填充它:

```
// loop through table rows
for i := 1; i <= numItems; i++ {
	// loop through table columns
	for w := 1; w <= capacity; w++ {
		// do stuff in each element
	}
}
```

然后对于每个元素，我们将计算它的价值。我们通过代表以下过程的代码来实现这一点:

如果与当前行匹配的索引处的项目符合当前列表示的重量容量，则取以下两者中的最大值:

*   已经在袋子中的物品的总价值，
*   袋子中除了前一行索引中的物品之外的所有物品的总价值，加上新物品的价值。

换句话说，当我们的算法考虑其中一件物品时，我们要求它决定在当前的总重量下，添加到包中的这件物品是否会比它添加到包中的最后一件物品产生更高的总价值。如果这个当前项目是更好的选择，就把它放进去——如果不是，就把它去掉。

下面是实现这一点的代码:

```
// if weight of item matching this index can fit at the current capacity column...
if is[i-1].weight <= w {
	// worth of this subset without this item
	valueOne := float64(matrix[i-1][w])
	// worth of this subset without the previous item, and this item instead
	valueTwo := float64(is[i-1].worth + matrix[i-1][w-is[i-1].weight])
	// take maximum of either valueOne or valueTwo
	matrix[i][w] = int(math.Max(valueOne, valueTwo))
// if the new worth is not more, carry over the previous worth
} else {
	matrix[i][w] = matrix[i-1][w]
}
```

这一比较物品组合的过程将持续下去，直到每一件物品都被考虑到行李总重量增加的每一个可能阶段。当考虑了以上所有因素后，我们将列举解决方案空间——用所有可能的总价值填充矩阵。

我们会有一个大的数字图表，在最后一列最后一行我们会有我们可能的最高值。

![0*1OZ_-r3Zx-Yhm4zR](img/7b6f11979baecda5a68fa798a988001f.png)

A strictly representative representation of the filled matrix.

这很好，但是我们如何找出放在袋子里的物品的组合来达到这个价值呢？

#### 获取我们的优化项目列表

要查看哪些项目组合在一起创建了我们的最佳装箱单，我们需要以与创建时相反的方式检查我们的矩阵。因为我们知道最大可能值在最后一列的最后一行，所以我们从那里开始。为了找到物品，我们:

1.  获取当前单元格的值
2.  将当前单元格的值与其正上方单元格中的值进行比较
3.  如果数值不同，则说明箱包项目发生了变化。根据当前物品的重量，通过在列中向后移动来查找要检查的下一个单元格(查找添加当前物品之前袋子的值)
4.  如果值匹配，则箱包项目没有变化。向上移动到上一行的单元格，然后重复

我们试图实现的动作的性质非常适合递归函数。如果你还记得[我上一篇关于做苹果派](https://victoria.dev/verbose/understanding-array.prototype.reduce-and-recursion-using-apple-pie/)的文章，递归函数就是在特定条件下调用自己的函数。它看起来是这样的:

```
func checkItem(b *bag, i int, w int, is []item, matrix [][]int) {
	if i <= 0 || w <= 0 {
		return
	}

	pick := matrix[i][w]
	if pick != matrix[i-1][w] {
		b.addItem(is[i-1])
		checkItem(b, i-1, w-is[i-1].weight, is, matrix)
	} else {
		checkItem(b, i-1, w, is, matrix)
	}
}
```

如果我们在步骤 4 中描述的条件为真，我们的`checkItem()`函数会调用它自己。如果步骤 3 为真，它也调用自身，但使用不同的参数。

递归函数需要一个基本用例。在这个例子中，我们希望函数在用完值得比较的值时停止。因此，我们的基本情况是当`i`或`w`是`0`时。

下面是动态编程方法综合起来的样子:

```
func checkItem(b *bag, i int, w int, is []item, matrix [][]int) {
	if i <= 0 || w <= 0 {
		return
	}

	pick := matrix[i][w]
	if pick != matrix[i-1][w] {
		b.addItem(is[i-1])
		checkItem(b, i-1, w-is[i-1].weight, is, matrix)
	} else {
		checkItem(b, i-1, w, is, matrix)
	}
}

func dynamic(is []item, b *bag) *bag {
	numItems := len(is)          // number of items in knapsack
	capacity := b.maxItemsWeight // capacity of knapsack

	// create the empty matrix
	matrix := make([][]int, numItems+1) // rows representing items
	for i := range matrix {
		matrix[i] = make([]int, capacity+1) // columns representing grams of weight
	}

	// loop through table rows
	for i := 1; i <= numItems; i++ {
		// loop through table columns
		for w := 1; w <= capacity; w++ {

			// if weight of item matching this index can fit at the current capacity column...
			if is[i-1].weight <= w {
				// worth of this subset without this item
				valueOne := float64(matrix[i-1][w])
				// worth of this subset without the previous item, and this item instead
				valueTwo := float64(is[i-1].worth + matrix[i-1][w-is[i-1].weight])
				// take maximum of either valueOne or valueTwo
				matrix[i][w] = int(math.Max(valueOne, valueTwo))
			// if the new worth is not more, carry over the previous worth
			} else {
				matrix[i][w] = matrix[i-1][w]
			}
		}
	}

	checkItem(b, numItems, capacity, is, matrix)

	// add other statistics to the bag
	b.totalWorth = matrix[numItems][capacity]
	b.totalWeight = b.bagWeight + b.currItemsWeight

	return b
}
```

#### 动态编程结果

我们期望动态规划方法会给我们一个比贪婪算法更优化的解决方案。是吗？结果如下:

**包和物品的总重量:**6982 克

**打包物品的总价值:** 757

以下是我们的动态规划算法选择的项目，按价值排序:

![1*BYsgbQMgcl2jzkgSNIE3mA](img/7f37b771f428e1bcc843b53bdfbb25d9.png)

与贪婪算法相比，我们的动态编程解决方案有了明显的改进。我们的总价值 757 比贪婪算法的解决方案 716 高 41 点，而且重量也少了几克！

#### 输入排序顺序

在测试我的动态编程解决方案时，我在将输入传递给我的函数之前，对输入实现了 [Fisher-Yates shuffle 算法](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)，只是为了确保答案不会以某种方式依赖于输入的排序顺序。围棋中的洗牌是这样的:

```
rand.Seed(time.Now().UnixNano())

for i := range itemList {
	j := rand.Intn(i + 1)
	itemList[i], itemList[j] = itemList[j], itemList[i]
}
```

当然，我随后意识到 Go 1.10 现在有一个内置的洗牌。它的工作方式完全相同，如下所示:

```
rand.Shuffle(len(itemList), func(i, j int) {
	itemList[i], itemList[j] = itemList[j], itemList[i]
})
```

那么，项目的处理顺序会影响结果吗？嗯…

#### 突然…一个流氓重量出现！

事实证明，在某种程度上，答案确实取决于输入的顺序。当我几次运行我的动态编程算法时，我有时会看到一个不同的包的总重量，尽管总价值仍然是 757。在检查伴随两个不同总重量值的两组项目之前，我最初认为这是一个错误。一切都是一样的，除了一些变化，这些变化共同构成了一个不同的项目子集，占 757 个价值点中的 14 个。

在这种情况下，仅基于最高可能总价值的成功度量，就有两个同样最优的解决方案。打乱输入似乎会影响矩阵中条目的位置，从而影响`checkItem()`函数在矩阵中查找所选条目时所采用的路径。因为在两个项目集中具有最高可能价值的成功标准是相同的，所以我们没有一个唯一的解决方案——有两个！

作为一个学术练习，这两组题目都是正确答案。我们可以选择通过另一个指标来进一步优化，比如所有商品的总重量。以尽可能小的重量获得尽可能高的价值可以被视为一种理想的解决方案。

下面是第二个更简单的动态编程结果:

**包和物品的总重量:**6955 克

**打包物品的总价值:** 757

![1*_PzSh7J1yDo29RGh-DSlSg](img/45bd89676d5d44e9a4cc0440229474d0.png)

### 哪种方法更好？

#### 进行基准测试

Go 标准库的`testing`包让我们能够直接[对这两种方法进行基准测试。我们可以找出每个算法运行需要多长时间，以及每个算法使用多少内存。这里有一个简单的`main_test.go`文件:](https://golang.org/pkg/testing/#hdr-Benchmarks)

```
package main

import (
	"testing"
)

func Benchmark_greedy(b *testing.B) {
	itemList := readItems("objects.csv")
	for i := 0; i < b.N; i++ {
		minaal := bag{bagWeight: 1415, currItemsWeight: 0, maxItemsWeight: 5585}
		greedy(itemList, minaal)
	}
}

func Benchmark_dynamic(b *testing.B) {
	itemList := readItems("objects.csv")
	for i := 0; i < b.N; i++ {
		minaal := bag{bagWeight: 1415, currItemsWeight: 0, maxItemsWeight: 5585}
		dynamic(itemList, &minaal)
	}
}
```

我们可以运行`go test -bench=. -benchmem`来查看这些结果:

```
Benchmark_greedy-4       1000000              1619 ns/op            2128 B/op          9 allocs/op
Benchmark_dynamic-4         1000           1545322 ns/op         2020332 B/op         49 allocs/op 
```

#### 贪婪算法性能

在运行贪婪算法 1，000，000 次后，该算法的速度被可靠地测量为 0.001619 毫秒(翻译:非常快)。它需要 2128 字节或大约 2 千字节的内存，并且每次迭代需要 9 个不同的内存分配。

#### 动态编程性能

动态规划算法运行了 1000 次。它的速度被测量为 1.545322 毫秒或 0.001545322 秒(翻译:仍然相当快)。每次迭代需要 2，020，332 字节或大约 2 兆字节，以及 49 个不同的内存分配。

### 判决

选择解决任何编程问题的正确方法的一部分是考虑输入数据集的大小。在这种情况下，它是一个小的。在这种情况下，单遍贪婪算法总是比动态编程更快，对资源的需求也更少，原因很简单，因为它的步骤更少。我们的贪婪算法比我们的动态编程算法快了几乎两个数量级，并且对内存的需求更少。

然而，没有这些额外的步骤意味着从贪婪算法中获得最佳可能的解决方案是不可能的。

很明显，动态规划算法给了我们更好的数字:更低的权重，更高的整体价值。

#### 贪婪算法

*   总重量:6987 克
*   总价值:716 英镑

#### 动态规划

*   总重量:6955 克
*   总价值:757 英镑

小数据集上的动态编程在性能上有所欠缺，但在优化上有所弥补。那么问题就变成了额外的优化是否值得付出性能代价。

当然，“更好”是一种主观判断。如果速度和低资源使用是我们的成功标准，那么贪婪算法显然更好。如果包里物品的总价值是我们成功的衡量标准，那么动态规划显然更好。

然而，我们的场景是实际的，并且这些算法设计中只有一个返回了我选择的答案。在优化包中物品的最大可能总价值时，动态规划算法忽略了我价值最高但也最重的物品:我的笔记本电脑。没有它，附带的充电器和电缆、支架和键盘就没什么用了。

### 更好的算法设计

有一个简单的方法来改变动态编程方法，使笔记本电脑总是被包括在内:我们可以修改数据，使笔记本电脑的价值大于所有其他项目的价值总和。(试试看！)

也许在重新设计动态规划算法使其更加实用时，我们可以选择另一个更好地反映项目重要性的成功度量，而不是主观的价值。我们可以使用许多可能的指标来表示一个项目的价值。以下是一些好的代理示例:

*   使用项目所花费的时间
*   购买物品的初始成本
*   如果物品今天丢失，重置成本
*   使用该项目的产品的美元价值

出于同样的原因，贪婪算法的结果可以通过使用这些替代指标中的一个来改进。

一般来说，除了选择合适的方法来解决背包问题之外，以一种将场景的实用性转化为代码的方式来设计我们的算法是有帮助的。

对于更好的算法设计，有许多考虑因素超出了这篇介绍性文章的范围，我计划在以后的文章中讨论(其中的一些)。未来的算法很可能会在我下次旅行时决定我包里的东西，但我们还没到那一步。敬请期待！

*感谢阅读！我希望这篇文章能让您更好地了解这两种常见方法是如何工作的。如果你想了解更多关于我如何用一个随身携带的包生活的信息，请查看我在 herOneBag.com T2 的博客。*

祝你今天过得愉快。:)