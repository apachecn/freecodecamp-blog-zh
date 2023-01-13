# Golang 中的迭代——如何在 Go 中遍历数据结构

> 原文：<https://www.freecodecamp.org/news/iteration-in-golang/>

在编程中，迭代(通常称为循环)是一个过程，其中一个步骤重复 n 次，直到满足特定条件。

就像其他编程语言一样，Golang 有一种迭代不同数据结构和数据类型的方法，比如结构、映射、数组、字符串等等。

在本文中，您将了解到:

*   如何循环访问数组
*   如何在字符串中循环
*   如何在地图中循环
*   如何在 struts 中循环

## 如何在 Go 中循环遍历数组和切片

数组是存储相似类型数据的强大数据结构。您可以通过索引来识别和访问其中的元素。

在 Golang 中，您可以使用`for`循环遍历数组，方法是将变量 I 初始化为 0，然后递增变量，直到它达到数组的长度。

它们的语法如下所示:

```
for i := 0; i < len(arr); i++ {
    // perform an operation
}
```

举个例子，让我们遍历一个整数数组:

```
package main

import (
	"fmt"
)

func main() {
	numbers := []int{7, 9, 1, 2, 4, 5}

	for i := 0; i < len(numbers); i++ {
		fmt.Println(numbers[i])

	}
}
```

在上面的代码中，我们定义了一个名为`numbers`的整数数组，并通过初始化一个变量`i`来遍历它们。然后我们打印出数组中每个索引的值，同时递增`i`。

上面的代码输出以下内容:

```
7
9
1
2
4
5
```

我们也可以使用关键字`range`遍历一个数组，遍历整个数组。

语法如下所示:

```
for index, arr := range arr {
  // perform an operation	
}
```

例如:

```
package main

import (
	"fmt"
)

func main() {
	arr := []string{"a", "b", "c", "d", "e", "f"}

	for index, a := range arr {
		fmt.Println(index, a)
	}

}
```

在上面的代码中，我们定义了一个字符串数组，并使用关键字`for..range`遍历它的索引和值。

`for...range`的语法更简单，也更容易理解。您可以使用它来迭代不同的数据结构，如数组、字符串、映射、切片等等。

这将输出以下内容:

```
0 a
1 b
2 c
3 d
4 e
5 f
```

假设我们要忽略索引，只打印出数组的元素，您只需用下划线替换`index`变量。

例如:

```
package main

import (
	"fmt"
)

func main() {
	arr := []string{"a", "b", "c", "d", "e", "f"}

	for _, a := range arr {
		fmt.Println(a)
	}

}
```

在上面的代码中，我们修改了前面的例子，用下划线替换了`index`变量。我们这样做是为了忽略索引，而是输出数组的元素。

这将输出以下内容:

```
a
b
c
d
e
f
```

## 如何在 Go 中循环遍历字符串

编程中的字符串是不可变的——这意味着你不能在创建后修改它们。它们是一个或多个字符(如字母、数字或符号)的有序序列，可以是常量也可以是变量。

