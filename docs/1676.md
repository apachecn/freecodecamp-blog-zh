# PHP 数组长度教程——如何获得数组大小

> 原文：<https://www.freecodecamp.org/news/php-array-length-how-to-get-an-array-size/>

数组是 PHP 中一种强大的数据类型。知道如何快速确定数组的大小是一项有用的技能。

在本文中，我将向您简要介绍数组是如何工作的，然后我将深入研究如何获得 PHP 数组的大小。

如果你已经知道什么是数组，你可以直接跳到 **[如何得到数组的大小？](#how-to-get-an-array-size)** 节。

## PHP 中的数组是什么？

在我们开始获取数组大小之前，我们需要确保我们理解什么是数组。PHP 中的[数组是一种变量类型，允许您存储多条数据。](https://www.php.net/manual/en/language.types.array.php)

例如，如果要存储一个简单的字符串，可以使用 PHP 字符串类型:

```
$heading = 'PHP Array Length Tutorial';
```

但是，如果您想存储更多的独立数据，可以考虑使用两个字符串变量。

```
$heading = 'PHP Array Length Tutorial';
$subheading = 'How to get an array size';
$author = 'Jonathan Bossenger'
```

这很好，但是如果您需要存储更多的数据，并在代码的其他地方快速调用这些数据，该怎么办呢？这就是数组派上用场的地方。您仍然可以存储单独的数据片段，但是使用单个变量。

```
$post_data = array(
    'PHP Array Length Tutorial',
    'How to get an array size',
    'Jonathan Bossenger'
);
```

数组中的每一项都可以通过它的数字键来引用。因此，不需要调用单个变量，您可以通过数字键引用单个数组项。

```
echo $post_data[0];
```

In PHP, Array keys start at 0

为了更好地控制，数组还允许您使用字符串定义自己的数组键。

```
$post_data = array(
    'heading' => 'PHP Array Length Tutorial',
    'subheading' => 'How to get an array size',
    'author' => 'Jonathan Bossenger'
);
```

这也允许您通过字符串键引用数组项。

```
echo $post_data['heading'];
```

您还可以使用新的短数组符号来定义数组，这类似于 JavaScript:

```
$post_data = [
    'heading' => 'PHP Array Length Tutorial',
    'subheading' => 'How to get an array size',
    'author' => 'Jonathan Bossenger'
];
```

数组也可以嵌套，形成更复杂的数组变量:

```
$post_data = [
    'heading' => 'PHP Array Length Tutorial',
    'subheading' => 'How to get an array size',
    'author' => [
        'name' => 'Jonathan Bossenger',
        'twitter' => 'jon_bossenger',
    ]
]; 
```

并且，您可以使用嵌套键调用特定的数组值:

```
echo $post_data['author']['name'];
```

然而，如果你发现自己经常这样做，你可能想考虑使用[对象](https://www.php.net/manual/en/language.types.object.php)而不是数组。

如果您需要快速收集并在一个函数中使用不同的相关数据，或者将这些数据传递给另一个函数，那么数组非常有用。

通过将这些数据放入一个数组中，您定义了更少的变量，这可以使您的代码在以后更容易阅读和理解。将单个数组变量传递给另一个函数也比传递多个字符串容易得多。

```
$post_data = [
    'heading' => 'PHP Array Length Tutorial',
    'subheading' => 'How to get an array size',
    'author' => [
        'name' => 'Jonathan Bossenger',
        'twitter' => 'jon_bossenger',
    ]
];

$filtered_post_data = filter_post_data($post_data)
```

## 如何在 PHP 中获取数组的大小

通常当我们谈论数组的大小时，我们谈论的是数组中有多少个元素。有两种常见的方法来获取数组的大小。

最流行的方法是使用 PHP 的 [count()](https://www.php.net/manual/en/function.count.php) 函数。正如函数名所示，`count()`将返回一个数组的元素数。但是我们如何使用`count()`函数取决于数组结构。

让我们看看我们之前定义的两个示例数组。

```
$post_data = array(
	'heading' => 'PHP Array Length Tutorial',
	'subheading' => 'How to get an array size',
	'author' => 'Jonathan Bossenger'
);

echo count($post_data);
```

在本例中，`count($post_data)`将得到 3。这是因为数组中有 3 个元素:“标题”、“副标题”和“作者”。但是我们的第二个嵌套数组示例呢？

```
$post_data = [
	'heading' => 'PHP Array Length Tutorial',
	'subheading' => 'How to get an array size',
	'author' => [
		'name' => 'Jonathan Bossenger',
		'twitter' => 'jon_bossenger',
	]
];
echo count($post_data);
```

信不信由你，在这个例子中，`count($post_data)`也会返回 3。这是因为默认情况下，`count()`函数只计算顶级数组元素。

如果你看一下[函数定义](https://www.php.net/manual/en/function.count.php)，你会看到它接受两个参数——要计数的数组和一个`mode`整数。该模式的默认值是预定义的常数`COUNT_NORMAL`，它告诉函数只计算顶级数组元素。

如果我们改为传递预定义的常量`COUNT_RECURSIVE`，它将遍历嵌套的所有级别，并对这些级别进行计数。

```
$post_data = [
	'heading' => 'PHP Array Length Tutorial',
	'subheading' => 'How to get an array size',
	'author' => [
		'name' => 'Jonathan Bossenger',
		'twitter' => 'jon_bossenger',
	]
];
echo count($post_data, COUNT_RECURSIVE);
```

现在，`count($post_data, COUNT_RECURSIVE)`的结果将会是，不出所料，5。

“但是等等！”，我听到你哭了。“你提到还有另一种方法？”。

是的，你可以使用的另一个函数是 [sizeof()](https://www.php.net/manual/en/function.sizeof.php) 。然而，`sizeof()`只是`count()`的别名，许多人认为`sizeof()`会返回数组的内存使用情况。

因此最好坚持使用`count()`，这是一个更适合你正在做的事情的名字——计算数组中的元素。

感谢阅读！我希望你现在对如何在 PHP 中找到数组的大小有了更好的理解。