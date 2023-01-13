# 类型脚本泛型–用例及示例

> 原文：<https://www.freecodecamp.org/news/typescript-generics-use-case-example/>

在本教程中，您将学习 TypeScript 中泛型的基础知识。我们将讨论如何使用它们以及它们何时在您的代码中有用。

## 泛型的用例

让我们从一个简单的例子开始，您希望打印传递的参数的值:

```
function printData(data: number) {
    console.log("data: ", data);
}

printData(2);
```

**printData.ts**

现在，让我们假设您想让`printData`成为一个更通用的函数，您可以向它传递任何类型的参数，比如:**数字** / **字符串** / **布尔型**。因此，您可能会考虑采用如下方法:

```
function printData(data: number | string | boolean) {
    console.log("data: ", data);
}

printData(2);
printData("hello");
printData(true);
```

**printData-new.ts**

但是在将来，您可能希望使用相同的函数打印一组数字。在这种情况下，类型会增加，维护所有这些不同的类型会变得很麻烦。

这时，仿制药出现了。

## 泛型在 TS 中如何工作

泛型就像变量——确切地说，是类型变量——将类型(例如数字、字符串、布尔值)存储为值。

因此，您可以用如下所示的泛型来解决我们上面讨论的问题:

```
function printData<T>(data: T) {
    console.log("data: ", data);
}

printData(2);
printData("hello");
printData(true);
```

**printData-generics.ts**

在上面的例子`printData-generics.ts`中，语法略有不同:

1.  在函数名`<T>`后面的尖括号中使用了一个类型变量
2.  然后将类型变量分配给参数`data: T`

让我们更深入地探讨一下这些差异。

要使用泛型，您需要使用尖括号，然后在其中指定一个类型变量。开发者一般用`T`、`X`、`Y`。但它可以是任何东西，取决于你的喜好。

然后，您可以将与类型相同的变量名赋给函数的参数。

现在，无论你传递给函数什么参数，它都会被推断出来，不需要在任何地方硬编码类型。

即使您将一个数字数组或一个对象传递给`printData`函数，一切都会正常显示，TS 不会抱怨:

```
function printData<T>(data: T) {
    console.log("data: ", data);
}

printData(2);
printData("hello");
printData(true);
printData([1, 2, 3, 4, 5, 6]);
printData([1, 2, 3, "hi"]);
printData({ name: "Ram", rollNo: 1 });
```

**printData-new.ts**

让我们看另一个例子:

```
function printData<X,Y>(data1: X, data2: Y) {
    console.log("Output is: ", data1, data2);
}

printData("Hello", "World");
printData(123, ["Hi", 123]);
```

**generics-example2.ts**

在上面的例子中，我们向`printData`传递了两个参数，并用`X`和`Y`来表示这两个参数的类型。`X`指自变量的第一个值，`Y`指自变量的第二个值。

这里也没有明确指定`data1`和`data2`的类型，因为 TypeScript 在泛型的帮助下处理类型推断。

### 如何在接口中使用泛型

你甚至可以在接口中使用泛型。让我们借助一个代码片段来看看这是如何工作的:

```
interface UserData<X,Y> {
    name: X;
    rollNo: Y;
}

const user: UserData<string, number> = {
    name: "Ram",
    rollNo: 1
}
```

**interface-generic.ts**

在上面的代码片段中，`<string, number>`被传递给接口`UserData`。通过这种方式，`UserData`成为了一个可重用的接口，其中任何数据类型都可以根据用例进行分配。

在本例中，`name`和`rollNo`将始终分别为`string`和`number`。但是这个例子是为了展示如何在 TS 中使用泛型和接口。

### 感谢阅读！

如果你觉得这篇文章有用，请与你的朋友和同事分享。