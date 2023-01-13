# Python 中的转义序列

> 原文：<https://www.freecodecamp.org/news/escape-sequences-python/>

转义序列允许您在字符串中包含特殊字符。为此，只需在想要转义的字符前添加一个反斜杠(`\`)。

例如，假设您用单引号初始化了一个字符串:

```
s = 'Hey, whats up?'
print(s)
```

**输出:**

```
Hey, whats up?
```

但是，如果您包含一个撇号而不转义它，那么您将得到一个错误:

```
s = 'Hey, what's up?'
print(s)
```

**输出:**

```
 File "main.py", line 1
    s = 'Hey, what's up?'
                   ^
SyntaxError: invalid syntax
```

要解决这个问题，只需对撇号进行转义:

```
s = 'Hey, what\'s up?'
print(s)
```

要在字符串中添加换行符，请使用`\n`:

```
print("Multiline strings\ncan be created\nusing escape sequences.")
```

**输出:**

```
Multiline strings
can be created
using escape sequences.
```

要记住的一件重要的事情是，如果你想在一个字符串中包含一个反斜杠字符，你需要对它进行转义。例如，如果您想在 Windows 中打印一个目录路径，您需要对字符串中的每个反斜杠进行转义:

```
print("C:\\Users\\Pat\\Desktop")
```

**输出:**

```
C:\Users\Pat\Desktop
```

## Raw strings

通过在字符串前加上`r`或`R`可以使用一个*原始*字符串，这允许包含反斜杠而不需要对它们进行转义。例如:

```
print(r"Backslashes \ don't need to be escaped in raw strings.") 
```

**输出:**

```
Backslashes \ don't need to be escaped in raw strings.
```

但是请记住，原始字符串末尾的非转义反斜杠会导致错误:

```
print(r"There's an unescaped backslash at the end of this string\")
```

**输出:**

```
 File "main.py", line 1
    print(r"There's an unescaped backslash at the end of this string\")
                                                                      ^
SyntaxError: EOL while scanning string literal
```

# 常见转义序列

| 换码顺序 | 意义 |
| --- | --- |
| \ | 反斜杠(`\`) |
| ' | 单引号(`'`) |
| " | 双引号(`"`) |
| \n | ASCII 换行符(添加换行符) |
| \b | ASCII 退格 |

转义序列的完整列表可以在 Python 文档中的[这里](https://docs.python.org/3/reference/lexical_analysis.html#strings)找到。