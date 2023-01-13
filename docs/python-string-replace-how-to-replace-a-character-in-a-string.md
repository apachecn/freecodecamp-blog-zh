# python String . Replace()–如何替换字符串中的字符

> 原文：<https://www.freecodecamp.org/news/python-string-replace-how-to-replace-a-character-in-a-string/>

Python 有许多字符串修改方法，其中之一是`replace`方法。

在本文中，我将向您展示如何使用这种方法来替换字符串中的字符。

## Python 中的`replace`方法如何工作

`replace` string 方法返回一个新的字符串，其中原始字符串中的一些字符被替换为新的字符。原始字符串不受影响或修改。

`replace`方法的语法是:

```
string.replace(old_char, new_char, count) 
```

`old_char`参数是要替换的字符集。

`new_char`参数是替换`old_char`的字符集。

`count`参数是可选的，它指定将替换多少次。如果未指定，所有出现的`old_char`将被替换为`new_char`。

让我们看一些例子。

## `string.replace()`例子

下面是一个将字符串中的“JavaScript”替换为“PHP”的示例:

```
str = "I love JavaScript. I prefer JavaScript to Python because JavaScript looks more beautiful"

new_str = str.replace("JavaScript", "PHP")

print(new_str)
# I love PHP. I prefer PHP to Python because PHP looks more beautiful 
```

您可以看到`replace`方法是如何用“PHP”替换“JavaScript”的。

在本例中，三次出现的“JavaScript”被替换为“PHP”。如果您只想替换一个事件，该怎么办？那么你可以像这样使用`count`参数:

```
str = "I love JavaScript. I prefer JavaScript to Python because JavaScript looks more beautiful"

new_str = str.replace("JavaScript", "PHP", 1)

print(new_str)
# I love PHP. I prefer JavaScript to Python because JavaScript looks more beautiful 
```

通过应用一个 **1** 的`count`参数，可以看到只有第一个“JavaScript”(第一次出现)被替换为“PHP”。其余的“JavaScript”保持不变。

## 在 Python 中何时使用`replace`方法

这种方法的一个很好的用例是替换用户输入中的字符以符合某种标准。

比方说，您希望用户输入他们的用户名，但是您不希望用户名输入中有空白字符。您可以使用`replace`方法用连字符替换提交的字符串中的空格。下面是如何做到这一点:

```
user_input = "python my favorite"

updated_username = user_input.replace(" ", "-")

print(updated_username)
# python-my-favorite 
```

从`replace`方法的结果中，您可以看到空格已经被符合您的标准的连字符所替换。

## 包扎

`replace`方法用一个新的子串替换一个字符串中现有的子串。您可以指定现有子字符串有多少次将被该方法的`count`参数替换。