# 外壳脚本

> 原文：<https://www.freecodecamp.org/news/shell-scripting/>

在命令行中，shell 脚本是一个可执行文件，其中包含 shell 将执行的一组指令。它的主要目的是将一组指令(或命令)减少到一个文件中。它也可以处理一些逻辑，因为它是一种编程语言。

## **如何创建它**

创建文件:

```
$ touch myscript.sh
```

在文件的开头添加一个 [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) 。Shebang 行负责让命令解释器知道 shell 脚本将与哪个解释器一起运行:

```
$ echo "#!/bin/bash" > myscript.sh
# or
$ your-desired-editor myscript.sh
# write at the first line #!/bin/bash
```

添加一些命令:

```
$ echo "echo Hello World!" >> myscript.sh
```

给文件*执行*模式:

```
$ chmod +x myscript.sh
```

执行它！

```
$ ./myscript.sh
Hello World!
```

更多关于 shell 脚本的信息可以在[这里](https://www.shellscript.sh/)找到

## 关于 shell 脚本和 Linux 的更多信息:

*   [在 Linux shell 中生存的初学者指南](https://www.freecodecamp.org/news/a-beginners-guide-to-surviving-in-the-linux-shell-cda0f5a0698c/)
*   [需要了解的基本 Linux 命令](https://guide.freecodecamp.org/linux/basic-linux-commands)
*   [Shell 脚本技巧](https://www.freecodecamp.org/news/functional-and-flexible-shell-scripting-tricks-a2d693be2dd4/)