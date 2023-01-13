# 用代码示例解释 Go 中的泛型

> 原文：<https://www.freecodecamp.org/news/generics-in-golang/>

泛型是几年前为 go 提出的，它们最终在今年早些时候被该语言接受。他们计划在今年年底正式发布。

泛型真的会对围棋产生怎样的影响？它会改变我们编码的方式吗？

为了真正回答这些问题，我们需要看看泛型是如何工作的。方便的是，开发者为我们提供了一个 [web 编译器](https://go2goplay.golang.org/),我们可以在那里自己试验泛型。

## Go 中泛型真正改变了什么？

![image-316](img/b11f6d4c2ec17c1c06b42bb1352faa72.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

泛型允许我们的函数或数据结构接受以泛型形式定义的几种类型。

为了真正理解这意味着什么，让我们看一个非常简单的案例。

假设你需要做一个函数，取一个切片并打印出来。那么你可以写这种类型的函数:

```
func Print(s []string) {
	for _, v := range s {
		fmt.Print(v)
	}
}
```

简单吧？如果我们希望切片是一个整数呢？你需要为此制定一个新的方法:

```
func Print(s []int) {
	for _, v := range s {
		fmt.Print(v)
	}
}
```

这些解决方案似乎是多余的，因为我们只是改变了参数。但目前，这就是我们在 Go 中解决它的方法，而不是把它变成某种接口。

现在有了泛型，它们将允许我们像这样声明我们的函数:

```
func Print[T any](s []T) {
	for _, v := range s {
		fmt.Print(v)
	}
}
```

在上面的函数中，我们声明了两件事:

1.  我们有 T，它是关键字`any`的类型(这个关键字被明确定义为泛型的一部分，表示任何类型)
2.  还有我们的参数，这里我们有变量`s`，它的类型是`T`的一个片。

我们现在可以这样调用我们的方法:

```
func main() {
	Print([]string{"Hello, ", "playground\n"})
	Print([]int{1,2,3})
} 
```

任何类型的变量都有一个方法——很简洁，是吧？

这只是泛型最基本的实现之一。但目前看来还不错。

让我们进一步探索，看看泛型能让我们走多远。

## 泛型的局限性

![image-317](img/82e2efd5c8557db85cfcbe0dafec9a34.png)

Photo by [Nick Tiemeyer](https://unsplash.com/@nickeedoo?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

我们已经看到了泛型能做什么。它们让我们指定一个可以接受任何类型参数的函数。

但是我之前举的例子很简单。泛型能带我们走多远是有限制的。例如，打印非常简单，因为 Golang 可以打印出任何类型的变量。

如果我们想做更复杂的事情呢？假设我们已经为一个结构定义了自己的方法，并希望将其命名为:

```
package main

import (
	"fmt"
)

type worker string

func (w worker) Work(){
	fmt.Printf("%s is working\n", w)
}

func DoWork[T any](things []T) {
    for _, v := range things {
        v.Work()
    }
}

func main() {
	var a,b,c worker
	a = "A"
	b = "B"
	c = "C"
	DoWork([]worker{a,b,c})	
} 
```

你会得到这个:

```
type checking failed for main
prog.go2:25:11: v.Work undefined (type bound for T has no method Work)
```

它无法运行，因为在函数内部处理的切片属于类型`any`，并且它没有实现方法`Work`，这使得它无法运行。

不过，我们实际上可以通过使用一个接口来实现它:

```
package main

import (
	"fmt"
)

type Person interface {
    Work()
}

type worker string

func (w worker) Work(){
	fmt.Printf("%s is working\n", w)
}

func DoWork[T Person](things []T) {
    for _, v := range things {
        v.Work()
    }
}

func main() {
	var a,b,c worker
	a = "A"
	b = "B"
	c = "C"
	DoWork([]worker{a,b,c})
} 
```

它会打印出这个:

```
A is working
B is working
C is working
```

它与接口一起工作，但是只有一个没有泛型的接口也很好:

```
package main

import (
	"fmt"
)

type Person interface {
    Work()
}

type worker string

func (w worker) Work(){
	fmt.Printf("%s is working\n", w)
}

func DoWorkInterface(things []Person) {
    for _, v := range things {
        v.Work()
    }
}

func main() {
	var d,e,f worker
	d = "D"
	e = "E"
	f = "F"
	DoWorkInterface([]Person{d,e,f})
} 
```

这将为我们提供以下结果:

```
D is working
E is working
F is working
```

使用泛型只会给我们的代码增加额外的逻辑。因此，如果只使用接口就足够了，我看不出有任何理由在代码中添加泛型。

泛型仍处于开发的早期阶段，它们在进行复杂处理时仍有局限性。

## 拘束地玩耍

![image-318](img/3d6c821ccc32f9800c84b9f8c3ff2ab0.png)

Photo by [Paulo Brandao](https://unsplash.com/@pbrandao?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

早些时候，我们遇到了通用约束的`any`类型。除了这种类型，我们还可以使用其他一些约束。

其中一个约束是`comparable`。让我们看看它是如何工作的:

```
func Equal[T comparable](a, b T) bool {
    return a == b
}

func main() {
	Equal("a","a")
}
```

除此之外，我们还可以尝试像这样制定自己的约束条件:

```
package main

import(
	"fmt"
)

type Number interface {
    type int, float64
}

func MultiplyTen[T Number](a T) T{
	return a*10
}

func main() {
	fmt.Println(MultiplyTen(10))
	fmt.Println(MultiplyTen(5.55))
}
```

我认为这很好——我们可以用一个函数来表达一个简单的数学表达式。通常我们最终会做两个函数来接收它，或者我们会使用反射，所以我们只写一个函数。

虽然这很酷，但是我们仍然需要做一些实验来制定我们自己的约束。现在知道他们的局限性还为时过早。我们应该小心不要滥用它，只有当我们真的确定需要它的时候才使用它。

## 使用泛型的其他方式

![image-319](img/d6a906081758755972c49774adb08d72.png)

Photo by [Marcelo Franchi](https://unsplash.com/@jotaemee?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

除了将泛型用作函数的一部分，您还可以将它们声明为变量，如下所示:

```
type GenericSlice[T any] []T
```

你可以把它作为一个函数的参数，也可以用它来构造方法:

```
func (g GenericSlice[T]) Print() {
	for _, v := range g {
		fmt.Println(v)
	}
}

func Print [T any](g GenericSlice[T]) {
	for _, v := range g {
		fmt.Println(v)
	}
}

func main() {
	g := GenericSlice[int]{1,2,3}

	g.Print() //1 2 3
	Print(g) //1 2 3
}
```

根据您的需要，用法会有所不同。我所能说的是，我们仍然需要更多地试验泛型，看看什么用例工作得最好。

## 我对仿制药的看法

泛型仍然处于非常早期的阶段(它们甚至还没有出来！)，但我对它们的制作方式印象深刻。实现泛型不需要很多复杂的术语和库，这种简单性非常好。

在几个用例中，我已经看到使用泛型会更好(就像使用 multiply 方法的情况)。有一点很多人似乎很困惑，那就是泛型可能是使用接口(包括接口{}类型和接口实现)的替代品。

我的建议是不要认为泛型可以替代任何东西。泛型只是在我们的编码生活中提供给我们的另一个工具。此外，泛型可能看起来既漂亮又酷，您可能希望在代码的每个块中都使用它们。但是不要过度使用它们——只在真正需要的时候使用，而不是在适合的时候使用。

仅此而已。感谢您阅读我的文章，我真心希望泛型能对您有用。

最后，大声喊出这个[站点](https://bitfieldconsulting.com/golang/generics)，它是我写这篇文章时的一个很好的参考。它解释了许多关于 Go 中泛型的背景知识。

享受泛型带来的乐趣！