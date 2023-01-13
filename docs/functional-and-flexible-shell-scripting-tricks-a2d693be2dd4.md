# 实用灵活的 shell 脚本技巧

> 原文：<https://www.freecodecamp.org/news/functional-and-flexible-shell-scripting-tricks-a2d693be2dd4/>

作者洪斌·李

### Shell 脚本与 python 或 Perl

现在都 2020 年了，谁还写 shell 脚本？我说的对吗？显然我知道。\_(ツ)_/

这里的和这里的有一些很好的论据，主要围绕两件事:

1.  Shell 存在于所有 Unix 系统中，并利用系统默认特性。
2.  Shell 是一个“交互式命令功能”,用于在运行过程中获取用户输入。

另外，[这里的](https://stackoverflow.com/questions/5725296/difference-between-sh-and-bash)是关于`sh`和`bash`之间差异的额外相关阅读。

### 争论

在某些情况下，您需要将一个参数(或期望一个)传递到脚本中，就像您可能将一个参数传递到一个函数中一样。在这种情况下，您将使用类似于`$1`作为第一个参数，`$2`作为第二个参数。下面是一个示例，它看起来会是什么样子:

在脚本`run_this.sh`中:

```
echo "The input message was $1."
```

运行命令:

```
./run_this.sh userInputThe input message was userInput.
```

*注意:参数由空格分隔，所以如果你想输入一个包含空格的字符串作为参数，它可能需要类似于`./run_this.sh "user input"`的东西，这样`"user input"`将被完全算作`$1`。*

如果你不确定用户输入有多长，并且你想全部捕获，你可以使用`$@`来代替。在下面的例子中，我接受了整个字符串，并根据字符串之间的空格将它们分成一个字符串数组，然后逐字打印出来。

在脚本`run_this.sh`中:

```
userInputs=($@)for i in "${userInputs[@]}";; do  echo "$i"done
```

运行命令:

```
./run_this.sh who knows how long this can gowhoknowshowlongthiscango
```

### 功能

如果你做过任何种类的编程，你应该熟悉*函数*的概念。它基本上是一组你会一遍又一遍重复的命令/操作。您可以将它们放入一个函数中，而不是在代码中重复多次。然后只需调用有效减少需要编写的代码行的函数。

补充说明:如果你还不知道的话，LOC 对于编程方面的任何度量来说都是一个可怕的指标。不要听我的，听比尔·盖茨 : 的

> "用代码行数来衡量编程进度就像用重量来衡量飞机制造进度一样."

下面是一个普通函数的样子:

```
# Declaring the functiondoSomething() {
```

```
}
```

```
# Calling the functiondoSomething
```

非常简单易懂。现在，这里有一些 shell 脚本中的函数和普通编程语言之间的区别。

### 因素

如果你要在 Java 中将一个参数传递/使用到一个函数中，你必须在函数声明中声明它们。它们看起来像这样。

```
public static void main(String[] args) {    doSomething("random String");}
```

```
private static void doSomething (String words) {    System.out.println(words);}
```

然而，在 shell 中，它们根本不需要类型或名称的声明。它们中的每一个都像一个独立的脚本，存在于脚本本身中。如果您要使用 param，只需传入它并调用它，就像您在顶层接受该脚本的输入一样。大概是这样的:

```
doSomething() {    echo $1}
```

```
doSomething "random String"
```

1.  与上面类似，如果你想接收所有内容，你将使用`$@`而不是`$1`，因为`$1`将只使用第一个输入(而`$2`用于第二个输入，等等。).
2.  函数需要在被调用之前声明。(通常在任何主操作之前的文件的开始。)

### 返回

假设我们创建了一个名为`run_this.sh`的脚本，如下所示:

```
doSomething() {    echo "magic"    return 0}
```

```
output=`doSomething`echo $output
```

现在让我们运行它，看看什么被赋给了`output`变量。

```
$ ./run_this.shmagic
```

注意不是`0`，而是显示`magic`。这是因为当您执行`output=`doSomething``时，它会将输出消息分配给`output`而不是返回值，因为输出消息是您在 shell 脚本中传达几乎所有内容的方式。

那么什么时候使用`return`调用有意义呢？当您将它用作 if 语句的一部分时。大概是这样的:

在脚本`run_this.sh`中:

```
doSomething() {    echo "magic"    return 0}
```

```
if doSomething; then    echo "Its true!"fi
```

运行命令:

```
./run_this.shIts true!
```

在这里，`return 0`指的是`true`，而`return 1`指的是传统意义上的`boolean``false`。

### 多线回声

有时您需要打印多行消息。有几种方法可以解决这个问题。最简单的方法是像这样多次使用`echo`:

```
echo "line1"echo "line2"echo "line3"
```

这是可行的，但可能不是解决这个问题的最佳方式。而是可以用`cat <&l`t；改为 EOF。大概是这样的:

```
cat << EOFline1line2line3EOF
```

注意`EOF`前不能有任何内容(包括空格或制表符)。如果您想在`if`语句中实现，应该如下所示。

```
if [ "a" == "a" ]; then  cat << EOFline1line2line3EOFfi
```

要知道，即使是消息本身也是靠左对齐的。这是因为如果将它们保留为选项卡式，命令行中显示的输出消息也将是选项卡式的。此外，如果`EOF`是选项卡式的，shell 会抱怨它，通常会在那里结束脚本。

### 标志/选项

您可能已经见过一些带有添加标志(有时是特定标志的参数)功能的脚本或命令。类似`git commit -a -m "Some commit message"`的东西。

这里有一个简单的例子(我已经尽可能全面地展示了这个例子。)

在脚本`run_this.sh`中:

```
while getopts ac: opt; do    case $opt in        a)            echo "\"a\" was executed."            ;;        c)            echo "\"c\" was executed with parameter \"$OPTARG\"."            ;;        \?)            echo "Invalid option: -$opt"            exit 1            ;;        :)            echo "option -$opt requires an argument."            exit 1            ;;    esacdone
```

运行命令:

```
./run_this.sh
```

```
./run_this.sh -a"a" was executed.
```

```
./run_this.sh -coption -c requires an argument.
```

```
./run_this.sh -c abcd"c" was executed with parameter "abcd".
```

```
./run_this.sh -a -c abc"a" was executed."c" was executed with parameter "abc".
```

```
./run_this.sh -xInvalid option: -x
```

在上面的例子中，选项`-a`和`-c`的不同之处在于在`getopts`行中，`c`后面有一个冒号(`:`，因此告诉程序该选项需要一个参数。另一件要记住的事情是，选项需要按字母顺序声明。如果你声明类似于`acb`的东西，那么`b`声明将被忽略，并且在切换条件下使用`-b`标志将导致错误消息而不是`b`情况。

感谢阅读！

### 关于我

我目前在脸书工作，是一名软件工程师。我花一些空闲时间用我觉得有趣的技术来试验和构建新的东西。在此或在 [GitHub](https://github.com/binhonglee) 上跟随我的探索之旅[。](https://binhong.me/blog)

### 参考

*   [小 getopts 教程](http://wiki.bash-hackers.org/howto/getopts_tutorial)
*   [如何在 Bash 中输出多行字符串](https://stackoverflow.com/questions/10969953/how-to-output-a-multiline-string-in-bash#10970616)