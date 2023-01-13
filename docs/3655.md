# C 语言中的文件处理——如何打开、关闭和写入文件

> 原文：<https://www.freecodecamp.org/news/file-handling-in-c-how-to-open-close-and-write-to-files/>

如果你以前写过 C `helloworld`程序，你就已经知道 C:

```
/* A simple hello world in C. */
#include <stdlib.h>

// Import IO functions.
#include <stdio.h>

int main() {
    // This printf is where all the file IO magic happens!
    // How exciting!
    printf("Hello, world!\n");
    return EXIT_SUCCESS;
}
```

文件处理是编程最重要的部分之一。在 C 中，我们使用文件类型的结构指针来声明文件:

```
FILE *fp;
```

c 提供了许多内置函数来执行基本的文件操作:

*   `fopen()` -创建新文件或打开现有文件
*   `fclose()` -关闭文件
*   `getc()` -从文件中读取字符
*   `putc()` -将字符写入文件
*   `fscanf()` -从文件中读取一组数据
*   `fprintf()` -将一组数据写入文件
*   `getw()` -从文件中读取一个整数
*   `putw()` -将整数写入文件
*   `fseek()` -将位置设置到所需点
*   `ftell()` -给出文件中的当前位置
*   `rewind()` -将位置设置到起点

### **打开文件**

`fopen()`功能用于创建文件或打开现有文件:

```
fp = fopen(const char filename,const char mode);
```

打开文件有多种模式:

*   `r` -以读取模式打开文件
*   `w` -以写模式打开或创建一个文本文件
*   `a` -以追加模式打开文件
*   `r+` -以读写模式打开文件
*   `a+` -以读写模式打开文件
*   `w+` -以读写模式打开文件

下面是一个从文件中读取数据并向其中写入数据的示例:

```
#include<stdio.h>
#include<conio.h>
main()
{
FILE *fp;
char ch;
fp = fopen("hello.txt", "w");
printf("Enter data");
while( (ch = getchar()) != EOF) {
  putc(ch,fp);
}
fclose(fp);
fp = fopen("hello.txt", "r");

while( (ch = getc(fp)! = EOF)
  printf("%c",ch);

fclose(fp);
}
```

现在你可能会想，“这只是将文本打印到屏幕上。这个文件 IO 怎么样？”

答案一开始并不明显，需要对 UNIX 系统有所了解。在 UNIX 系统中，一切都被视为一个文件，这意味着您可以对其进行读写。

这意味着你的打印机可以被抽象成一个文件，因为你用打印机所做的一切就是用它来写。将这些文件看作流也是有用的，因为您将在后面看到，您可以用 shell 重定向它们。

那么这与`helloworld`和文件 IO 有什么关系呢？

当你调用`printf`时，你实际上只是在写一个叫做`stdout`的特殊文件，是 ****标准输出**** 的简称。`stdout`表示由您的 shell(通常是终端)决定的标准输出。这解释了为什么它会打印到你的屏幕上。

还有另外两个流(即文件)是你努力就能得到的，`stdin`和`stderr`。`stdin`代表 ****标准输入**** ，您的 shell 通常会将它附加到键盘上。`stderr`代表 ****标准错误**** 输出，您的 shell 通常会将它附加到终端上。

### **初级文件 IO，或者说我是如何学会铺设管道的**

理论够了，写点代码言归正传吧！写入文件最简单的方法是使用输出重定向工具`>`重定向输出流。

如果要追加，可以使用`>>`:

```
# This will output to the screen...
./helloworld
# ...but this will write to a file!
./helloworld > hello.txt
```

不出所料，`hello.txt`的内容将是

```
Hello, world!
```

假设我们有另一个叫做`greet`的程序，类似于`helloworld`，它用一个给定的`name`问候你:

```
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Initialize an array to hold the name.
    char name[20];
    // Read a string and save it to name.
    scanf("%s", name);
    // Print the greeting.
    printf("Hello, %s!", name);
    return EXIT_SUCCESS;
}
```

我们可以使用`<`工具重定向`stdin`来读取文件，而不是从键盘读取:

```
# Write a file containing a name.
echo Kamala > name.txt
# This will read the name from the file and print out the greeting to the screen.
./greet < name.txt
# ==> Hello, Kamala!
# If you wanted to also write the greeting to a file, you could do so using ">".
```

注意:这些重定向操作符在`bash`和类似的 shells 中。

### **真正的交易**

上述方法只适用于最基本的情况。如果你想做更大更好的事情，你可能会想从 C 内部而不是通过外壳来处理文件。

为此，您将使用一个名为`fopen`的函数。这个函数有两个字符串参数，第一个是文件名，第二个是模式。

这些模式基本上都是权限，因此`r`表示读，`w`表示写，`a`表示追加。您也可以组合它们，所以`rw`意味着您可以读写文件。还有更多模式，但这些是最常用的。

有了一个`FILE`指针后，你可以使用基本上相同的 IO 命令，除了你必须给它们加上前缀`f`，第一个参数将是文件指针。比如`printf`的文件版本是`fprintf`。

这里有一个名为`greetings`的程序，它从一个包含姓名列表的文件中读取一个，并将问候语写入另一个文件:

```
#include <stdio.h>
#include <stdlib.h>

int main() {
    // Create file pointers.
    FILE *names = fopen("names.txt", "r");
    FILE *greet = fopen("greet.txt", "w");

    // Check that everything is OK.
    if (!names || !greet) {
        fprintf(stderr, "File opening failed!\n");
        return EXIT_FAILURE;
    }

    // Greetings time!
    char name[20];
    // Basically keep on reading untill there's nothing left.
    while (fscanf(names, "%s\n", name) > 0) {
        fprintf(greet, "Hello, %s!\n", name);
    }

    // When reached the end, print a message to the terminal to inform the user.
    if (feof(names)) {
        printf("Greetings are done!\n");
    }

    return EXIT_SUCCESS;
}
```

假设`names.txt`包含以下内容:

```
Kamala
Logan
Carol
```

然后在运行`greetings`之后，文件`greet.txt`将包含:

```
Hello, Kamala!
Hello, Logan!
Hello, Carol!
```