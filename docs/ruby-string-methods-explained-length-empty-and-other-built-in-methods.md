# Ruby String 方法解释——Length、Empty 和其他内置方法

> 原文：<https://www.freecodecamp.org/news/ruby-string-methods-explained-length-empty-and-other-built-in-methods/>

Ruby 有许多处理字符串的内置方法。默认情况下，Ruby 中的字符串是可变的，可以就地更改，也可以从方法中返回新的字符串。

## 长度

属性返回字符串中的字符数，包括空格。

```
"Hello".length #=> 5
"Hello World!".length #=> 12
```

## 空的

如果字符串长度为零，`.empty?`方法返回`true`。

```
"Hello".empty? #=> false
"!".empty?     #=> false
" ".empty?     #=> false
"".empty?      #=> true
```

## 数数

方法计算一个特定的字符在一个字符串中出现的次数。

此方法区分大小写。

```
"HELLO".count('L') #=> 2
"HELLO WORLD!".count('LO') #=> 1
```

## 插入

方法将一个字符串插入到另一个字符串中给定的索引之前。

```
"Hello".insert(3, "hi5") #=> Helhi5lo # "hi5" is inserted into the string right before the second 'l' which is at index 3
```

## 大写字母

`.upcase`方法将字符串中的所有字母转换成大写。

```
"Hello".upcase #=> HELLO
```

## 向下 case

`.downcase`方法将字符串中的所有字母转换成小写。

```
"Hello".downcase #=> hello
```

## 交换情况

方法将字符串中的大写字母转换成小写字母，并将小写字母转换成大写字母。

```
"hELLO wORLD".swapcase #=> Hello World
```

## 大写

`.capitalize`方法使字符串中的第一个字母大写，其余字母小写。

```
"HELLO".capitalize #=> Hello
"HELLO, HOW ARE YOU?".capitalize #=> Hello, how are you?
```

*注意，第一个字母只有在字符串的开头才大写。* `ruby "-HELLO".capitalize #=> -hello "1HELLO".capitalize #=> 1hello`

## 反面的

`.reverse`方法颠倒字符串中字符的顺序。

```
"Hello World!".reverse #=> "!dlroW olleH"
```

## 裂开

`.split`获取一个字符串，*将*拆分成一个数组，然后返回该数组。

```
"Hello, how are you?".split #=> ["Hello,", "how", "are", "you?"]
```

默认方法基于空白分割字符串，除非提供了不同的分隔符(见第二个例子)。

```
"H-e-l-l-o".split('-') #=> ["H", "e", "l", "l", "o"]
```

## 砍

`.chop`方法删除字符串的最后一个字符。

一个新的字符串被返回，除非你使用`.chop!`方法来改变原来的字符串。

```
"Name".chop #=> Nam
```

```
name = "Batman"
name.chop
name == "Batma" #=> false
```

```
name = "Batman"
name.chop!
name == "Batma" #=> true
```

## 剥夺

`.strip`方法删除字符串中的前导和尾随空格，包括制表符、换行符和回车符(`\t`、`\n`、`\r`)。

```
"  Hello  ".strip #=> Hello
```

## 切齿

`.chomp`方法移除字符串中的最后一个字符，仅当它是回车符或换行符(`\r`、`\n`)。

这种方法通常与`gets`命令一起使用，从用户输入中删除返回。

```
"hello\r".chomp #=> hello
"hello\t".chomp #=> hello\t # because tabs and other whitespace remain intact when using `chomp`
```

## 到整数

方法将一个字符串转换成一个整数。

```
"15".to_i #=> 15 # integer
```

## Gsub

`gsub`将字符串中第一个参数的所有引用替换为第二个参数。

```
"ruby is cool".gsub("cool", "very cool") #=> "ruby is very cool"
```

`gsub`也接受模式(如 *regexp* )作为第一个参数，允许:

```
"ruby is cool".gsub(/[aeiou]/, "*") #=> "r*by *s c**l"
```

## 串联

Ruby 实现了一些方法来连接两个字符串。

`+`方法:

```
"15" + "15" #=> "1515" # string
```

`<<`方法:

```
"15" << "15" #=> "1515" # string
```

`concat`方法:

```
"15".concat "15" #=> "1515" # string
```

## 索引

`index`方法返回子字符串或正则表达式模式匹配在字符串中第一次出现的索引位置。如果没有找到匹配，则返回`nil`。

第二个可选参数指示从字符串中的哪个索引位置开始搜索匹配项。

```
"information".index('o') #=> 3
"information".index('mat') #=> 5
"information".index(/[abc]/) #=> 6
"information".index('o', 5) #=> 9
"information".index('z') #=> nil
```

## 清楚的

移除字符串内容。

```
a = "abcde"
a.clear    #=> ""
```