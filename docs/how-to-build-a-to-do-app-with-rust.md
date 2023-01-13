# Rust 编程语言教程——如何构建待办事项应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-to-do-app-with-rust/>

自 2015 年首次开源发布以来，Rust 编程语言获得了社区的广泛关注。自 2016 年以来，它还被 StackOverflow 的开发者调查选为最受欢迎的编程语言。

Rust 由 Mozilla 设计，被认为是一种系统编程语言(像 C 或 C++)。它没有垃圾收集器，这使得它的性能非常好。但它的设计往往让它的观感非常“高级”。

Rust 的学习曲线被认为有些陡峭。我自己并不是这门语言的大师，但是在这篇教程中，我会尝试给你一些概念的实用方法，以帮助你更深入地挖掘。

## 我们将在本实践教程中构建什么

我决定遵循 JavaScript 应用的悠久传统，做一个待办事项应用作为我们的第一个项目。我们将使用命令行，所以有必要熟悉它。您还需要一些一般编程概念的知识。

此应用程序将在终端中运行。我们将把值存储为一个项目集合和一个表示其活动状态的布尔值。

## 我们将在此介绍的内容

*   Rust 的错误处理。
*   选项和空类型。
*   struts 和 impl。
*   终端输入输出
*   文件系统处理。
*   铁锈中的所有权和借款。
*   匹配模式。
*   迭代器和闭包。
*   使用外部板条箱。

## 在开始之前

在我们开始之前，来自一个有 JavaScript 背景的人的一些建议:

*   Rust 是一种强类型语言。这意味着当编译器不能为我们推断类型时，我们将不得不注意变量类型。
*   同样与 JavaScript 相反，没有 AFI 和 T2。这意味着我们必须键入分号(“；”)我们自己，除非它是一个函数的最后一个语句。在这种情况下，您可以省略`;`将它作为返回。

事不宜迟，我们开始吧。

## 如何入门 Rust