在 Golang 中，[字符串](https://golangbot.com/strings/)不同于 Python 或 JavaScript 等其他语言。它们被表示为一个 [UTF-8 字节序列](https://naveenr.net/unicode-character-set-and-utf-8-utf-16-utf-32-encoding/)，字符串中的每个元素代表一个字节。

使用`for...range`循环或常规循环遍历字符串。

例如:

```
package main

import (
	"fmt"
)

func main() {
	word := "Ab$du"

	for index, a := range word {
		fmt.Println(index, string(a))
	}
}
```

在上面的代码中，我们定义了一个包含不同字符的字符串，并循环遍历它的条目。在 Golang 中，字符串被表示为字节，这就是为什么我们需要在打印时将每个值转换为类型`string`。

这将输出:

```
0 A
1 b
2 $
3 d
4 u
```

如果我们没有将每个条目转换成字符串，Golang 会打印出字节表示。

例如:

```
package main

import (
	"fmt"
)

func main() {
	word := "Ab$du"

	for index, a := range word {
		fmt.Println(index, a)
	}
}
```

产出:

```
0 65
1 98
2 36
3 100
4 117
```

我们还可以通过使用常规的`for loop`来遍历字符串。

```
package main

import (
	"fmt"
)

func main() {
	word := "ab$du"

	for i := 0; i < len(word); i++ {
		fmt.Println(i, string(word[i]))
	}
}
```

## 如何在 Go 中循环浏览地图

在 Golang 中，map 是一种以键-值对存储元素的数据结构，其中键用于标识 map 中的每个值。它类似于 Python 和 Java 等其他语言中的字典和散列表。

您可以使用`for...range`语句遍历 Golang 中的`map`，获取索引及其相应的值。

例如:

```
package main

import (
	"fmt"
)

func main() {
	books := map[string]int{
		"maths":     5,
		"biology":   9,
		"chemistry": 6,
		"physics":   3,
	}
	for key, val := range books {
		fmt.Println(key, val)
	}
}
```

在上面的代码中，我们定义了一个存储书店详细信息的映射，将类型`string`作为键，将类型`int`作为值。然后我们使用`for..range`关键字遍历它的键和值。

在 Golang 中遍历地图没有任何特定的顺序，我们不应该期望键按照我们循环时定义的顺序返回。

此代码输出:

```
physics 3
maths 5
biology 9
chemistry 6
```

如果我们不想指定值而只返回键，我们就不定义值变量而只定义键变量。

例如:

```
package main

import (
	"fmt"
)

func main() {
	books := map[string]int{
		"maths":     5,
		"biology":   9,
		"chemistry": 6,
		"physics":   3,
	}
	for key := range books {
		fmt.Println(key)
	}
}
```

这将输出以下内容:

```
maths
biology
chemistry
physics
```

同样，如果我们对映射的键不感兴趣，我们使用下划线来忽略键，并为值定义一个变量。

例如:

```
package main

import (
	"fmt"
)

func main() {
	books := map[string]int{
		"maths":     5,
		"biology":   9,
		"chemistry": 6,
		"physics":   3,
	}
	for _, val := range books {
		fmt.Println(val)
	}
}
```

这将输出:

```
5
9
6
3
```

## 如何在 Go 中循环遍历结构

Struct 是 Golang 中的一种数据结构，用于将不同的数据类型组合成一种。与数组不同，struct 可以包含整数、字符串、布尔值等等——所有这些都在一个地方。

与 map 不同，在 map 中我们可以很容易地循环遍历它的键和值，在 Golang 中循环遍历一个结构需要使用一个名为`reflect`的包。这允许我们用任意类型修改对象。

例如，让我们创建一个结构并遍历它:

```
package main

import (
	"fmt"
	"reflect"
)

type Person struct {
	Name   string
	Age    int
	Gender string
	Single bool
}

func main() {
	ubay := Person{
		Name:   "John",
		Gender: "Female",
		Age:    17,
		Single: false,
	}
	values := reflect.ValueOf(ubay)
	types := values.Type()
	for i := 0; i < values.NumField(); i++ {
		fmt.Println(types.Field(i).Index[0], types.Field(i).Name, values.Field(i))
	}
}
```

这将输出:

```
0 Name John
1 Age 17
2 Gender Female
3 Single false
```

在上面的代码中，我们用不同的属性定义了一个名为`Person`的`struct`，并创建了一个`struct`的新实例。然后，我们使用`reflect`包来获取`struct`及其`type`的值。

通过使用常规的`for`循环，我们增加了初始化的变量`i`，直到它达到结构的长度。

我们使用`NumField`方法来获得结构中字段的总数。`types.Field(i).Index`方法返回结构中每个键的索引。`types.Field(i).Name`方法返回结构中每个键的字段名。并且`values.Field(i)`返回结构中每个键的值。

在本文中，您可以了解关于 reflect 包的更多信息:

[Learning to Use Go ReflectionMost of the time, variables, types, and functions in Go are pretty straightforward. When you need a type, you define a type: But sometimes you want to work with variables at runtime using information…![1*sHhtYhaCe2Uc3IU0IgKwIQ](img/891805fbb36bc04f4eccf5b9c18ff279.png)Jon BodnerCapital One Tech![1*WYmPAeP6c0pLJzNBf6UGyw](img/c1c7a3aeffa62b3f59e200ecbd9fb6c7.png)](https://medium.com/capital-one-tech/learning-to-use-go-reflection-822a0aed74b7)

Learn more about the reflect package in golang

## 结论

在本文中，我们探讨了如何在 Golang 中对不同的数据类型进行迭代。

虽然您可以使用`for`循环或`for..range`循环遍历数组、映射和字符串，但结构需要一个名为`reflect`的附加包来遍历它们的键和值。

我希望这篇文章能帮助你更好地理解 Golang 中的迭代。

感谢阅读。