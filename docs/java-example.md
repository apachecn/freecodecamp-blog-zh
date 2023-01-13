# 最好的 Java 例子

> 原文：<https://www.freecodecamp.org/news/java-example/>

## Java 是什么？

[Java](https://www.oracle.com/java/index.html) 是由[太阳微系统](https://en.wikipedia.org/wiki/Sun_Microsystems)在 1995 年开发的一种编程语言，后来被[甲骨文](http://www.oracle.com/index.html)收购。它现在是一个完整的平台，有许多标准 API、开源 API、工具和一个巨大的开发人员社区。

它被大公司和小公司用来构建最值得信赖的企业解决方案。 [Android](https://www.android.com/) 应用开发完全由 Java 及其生态系统完成。

要了解更多 Java 基础知识，请阅读[本](https://java.com/en/download/faq/whatis_java.xml)和[本](http://tutorials.jenkov.com/java/what-is-java.html)。

## **版本**

最新版本是 2018 年发布的 [Java 11](http://www.oracle.com/technetwork/java/javase/overview) ，在之前版本 Java 10 的基础上做了[各种改进](https://www.oracle.com/technetwork/java/javase/11-relnote-issues-5012449.html)。但是出于所有的意图和目的，我们将在这个 wiki 的所有教程中使用 Java 8。

Java 也分为几个“版本”:

*   标准版-用于桌面和独立服务器应用
*   企业版——用于开发和执行嵌入在 Java 服务器中运行的 Java 组件
*   ME(微型版)，用于在手机和嵌入式设备上开发和执行 Java 应用程序

## 安装:JDK 还是 JRE？

从[官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载最新的 Java 二进制文件。这里你可能会面临一个问题，到底下载 JDK 还是 JRE？JRE 代表 Java 运行时环境，它是运行 Java 代码的平台相关的 Java 虚拟机，JDK 代表 Java 开发工具包，它包括大多数开发工具，最重要的是编译器`javac`，还有 JRE。因此，对于普通用户来说，JRE 就足够了，但是因为我们要用 Java 开发，所以我们要下载 JDK。

## **平台特定安装说明**

### **窗户**

*   下载相关的[。msi](https://en.wikipedia.org/wiki/Windows_Installer) 文件(32 位 x86 / i586，64 位 x64)
*   运行。msi 文件。这是一个自解压的可执行文件，它将在你的系统中安装 Java！

### **Linux**

*   为您的系统下载相关的[tar.gz](http://www.cyberciti.biz/faq/linux-unix-bsd-extract-targz-file/)文件并安装:

`bash $ tar zxvf jdk-8uversion-linux-x64.tar.gz`

*   [基于 RPM 的 Linux 平台](https://en.wikipedia.org/wiki/List_of_Linux_distributions#RPM-based)下载相关的[。rpm](https://en.wikipedia.org/wiki/RPM_Package_Manager) 文件和安装:

`bash $ rpm -ivh jdk-8uversion-linux-x64.rpm`

*   用户可以选择安装开源版本的 Java、OpenJDK 或 Oracle JDK。虽然 OpenJDK 正在积极开发中，并与甲骨文 JDK 公司保持同步，但它们只是在许可方面有所不同。然而，很少有开发商抱怨开放的 JDK 的稳定性。

### Ubuntu 的说明:

打开 JDK 安装:
`bash sudo apt-get install openjdk-8-jdk`

Oracle JDK 安装:
`bash sudo add-apt-repository ppa:webupd8team/java sudo apt-get update sudo apt-get install oracle-java8-installer`

### **Mac**

*   要么下载麦克·OSX。来自 Oracle 下载的 dmg 可执行文件
*   或者用[自制](http://brew.sh/)到[安装](http://stackoverflow.com/a/28635465/2861269):

```
brew tap caskroom/cask  
brew install brew-cask  
brew cask install java
```

### **验证安装**

通过打开命令提示符(Windows) / Windows Powershell /终端(Mac OS 和*Unix)并检查 Java 运行时和编译器的版本，验证 Java 是否已正确安装在您的系统中:

```
$ java -version
java version "1.8.0_66"
Java(TM) SE Runtime Environment (build 1.8.0_66-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.66-b17, mixed mode)

$ javac -version
javac 1.8.0_66
```

****提示**** :如果在`java`或`javac`或两者上出现诸如“未找到命令”之类的错误，不要惊慌，这只是您的系统路径设置不正确。对于 Windows，参见 [this StackOverflow answer](http://stackoverflow.com/questions/15796855/java-is-not-recognized-as-an-internal-or-external-command) 或[这篇文章](http://javaandme.com/)如何做。也有针对 Ubuntu 和 Mac 的指南。如果你还是不明白，不要担心，就在我们的[聊天室](https://gitter.im/FreeCodeCamp/java)问我们！

## JVM

好了，现在我们已经完成了安装，让我们首先开始理解 Java 生态系统的本质。Java 是一种[解释和编译的](http://stackoverflow.com/questions/1326071/is-java-a-compiled-or-an-interpreted-programming-language)语言，也就是说，我们编写的代码被编译成字节码并被解释运行。我们把代码写进去。java 文件，Java 将它们编译成在 Java 虚拟机或 JVM 上运行的[字节码](https://en.wikipedia.org/wiki/Java_bytecode)。这些字节码通常有一个. class 扩展名。

Java 是一种非常安全的语言，因为它不允许你的程序直接在机器上运行。相反，您的程序运行在一个名为 JVM 的虚拟机上。这个虚拟机为你可以进行的低级机器交互提供了几个 API，但是除此之外，你不能显式地使用机器指令。这大大增加了安全性。

此外，一旦您的字节码被编译，它可以在任何 Java 虚拟机上运行。该虚拟机依赖于机器，即它在 Windows、Linux 和 Mac 上有不同的实现。但是多亏了这个虚拟机，你的程序可以在任何系统上运行。这种哲学被称为[“一次编写，随处运行”](https://en.wikipedia.org/wiki/Write_once,_run_anywhere)。

## 你好，世界！

让我们编写一个简单的 Hello World 应用程序。打开任意编辑器/ IDE 并创建一个文件`HelloWorld.java`。

```
public class HelloWorld {

    public static void main(String[] args) {
        // Prints "Hello, World" to the terminal window.
        System.out.println("Hello, World");
    }

}
```

****N.B.**** 记住在 Java 中文件名应该与 ****公有类的名称**** 完全相同以便编译！

现在打开终端/命令提示符。在终端/命令提示符下，将当前目录更改为文件所在的目录。并编译该文件:

```
$ javac HelloWorld.java
```

现在使用`java`命令运行文件！

```
$ java HelloWorld
Hello, World
```

恭喜你。您的第一个 Java 程序已经成功运行。这里我们只是打印一个字符串，并将其传递给 API `System.out.println`。我们将涵盖代码中的所有概念，但是欢迎您仔细研究一下！如果您有任何疑问或需要额外的帮助，请随时在我们的 [Gitter 聊天室](https://gitter.im/FreeCodeCamp/java)联系我们！

## **文档**

Java 有大量的文档记录，因为它支持大量的 API。如果您正在使用任何主流的 IDE，比如 Eclipse 或 IntelliJ IDEA，您会发现其中包含了 Java 文档。

另外，这里有一个 Java 编码的免费 ide 列表:

*   [NetBeans](https://netbeans.org/)
*   [月食](https://eclipse.org/)
*   [IntelliJ IDEA](https://www.jetbrains.com/idea/features/)
*   [安卓工作室](https://developer.android.com/studio/index.html)
*   [蓝色](https://www.bluej.org/)
*   [绝地武士](http://www.jedit.org/)
*   [Oracle JDeveloper](http://www.oracle.com/technetwork/developer-tools/jdev/overview/index-094652.html)

## 基本操作

Java 支持对变量的以下操作:

*   ****算术**** : `Addition (+)`，`Subtraction (-)`，`Multiplication (*)`，`Division (/)`，`Modulus (%)`，`Increment (++)`，`Decrement (--)`。
*   ****字符串串联**** : `+`可用于字符串串联，但字符串上的减法`-`不是有效操作。
*   ****关系**** : `Equal to (==)`、`Not Equal to (!=)`、`Greater than (>)`、`Less than (<)`、`Greater than or equal to (>=)`、`Less than or equal to (<=)`
*   ****按位**** : `Bitwise And (&)`、`Bitwise Or (|)`、`Bitwise XOR (^)`、`Bitwise Compliment (~)`、`Left shift (<<)`、`Right Shift (>>)`、`Zero fill right shift (>>>)`
*   ****逻辑**** : `Logical And (&&)`，`Logical Or (||)`，`Logical Not (!)`
*   **:`=``+=``-=``*=``/=``%=``<<=``>>=``&=``^=``|=`**
*   ******其他**** : `Conditional/Ternary(?:)`，`instanceof`**

**虽然大多数运算都是不言自明的，但条件(三元)运算符的工作方式如下:**

**`expression that results in boolean output ? return this value if true : return this value if false;`**

**示例:真实条件:**

```
 `int x = 10;
    int y = (x == 10) ? 5 : 9; // y will equal 5 since the expression x == 10 evaluates to true` 
```

**错误条件:**

```
 `int x = 25;
    int y = (x == 10) ? 5 : 9; // y will equal 9 since the expression x == 10 evaluates to false`
```

**运算符的实例用于类型检查。它可以用来测试一个对象是否是一个类、子类或接口的实例。通用格式-*类/子类/接口的对象 ****实例******

**下面是一个说明运算符实例的程序:**

```
 `Person obj1 = new Person();
        Person obj2 = new Boy();

        // As obj is of type person, it is not an
        // instance of Boy or interface
        System.out.println("obj1 instanceof Person: " +  (obj1 instanceof Person)); /*it returns true since obj1 is an instance of person */` 
```

# **可变示例**

**变量存储值。它们是用来存储文本、数字等数据的最基本的实体。在一个程序中。**

**在 Java 中，变量是强类型的，这意味着无论何时声明变量，都必须定义它的类型。否则，编译器将在编译时抛出错误。因此，每个变量都有一个关联的“数据类型”,属于以下类型之一:**

**原始类型:int、short、char、long、boolean、byte、float、double**

**包装类型:整型、短整型、字符型、长型、布尔型、字节型、浮点型、双精度型**

**引用类型:String、StringBuilder、Calendar、ArrayList 等。**

**您可能已经注意到，包装类型由与原始类型拼写完全相同的类型组成，除了开头的大写字母(像引用类型)。这是因为包装器类型实际上是更一般的引用类型的一部分，但是通过自动装箱和取消装箱与它们的原始对应物紧密地联系在一起。现在，你只需要知道这样一个“包装器类型”存在。**

**通常，您可以按照以下语法声明(即创建)变量:<data-type><variablename>；</variablename></data-type>**

```
`// Primitive Data Type
int i;

// Reference Data Type
Float myFloat;`
```

**你可以在声明变量的同时给变量赋值(这叫做初始化)，也可以在声明后在代码中的任何地方赋值。符号=用于相同的目的。**

```
`// Initialise the variable of Primitive Data Type 'int' to store the value 10
int i = 10;
double amount = 10.0;
boolean isOpen = false;
char c = 'a'; // Note the single quotes

//Variables can also be declared in one statement, and assigned values later.
int j;
j = 10;

// initiates an Float object with value 1.0
// variable myFloat now points to the object
Float myFloat = new Float(1.0);

//Bytes are one of types in Java and can be
//represented with this code
int byteValue = 0B101;
byte anotherByte = (byte)0b00100001;`
```

**从上面的例子可以看出，原始类型的变量与引用(& Wrapper)类型的变量行为略有不同——原始变量存储实际值，而引用变量引用包含实际值的“对象”。你可以在下面的链接中找到更多信息。**

# ****数组****

**数组是保存在连续内存地址中的相似数据类型(允许原始和引用两种数据类型)的值(或对象)的集合。数组用于存储相似数据类型的集合。数组总是从索引 0 开始，并被实例化为一组索引。数组中的所有变量必须是相同的类型，在实例化时声明。**

******语法:******

```
`dataType[] arrayName;   // preferred way`
```

**这里，`java datatype[]`描述了在它之后声明的所有变量将被实例化为指定数据类型的数组。因此，如果我们想要实例化更多相似数据类型的数组，我们只需将它们添加到指定的`java arrayName`之后(不要忘记仅用逗号分隔它们)。下一节给出一个例子供参考。**

```
`dataType arrayName[];  //  works but not preferred way`
```

**这里，`java datatype`只描述了它后面的变量属于那个数据类型。此外，变量名后的`java []`描述了变量是指定数据类型的数组(不仅仅是该数据类型的值或对象)。因此，如果我们想要实例化更多相似数据类型的数组，我们将在已经指定的变量之后添加变量名称，用逗号分隔，每次我们都必须在变量名称之后添加`java []`，否则变量将被实例化为普通的值存储变量(不是数组)。为了更好地理解，下一节给出了一个例子。**

## ****上述语法的代码片段:****

```
`double[] list1, list2; // preferred way`
```

**上面的代码片段实例化了两个双类型名称 list1 和 list2 的数组。**

```
`double list1[], list2; // works but not preferred way`
```

**上面的代码片段包含一个数据类型为 list1 的数组和一个数据类型为 list2 的简单变量(不要与名称 ****list2**** 混淆)。变量名称与变量类型无关)。**

**注意:样式`double list[]`不是首选，因为它来自 C/C++语言，在 Java 中被采用以适应 C/C++程序员。此外，它的可读性更好:你可以看到它是一个“双数组命名列表”，而不是“一个双调用列表是一个数组”**

## ****创建数组:****

```
`dataType[] arrayName = new dataType[arraySize];`
```

## ****上述语法的代码片段:****

```
`double[] List = new double[10];`
```

## ****创建数组的另一种方法:****

```
`dataType[] arrayName = {value_0, value_1, ..., value_k};`
```

## ****上述语法的代码片段:****

```
`double[] list = {1, 2, 3, 4};

The code above is equivalent to:
double[] list = new double[4];
*IMPORTANT NOTE: Please note the difference between the types of brackets
that are used to represent arrays in two different ways.`
```

## ****访问数组:****

```
`arrayName[index]; // gives you the value at the specified index`
```

## ****上述语法的代码片段:****

```
`System.out.println(list[1]);`
```

**输出:**

```
`2.0`
```

## ****修改数组:****

```
`arrayName[index] = value;` 
```

**注意:初始化数组后，不能改变它的大小或类型。注意:你可以像这样重置数组**

```
`arrayName = new dataType[] {value1, value2, value3};`
```

## ****数组大小:****

**使用“长度属性”可以找到数组中元素的数量。这里需要注意的是，`java length`是每个数组的 ****属性**** ，即存储变量长度的变量名。不要和数组的 ****方法**** 混淆，因为它的名字和字符串类对应的`java length()`方法相同。**

```
`int[] a = {4, 5, 6, 7, 8}; // declare array
System.out.println(a.length); //prints 5`
```

## ****上述语法的代码片段:****

```
`list[1] = 3; // now, if you access the array like above, it will output 3 rather than 2`
```

***代码示例:***

```
`int[] a = {4, 5, 6, 7, 8}; // declare array
for (int i = 0; i < a.length; i++){ // loop goes through each index
    System.out.println(a[i]); // prints the array
}`
```

**输出:**

```
 `4
    5
    6
    7
    8`
```

### ****多维数组****

**二维数组(2D 数组)可以被认为是一个由行和列组成的表格。尽管这种表示只是为了更好地解决问题而可视化阵列的一种方式。这些值实际上只存储在连续的内存地址中。**

```
`int M = 5;
int N = 5;
double[][] a = new double [M][N]; //M = rows N = columns
for(int i = 0; i < M; i++) {
    for (int j = 0; j < N; j++) {
        //Do something here at index 
    }
}`
```

**该循环将执行 M ^ N 次，并将构建:**

**[0 | 1 | 2 | 3 | 4](https://guide.freecodecamp.org/java/arrays)
0 | 1 | 2 | 3 | 4T5【0 | 1 | 2 | 3 | 4】**

**类似地，也可以制作 3D 阵列。它可以被想象成一个长方体而不是长方形(如上)，被分成更小的立方体，每个立方体存储一些值。可以按照以下方式进行初始化:**

```
`int a=2, b=3, c=4;
int[][][] a=new int[a][b][c];`
```

**以类似的方式，一个人可以按照他/她所希望的那样创建任意多个维度的数组，但是可视化多于 3 个维度的数组很难以特定的方式来可视化。**

### ****交错排列****

**交错数组是多维数组，它有固定数量的行，但有不同数量的列。交错数组用于节省数组的内存使用。下面是一个代码示例:**

```
`int[][] array = new int[5][]; //initialize a 2D array with 5 rows
array[0] = new int[1]; //creates 1 column for first row
array[1] = new int[2]; //creates 2 columns for second row
array[2] = new int[5]; //creates 5 columns for third row
array[3] = new int[5]; //creates 5 columns for fourth row
array[4] = new int[5]; //creates 5 columns for fifth row`
```

**输出:**

**[0](https://guide.freecodecamp.org/java/arrays)
0 | 1 | 2 | 3 | 4T5【0 | 1 | 2 | 3 | 4】**

# ****控制流程****

**控制流语句正是这个术语的含义。它们是基于决策、循环和分支来改变执行流的语句，以便程序可以有条件地执行代码块。**

**主要地，Java 具有以下用于流控制的结构:**

*   **`if`**
*   **`if...else`**

```
`if( <expression that results in a boolean> ){
    //code enters this block if the above expression is 'true'
}`
```

```
`if( <expression that results in a boolean> ){
    //execute this block if the expression is 'true'
} else{
    //execute this block if the expression is 'false'
}`
```

**`switch`**

**当有多个值和案例需要检查时，Switch 是`if...else`构造的一个替代。**

```
`switch( <integer / String / Enum > ){
    case <int/String/Enum>: 
        <statements>
        break;
    case <int/String/Enum>:
        <statements>
        break;
    default:
        <statements>
}`
```

**注意:如果`break`语句丢失，程序流程`falls through`下一个`case`。比方说，你对办公室里的每个人都说标准的“你好”，但你对坐在你旁边、听起来对你老板脾气暴躁的女孩格外友好。表示的方式应该是这样的:**

```
`switch(person){
    case 'boss': 
        soundGrumpy();
        break;
    case 'neighbour': 
        soundExtraNice();
        break;
    case 'colleague':
        soundNormal();
        break;
    default:
        soundNormal();
}`
```

```
`Note: The `default` case runs when none of the `case` matches. Remember that when a case has no `break` statement, it `falls through` to the next case and will continue to the subsequent `cases` till a `break` is encountered. Because of this, make sure that each case has a `break` statement. The `default` case does not require a `break` statement.` 
```

*   **`nested statements`**

**前面的任何控制流都可以嵌套。这意味着你可以嵌套`if`、`if..else`和`switch..case`语句。也就是说，您可以在其他语句中任意组合这些语句，并且对`nesting`的深度没有限制。**

**例如，让我们考虑以下场景:**

*   **如果你的钱少于 25 美元，你可以给自己买杯咖啡。**
*   **如果你有超过 25 美元，但少于 60 美元，你得到自己一顿像样的饭。**
*   **如果你有 60 多美元，但不到 100 美元，你就可以吃一顿像样的饭，外加一杯酒。**
*   **然而，当你有超过 100 美元时，取决于你和谁在一起，你要么去吃烛光晚餐(和你的妻子)，要么去体育酒吧(和你的朋友)。**

**表示这一点的一种方式是:**

```
`int cash = 150;
String company = "friends";

if( cash < 25 ){
    getCoffee();
} else if( cash < 60 ){
    getDecentMeal();
} else if( cash < 100 ){
    getDecentMeal();
    getGlassOfWine();
} else {
    switch(company){
        case "wife":
            candleLitDinner();
            break;
        case "friends": 
            meetFriendsAtSportsBar();
            break;
        default:
            getDecentMeal();
    }
}`
```

**在本例中，`meetFriendsAtSportsBar()`将被执行。**

# ****循环****

**每当您需要多次执行一个代码块时，循环通常会派上用场。**

**Java 有 4 种类型的循环:**

# ****While 循环****

**`while`循环重复执行语句块，直到括号中指定的条件评估为`false`。例如:**

```
`while (some_condition_is_true)
{
    // do something
}`
```

 **(执行语句块的)每一次“迭代”之前都要对括号内指定的条件进行评估——只有当条件评估为`true`时，才会执行语句。如果计算结果为`false`，程序将从`while`块后面的语句继续执行。**

******注意**** :对于开始执行的`while`循环，最初需要条件为`true`。然而，要退出循环，您必须在语句块中做一些事情，以便在条件评估为`false`时最终到达迭代(如下所示)。否则循环将永远执行。(实际上，它会一直运行，直到 [JVM](https://guide.freecodecamp.org/java/the-java-virtual-machine-jvm) 耗尽内存。)**

## ****例子****

**在下面的例子中，`expression`由`iter_While < 10`给出。每当循环执行一次，我们就将`iter_While`增加`1`。当`iter_While`值达到`10`时`while`循环中断。**

```
`int iter_While = 0;
while (iter_While < 10)
{
    System.out.print(iter_While + " ");
    // Increment the counter
    // Iterated 10 times, iter_While 0,1,2...9
    iter_While++;
}
System.out.println("iter_While Value: " + iter_While);`
```

**输出:**

```
`1 2 3 4 5 6 7 8 9
iter_While Value: 10`
```