首先，下载 Rust 到你的电脑上。为此，请遵循 Rust 官方网站的[入门](https://www.rust-lang.org/learn/get-started)页面上的说明。

在那里，您还可以找到将该语言与您最喜欢的编辑器相集成的说明，以获得更好的体验。

除了 Rust 编译器本身，Rust 还附带了一个名为 [Cargo](https://doc.rust-lang.org/cargo/index.html) 的工具。Cargo 是 Rust 包管理器，对于 JavaScript 开发人员来说，它就像 npm 或 yarn。

要启动一个新项目，导航到您想要创建项目的位置，然后简单地运行`cargo new <project-name>`。在我的例子中，我已经决定将我的项目命名为“todo-cli ”,这样我就可以运行:

```
$ cargo new todo-cli 
```

现在导航到新创建的目录并列出其内容。您应该会在那里看到两个文件:

```
$ tree .
.
├── Cargo.toml
└── src
 └── main.rs 
```

在本教程的剩余部分，我们将继续处理`src/main.rs`文件，所以请打开它。

像许多其他语言一样，Rust 有一个将首先运行的主函数。`fn`是如何声明函数，而`println!`中的`!`是一个[宏](https://doc.rust-lang.org/book/ch19-06-macros.html)。你可能猜到了，这个节目是《 *hello world》的铁锈版！*”。

要构建并运行它，只需执行`cargo run`。

```
$ cargo run
Hello world! 
```

## 如何解读论点

我们的目标是让我们的 CLI 接受两个参数:第一个是动作，第二个是项目。

我们将从读取用户输入的参数并打印出来开始。

**用以下内容替换**主要内容:

```
let action = std::env::args().nth(1).expect("Please specify an action");
let item = std::env::args().nth(2).expect("Please specify an item");

println!("{:?}, {:?}", action, item); 
```

让我们从消化所有这些信息开始。

*   `let`[【doc】](https://doc.rust-lang.org/std/keyword.let.html)将值绑定到变量。
*   `std::env::args()`[【doc】](https://doc.rust-lang.org/std/env/fn.args.html)是从标准库的 *env* 模块引入的函数，返回程序启动时的参数。因为它是一个迭代器，我们可以用`nth()`函数访问存储在每个位置的值。位置 0 的参数是程序本身，这就是为什么我们从第一个元素开始读取。
*   `expect()`[【doc】](https://doc.rust-lang.org/std/option/enum.Option.html#method.expect)是为`Option`枚举定义的方法，该方法或者返回值，或者如果不存在，将立即终止程序(通俗地说就是死机)，返回提供的消息。

因为程序可以在没有参数的情况下运行，Rust 要求我们通过给我们一个选项类型来检查一个值是否真的被提供了:要么值在那里，要么不在那里。

作为程序员，我们有责任确保在每种情况下都采取适当的行动。

目前，如果不提供参数，我们将立即退出程序。

让我们运行程序并传递两个参数。为此，将它们附加在`--`之后。例如:

```
$ cargo run -- hello world!
    Finished dev [unoptimized + debuginfo] target(s) in 0.01s
     Running `target/debug/todo_cli hello 'world'\!''`
"hello", "world!" 
```

## 如何用自定义类型插入和保存数据

让我们思考一下我们的计划目标。我们希望读取用户给出的参数，更新我们的 todo 列表，并将其存储在某个地方以供使用。

为此，我们将实现我们自己的类型，在那里我们可以定义我们的方法来满足业务需求。

我们将使用 Rust 的[结构](https://doc.rust-lang.org/std/keyword.struct.html)，它让我们以一种干净的方式完成这两项工作。它避免了在主函数中编写所有代码。

### 如何定义我们的结构

由于我们将在接下来的步骤中大量输入 HashMap，所以我们可以将它纳入范围，并节省一些输入。

在我们文件的顶部添加这一行:

```
use std::collections::HashMap 
```

这将让我们直接使用`HashMap`而不需要每次都键入完整的路径。

在主函数下面，我们添加以下代码:

```
struct Todo {
    // use rust built in HashMap to store key - val pairs
    map: HashMap<String, bool>,
} 
```

这将定义我们的自定义 Todo 类型:一个具有名为“map”的单个字段的结构。

这个字段是一个[散列表](https://doc.rust-lang.org/std/collections/struct.HashMap.html)。你可以把它看作是 JavaScript 对象的一种*类型*，Rust 要求我们声明键和值的类型。

*   意味着我们有由字符串组成的键，和一个布尔值:活动状态。

### 如何向我们的结构中添加方法

方法就像常规的函数——它们用关键字`fn`表示，它们接受参数，它们有返回值。

然而，它们与常规函数的不同之处在于，它们是在结构的上下文中定义的，并且它们的第一个参数总是*`self`。*

 *我们将在新添加的结构下定义一个 *impl* (实现)块。

```
impl Todo {
    fn insert(&mut self, key: String) {
        // insert a new item into our map.
        // we pass true as value
        self.map.insert(key, true);
    }
} 
```

这个函数非常简单:它简单地将一个*引用*传递给结构体和一个键，并使用 HashMap 内置的 [insert](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.insert) 方法将其插入到我们的映射中。

两条非常重要信息:

*   **mut**[【doc】](https://doc.rust-lang.org/std/keyword.mut.html)使变量可变。
    在 Rust 中每个变量都是*默认不可变*。如果您想要更新一个值，您需要使用`mut`关键字选择可变性。因为使用我们的函数，我们通过添加一个新值来有效地改变我们的映射，所以我们需要将它声明为可变的。

*   **&**[【doc】](https://doc.rust-lang.org/std/primitive.reference.html)表示一个*引用*。你可以把变量想象成一个指向存储值的内存位置的指针，而不是“值”本身。

    在 Rust 术语中，这被称为**借用**，意味着函数实际上并不拥有这个值，而只是指向它的存储位置。

## 鲁斯特所有权制度概述

有了前面关于借用和引用的提示，现在是简要讨论所有权的好时机。

所有权是 Rust 最独特的特征。它使 Rust 程序员能够编写程序，而无需手动分配内存(如 C/C++)，同时仍然能够在没有垃圾收集器(如 JavaScript 或 Python)的情况下运行，垃圾收集器会不断查看程序的内存，以释放未使用的资源。

所有权制度有三个规则:

*   Rust 中的每个值都有一个变量:它的所有者。
*   每个值一次只能有一个所有者。
*   当所有者超出范围时，该值将被丢弃。

Rust 在编译时检查这些规则，这意味着如果你想在内存中释放一个值，你必须明确。
想一想这个例子:

```
fn main() {
 // the owner of the String is x
 let x = String::from("Hello");

 // we move the value inside this function.
 // now doSomething is the owner of x.
 // Rust will free the memory associated with x 
 // as soon as it goes out of "doSomething" scope.
 doSomething(x);

 // The compiler will throw an error since we tried to use the value x
 // but since we moved it inside "doSomething"
 // we cannot use it as we don't have ownership
 // and the value may have been dropped.
 println!("{}", x);
} 
```

这个概念被广泛认为是学习 Rust 时最难掌握的，因为它对许多程序员来说可能是一个新的概念。

你可以从 Rust 的官方文档中读到关于[所有权](https://doc.rust-lang.org/book/ch04-00-understanding-ownership.html)更深入的解释。

我们就不深究所有制的来龙去脉了。现在只要记住我上面提到的规则。在每一步中，试着想一想，我们是否需要“拥有”这些值，然后放弃它们，或者我们是否需要它的一个引用，这样它才能被保留。

例如，在上面的插入方法中，我们不想拥有`map`，因为我们仍然需要它在某个地方存储它的数据。只有这样，我们才能最终释放分配的内存。

### 如何将地图保存到磁盘

由于这是一个演示应用程序，我们将采用最简单的长期存储解决方案:将地图写入磁盘文件。

让我们在我们的`impl`块中创建新方法。

```
impl Todo {
    // [rest of the code]
    fn save(self) -> Result<(), std::io::Error> {
        let mut content = String::new();
        for (k, v) in self.map {
            let record = format!("{}\t{}\n", k, v);
            content.push_str(&record)
        }
        std::fs::write("db.txt", content)
    }
} 
```

*   注释函数返回的类型。我们正在返回一个`Result`。
*   我们遍历地图，格式化每个字符串，用制表符分隔键和值，用新行分隔每一行。
*   我们将格式化后的字符串放入一个内容变量。
*   我们将`content`写在一个名为`db.txt`的文件中。

注意到`save` *拥有自我的所有权*是很重要的。
这是一个任意的决定，因此如果我们在调用 save 后无意中试图更新映射，编译器会阻止我们(因为 self 的内存将被释放)。

这是个人决定“强制”保存为最后使用的方法。这是一个很好的例子，展示了如何使用 Rust 的内存管理来创建更严格的无法编译的代码(这有助于防止开发过程中的人为错误)。

### 如何在 main 中使用 struct

现在我们有了这两种方法，就可以把它们派上用场了。我们从读到所提供的参数的地方就离开了 main。现在，如果提供的操作是“add ”,我们将把该项插入到文件中，并存储起来供以后使用。

在两个参数绑定下面添加这几行:

```
fn main() {
    // ...[arguments bindig code]

    let mut todo = Todo {
        map: HashMap::new(),
    };
    if action == "add" {
        todo.insert(item);
        match todo.save() {
            Ok(_) => println!("todo saved"),
            Err(why) => println!("An error occurred: {}", why),
        }
    } 
} 
```

让我们看看我们在做什么:

*   让我们实例化一个结构，将它绑定为可变的。
*   我们使用`.`符号调用`TODO insert`方法。
*   我们[匹配](https://doc.rust-lang.org/std/keyword.match.html)从 save 函数返回的结果，并在屏幕上为这两种情况打印一条消息。

我们来测试一下。导航到您的终端并键入:

```
$ cargo run -- add "code rust"
todo saved 
```

让我们检查保存的项目:

```
$ cat db.txt
code rust true 
```

到目前为止，你可以在这个[要点](https://gist.github.com/Marmiz/b67e98c2fc7be3561d124294cf3cb6ac)中找到完整的代码片段。

## 如何从文件中读取

现在我们的程序有一个根本性的缺陷:每次我们“添加”时，我们都在覆盖地图，而不是更新它。这是因为我们每次运行程序时都会创建一个新的空地图。让我们解决这个问题。

### 在 TODO 中添加新函数

我们将为 Todo 结构实现一个新函数。一旦被调用，它将读取我们文件的内容，并返回我们用先前存储的值填充的 Todo。注意，这不是一个方法，因为它没有将`self`作为第一个参数。

我们将称之为`new`，这只是一个 Rust 约定(见之前使用的 HashMap::new())。

让我们在 impl 块中添加以下代码:

```
impl Todo {
    fn new() -> Result<Todo, std::io::Error> {
        let mut f = std::fs::OpenOptions::new()
            .write(true)
            .create(true)
            .read(true)
            .open("db.txt")?;
        let mut content = String::new();
        f.read_to_string(&mut content)?;
        let map: HashMap<String, bool> = content
            .lines()
            .map(|line| line.splitn(2, '\t').collect::<Vec<&str>>())
            .map(|v| (v[0], v[1]))
            .map(|(k, v)| (String::from(k), bool::from_str(v).unwrap()))
            .collect();
        Ok(Todo { map })
    }

// ...rest of the methods
} 
```

不要担心，如果这感觉有点势不可挡。我们这次使用了更函数式的编程风格，主要是为了展示和介绍 Rust 支持其他语言中的许多范例，比如迭代器、闭包和 lambda 函数。

让我们看看这里发生了什么:

*   我们正在定义一个`new`函数，它将返回一个或者是`Todo`结构或者是`io:Error`的结果。
*   我们通过定义各种 [OpenOptions](https://doc.rust-lang.org/std/fs/struct.OpenOptions.html) 来配置如何打开“db.txt”文件。最值得注意的是`create(true)`标志，如果文件不存在，它将创建该文件。
*   `f.read_to_string(&mut content)?`读取文件中的所有字节，并将它们附加到`content`字符串中。
    *注意:*为了使用`read_to_string`方法，记得在文件的顶部加上`use std::io::Read;`和其他使用语句。
*   我们需要将文件的字符串类型转换成 HashMap。我们通过将一个 map 变量绑定到下面这行代码来实现这个目的:`let map: HashMap<String, bool>`。这是编译器为我们推断类型有困难的情况之一，所以我们自己声明它。
*   lines[【doc】](https://doc.rust-lang.org/std/primitive.str.html#method.lines)在一个字符串的每一行上创建一个迭代器，这意味着现在我们将迭代我们文件的每一个条目，因为我们在每个条目的末尾用一个`/n`来格式化它。
*   map[【doc】](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.map)接受一个闭包，并在迭代器的每个元素上调用它。
*   `line.splitn(2, '\t')`[【doc】](https://doc.rust-lang.org/std/primitive.str.html#method.splitn)将制表符上的行拆分。
*   文档中描述的`collect::<Vec<&str>>()`[【doc】](https://doc.rust-lang.org/core/iter/trait.Iterator.html#method.collect)是标准库中最强大的方法之一:它将一个迭代器转换成一个相关的集合。
    在这里，我们告诉 map 函数，通过在方法后面追加`::Vec<&str>`，将拆分后的字符串转换为借用的字符串片段的向量。这告诉编译器在操作结束时我们想要哪个集合。
*   然后，为了方便起见，我们使用`.map(|v| (v[0], v[1]))`将其转换为一个元组。
*   然后我们使用`.map(|(k, v)| (String::from(k), bool::from_str(v).unwrap()))`将元组的两个元素转换成一个字符串和一个布尔值。
    *注意:*记住在文件的顶部加上`use std::str::FromStr;`和其他使用语句，以便能够使用`from_str`方法。
*   我们最终将它们收集到我们的散列表中。这一次我们不需要声明类型，因为 Rust 从绑定声明中推断出了它。
*   最后，如果我们从未遇到任何错误，我们用`Ok(Todo { map })`将我们的结构返回给调用者。
    注意，这里很像 JavaScript，如果键和变量在结构中有相同的名字，我们可以使用更短的符号。

唷！

![dancing ferris.](img/3a40826661f87a8334e96643b14ace2b.png)

You are doing great! Image credits: https://rustacean.net/ 

### 另一种方法

尽管 map 通常被认为更符合习惯，但上面的代码也可以用一个`for`循环来实现。随便用你最喜欢的那个。

```
fn new() -> Result<Todo, std::io::Error> {
    // open the db file
    let mut f = std::fs::OpenOptions::new()
        .write(true)
        .create(true)
        .read(true)
        .open("db.txt")?;
    // read its content into a new string   
    let mut content = String::new();
    f.read_to_string(&mut content)?;

    // allocate an empty HashMap
    let mut map = HashMap::new();

    // loop over each lines of the file
    for entries in content.lines() {
        // split and bind values
        let mut values = entries.split('\t');
        let key = values.next().expect("No Key");
        let val = values.next().expect("No Value");
        // insert them into HashMap
        map.insert(String::from(key), bool::from_str(val).unwrap());
    }
    // Return Ok
    Ok(Todo { map })
} 
```

上面的代码在功能上等同于以前使用的更“功能化”的方法。

### 如何使用新功能

在 main 中，只需用以下代码将 binging 更新到我们的 todo 变量:

```
let mut todo = Todo::new().expect("Initialisation of db failed"); 
```

现在，如果我们回到终端，运行一系列“add”命令，我们应该会看到数据库正确更新:

```
$ cargo run -- add "make coffee"
todo saved
$ cargo run -- add "make pancakes"
todo saved
$ cat db.txt
make coffee     true
make pancakes   true 
```

你可以在这个[要点](https://gist.github.com/Marmiz/b659c7835054d25513106e3804c4539f)中找到迄今为止编写的完整代码。

## 如何更新集合中的值

就像所有的 TODO 应用程序一样，我们不仅希望能够添加项目，还希望能够切换项目，并将其标记为已完成。

### 如何添加完整的方法

为此，让我们在结构中添加一个名为“complete”的新方法。在其中，我们引用一个键，并更新值，或者如果键不存在，则返回`None`。

```
impl Todo {
// [Rest of the TODO methods]

  fn complete(&mut self, key: &String) -> Option<()> {
      match self.map.get_mut(key) {
          Some(v) => Some(*v = false),
          None => None,
      }
  }
} 
```

让我们看看这里发生了什么:

*   我们声明了我们的函数返回类型:一个空的`Option`。
*   整个方法返回匹配表达式的结果，该结果将是空的`Some()`或`None`。
*   `self.map.get_mut`[【doc】](https://doc.rust-lang.org/std/collections/struct.HashMap.html#method.get_mut)将给我们一个对 key 值的可变引用，或者如果集合中没有这个值，就给我们一个`None`。
*   我们使用`*`[【doc】](https://doc.rust-lang.org/book/appendix-02-operators.html)操作符去引用该值并将其设置为 false。

### 如何使用完整的方法

我们可以像以前使用 insert 一样使用“complete”方法。

在`main`中，让我们通过使用`else if`语句来检查作为参数传递的动作是否“完成”:

```
// in the main function

if action == "add" {
    // add action snippet
} else if action == "complete" {
    match todo.complete(&item) {
        None => println!("'{}' is not present in the list", item),
        Some(_) => match todo.save() {
            Ok(_) => println!("todo saved"),
            Err(why) => println!("An error occurred: {}", why),
        },
    }
} 
```

是时候分析我们在这里做什么了:

*   我们匹配由`todo.complete(&item)`方法返回的选项。
*   如果情况属实，我们会向用户发出警告，以获得更好的体验。
    我们使用`&item`将 item 作为引用传递给“todo.complete”方法，以便该值仍然归该函数所有。这意味着我们可以在下面的代码行中使用它作为我们的`println!`宏。
    如果我们不这样做，这个值就会被“完成”所拥有，并被丢弃在那里。
*   如果我们检测到`Some`值已经返回，我们调用`todo.save`将更改永久存储到我们的文件中。

和以前一样，您可以在这个[要点](https://gist.github.com/Marmiz/1480b31e8e0890e8745e7b6b44a803b8)中找到目前为止编写的代码的快照。

## 尝试运行该程序

是时候在我们的终端中试用我们在本地开发的应用程序了。让我们从删除 db 文件开始。

```
$ rm db.txt 
```

然后添加和修改一些待办事项:

```
$ cargo run -- add "make coffee"
$ cargo run -- add "code rust"
$ cargo run -- complete "make coffee"
$ cat db.txt
make coffee     false
code rust       true 
```

这意味着在这些命令的末尾，我们有一个已完成的动作(“煮咖啡”)和一个未完成的动作:“代码生锈”。

假设我们想再次煮咖啡:

```
$ cargo run -- add "make coffee
$ cat db.txt
make coffee     true
code rust       true 
```

## 额外收获:如何用 Serde 将其存储为 JSON

这个程序，即使很小，也在运行。但是让我们稍微改变一下。来自 JavaScript 世界的我决定将我的值存储为 JSON 文件，而不是纯文本文件。

我们将借此机会了解如何安装和使用 Rust 开源社区中名为 [crates.io](https://crates.io/) 的软件包。

### 如何安装 serde

要在我们的项目中安装一个新的包，打开`cargo.toml`文件。在底部，您应该会看到一个`[dependencies]`字段:只需将以下内容添加到文件中:

```
[dependencies]
serde_json = "1.0.60" 
```

仅此而已。下一次，cargo 将编译我们的程序，并将下载和包含新的包以及我们的代码。

### 如何更新 Todo::New

我们想使用 Serde 的第一个地方是当我们读取 db 文件时。现在不是读 a”。txt "，我们要读取一个 JSON 文件。

在`impl`块中，让我们更新`new`函数:

```
// inside Todo impl block

fn new() -> Result<Todo, std::io::Error> {
    // open db.json
    let f = std::fs::OpenOptions::new()
        .write(true)
        .create(true)
        .read(true)
        .open("db.json")?;
    // serialize json as HashMap
    match serde_json::from_reader(f) {
        Ok(map) => Ok(Todo { map }),
        Err(e) if e.is_eof() => Ok(Todo {
            map: HashMap::new(),
        }),
        Err(e) => panic!("An error occurred: {}", e),
    }
} 
```

值得注意的变化是:

*   file 选项不再有`mut f`绑定，因为我们不需要像以前一样手动将内容分配到一个字符串中。Serde 会帮我们处理的。
*   我们将文件扩展名更新为`db.json`。
*   `serde_json::from_reader`[【doc】](https://docs.serde.rs/serde_json/fn.from_reader.html)将为我们反序列化文件。它会干扰 map 的返回类型，并试图将我们的 JSON 转换成兼容的 HashMap。如果一切顺利，我们像以前一样返回我们的`Todo`结构。
*   `Err(e) if e.is_eof()`是一个[匹配守卫](https://doc.rust-lang.org/reference/expressions/match-expr.html#match-guards)，它让我们可以优化匹配声明的行为。
    如果 Serde 返回一个过早的 EOF(文件结束)错误，这意味着文件完全是空的(例如在第一次运行时，或者如果我们删除了文件)。在这种情况下，我们从错误中恢复并返回一个空的 HashMap。
*   对于所有其他错误，立即退出程序。

### 如何更新 Todo.save

我们希望使用 Serde 的另一个地方是当我们将地图保存为 JSON 时。为此，将 impl 块中的`save`方法更新为:

```
// inside Todo impl block
fn save(self) -> Result<(), Box<dyn std::error::Error>> {
    // open db.json
    let f = std::fs::OpenOptions::new()
        .write(true)
        .create(true)
        .open("db.json")?;
    // write to file with serde
    serde_json::to_writer_pretty(f, &self.map)?;
    Ok(())
} 
```

像以前一样，让我们看看我们在这里改变了什么:

*   `Box<dyn std::error::Error>`。这次我们返回一个包含 Rust 通用错误实现的[框](https://doc.rust-lang.org/std/boxed/index.html)。
    简单来说，盒子就是内存中分配的指针。
    由于我们可能在打开文件时返回一个文件系统错误，或者在转换文件时返回一个 Serde 错误，我们并不知道我们的函数会返回这两个错误中的哪一个。因此，我们返回一个指向可能错误的指针，而不是错误本身，以便调用者处理它们。
*   我们当然已经将文件名更新为`db.json`来匹配。
*   最后，我们让 Serde 来完成繁重的工作，并将我们的 HashMap 写成一个 JSON 文件(打印得很漂亮)。
*   记住从文件顶部删除`use std::io::Read;`和`use std::str::FromStr;`,因为我们不再需要它们了。

仅此而已。现在你可以运行你的程序并检查保存到文件的输出。如果一切顺利，您现在应该看到您的 todos 保存为 JSON。

你可以在这个[要点](https://gist.github.com/Marmiz/541c3ccea832a27bfb60d4882450a4a8)中找到完整的代码。

## 结束语、提示和其他资源

这是一段相当长的旅程，我很荣幸你带我一起走。我希望你能从这篇介绍中学到一些东西，并激发你的好奇心。不要忘记，我们使用的是一种非常“低级”的语言，然而审查代码对大多数人来说可能感觉非常熟悉。

这也是 Rust 吸引我的地方——它让我能够编写速度极快、内存高效的代码，而不用担心随之而来的责任问题:我知道编译器会在那里等我，甚至在我的代码可能运行之前就停止它。

在结束之前，我想与您分享一些额外的提示和资源，以帮助您在 Rust 之旅中继续前进:

*   Rust fmt 是一个非常方便的工具，你可以运行它来按照一致的模式格式化你的代码。不再浪费时间配置你最喜欢的插件。
*   `cargo check`[【doc】](https://doc.rust-lang.org/cargo/commands/cargo-check.html)将尝试编译你的代码而不运行:这是开发时非常有用的命令，你只是想检查代码的正确性而不实际运行它。
*   Rust 附带了一个集成测试套件和一个生成文档的工具: [cargo test](https://doc.rust-lang.org/cargo/commands/cargo-test.html) 和 [cargo doc](https://doc.rust-lang.org/cargo/commands/cargo-rustdoc.html) 。这一次我们没有触及它们，因为教程看起来相当密集。也许在将来。

要了解更多关于这门语言的知识，我认为最好的资源是:

*   官方 [Rust 网站](https://www.rust-lang.org/)，所有信息都在这里汇集。
*   如果你喜欢通过聊天互动，Rust 的 [Discord](https://discord.gg/rust-lang) 服务器有一个非常活跃和有用的社区。
*   如果您喜欢通过阅读来学习，那么“[Rust 编程语言](https://doc.rust-lang.org/book/title-page.html)”一书是您的正确选择。
*   如果你是一个视频类型，Ryan Levick 的[Rust](https://youtu.be/WnWGO-tLtLA)视频系列介绍是一个惊人的资源。

你可以在 [GitHub](https://github.com/Marmiz/todo-cli) 上找到这篇文章的源代码。

封面图片来自[https://rustacean.net/](https://rustacean.net/)。

感谢阅读和快乐编码！*