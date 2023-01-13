# 如何在 C#中将一个字符串转换成一个整数——带示例代码的教程

> 原文：<https://www.freecodecamp.org/news/how-to-convert-a-string-to-an-int-in-c-tutorial-with-example-code/>

当您处理用户输入、JSON 数据、API 响应或正则表达式时，将字符串转换为整数是常见的做法。但是对于每种情况，正确的方法是什么呢？

在本文中，我将解释用 C#将字符串转换成数字的三种方法，并向您展示如何为您的场景选择正确的方法。

## 确定数据的来源

首先来看看你的数据是从哪里来的。将字符串“123”转换成整数很容易，但在现实世界中，事情从来没有那么简单。

“数字字符串”可以来自数据库、文本文件、API 或应用程序的用户。你有多大把握这真的是一个数字？

| 数据源 | 信心 | 会发生什么 |
| --- | --- | --- |
| 用户输入 | 🙁 | 
【1.23】你好 |
| JSON 数据 | 😐 | 123.1"
" |
| API 响应 | 😐 | 11，7"
" |
| 正则表达式匹配 | 🙂 | 无效表达式不仅允许数字 |

## 你的数字能有多大？

你还需要知道你的目标数字可以有多大。在本文的范围内，我们讨论的是 Int。这通常被认为是`Int32` ( `int`)，但是你也可以使用`Int16` ( `short`)和`Int64` ( `long`)，这取决于你期望的数字有多大。

| 类型 | 最大数量 |
| --- | --- |
| `Int16` ( `short`) | 32767 ( `Int16.MaxValue`) |
| `Int32` ( `int`) | 2147483647(`Int32.MaxValue`) |
| `Int64` ( `long`) | 9223372036854775807(`Int64.MaxValue` |

## 里面的解析(字符串)-输入可信度:高🙂

当您确定输入确实是一个数字时，使用`int.Parse`。它还可以解析特定于文化或其他众所周知的格式的数字，但您需要知道确切的格式:

| 签名 | 输出 |
| --- | --- |
| `int.Parse("123")` | One hundred and twenty-three |
| `int.Parse("")` | 投掷`FormatException` |
| `int.Parse(null)` | 投掷`ArgumentNullException` |
| `int.Parse("123,000")` | 投掷`FormatException` |
| `int.Parse("123,000",`
`System.Globalization.NumberStyles.AllowThousands,`
 | One hundred and twenty-three thousand |

## 转换。ToInt32(字符串)–输入可信度:中😑

`Convert`与`int.Parse`非常相似，只有一点例外:`null`被转换为 0，并且不抛出异常。它还可以处理其他输入数据类型(不仅仅是字符串):

| 签名 | 输出 |
| --- | --- |
| `Convert.ToInt32("123")` | One hundred and twenty-three |
| `Convert.ToInt32("")` | 投掷`FormatException` |
| `Convert.ToInt32(null)` | Zero |
| `Convert.ToInt32("123,000")` | 投掷`FormatException` |
| `Convert.ToInt32("1.23")` | 投掷`FormatException` |
| `Convert.ToInt32(1.23)` | one |

*注意:可以用* `*Convert.ToInt32*` *去掉小数点后的数字精度。但是，为了确保良好的代码可读性，您应该使用* `*Math.Floor*` *来完成这项任务。*

## Int*。TryParse(String，Int32) -输入置信度:低🙁

每当您不信任您的数据源时，请使用`TryParse`。例如，当您从提交的表单中获取用户输入或解析和验证数据时:

| 签名 | 输出 |
| --- | --- |
| `int number;`
 | number = 123
可兑换=真 |
| `int number;`
 | number = 0
可兑换=假 |
| `int number;`
 | number = 0
可兑换=假 |

*注意:您也可以通过键入`out int number`将数字定义移动到`TryParse`方法调用中。*

最典型的例子就是用`Console.ReadLine`:

```
while (!Int32.TryParse(Console.ReadLine(), out int number))
{
	Console.WriteLine("Please input number");
}
Console.WriteLine(number); 
```

## 结论

在本文中，我向您展示了用 C#将数字转换为字符串的三种方法，并解释了如何根据数据源和您对数据源的信心来决定使用哪种方法。

如果你不想错过我的新文章，请在 [Twitter](https://twitter.com/ondrabus) 上关注我。