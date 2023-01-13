# 如何在 Java 中使用 Arrays.binarySearch()

> 原文：<https://www.freecodecamp.org/news/how-to-use-arrays-binarysearch-in-java/>

在本文中，我将向您展示如何在 Java 中使用`Arrays.binarySearch()`方法。

## Java 里的`Arrays.binarySearch()`是什么？

根据[官方文件](https://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html#binarySearch(byte[],%20byte))上的`Arrays.binarySearch()`方法:

> (它)使用二分搜索法算法在指定的字节数组中搜索指定的值。
> 
> 在进行这个调用之前，必须对数组进行排序(通过 [`sort(byte[])`](https://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html#sort(byte[])) 方法)。如果不排序，结果是不确定的。
> 
> 如果数组包含多个指定值的元素，不保证会找到哪一个。

简单地说，`Arrays.binarySearch()`方法可以在一个排序的数组中查找给定的元素，如果找到就返回它的索引。

```
import java.util.Arrays;

public class Main {

	public static void main(String[] args) {
		char vowels[] = {'a', 'e', 'i', 'o', 'u'};

		char key = 'i';

		int foundItemIndex = Arrays.binarySearch(vowels, key);

		System.out.println("The given vowel is at index: " + foundItemIndex);

	}
} 
```

`Arrays.binarySearch()`方法将您要搜索的数组作为第一个参数，将您要查找的键作为第二个参数。该程序的输出将是:

```
The given vowel is at index: 2
```

请记住，该方法返回找到的项的索引，而不是项本身。因此，您可以将索引存储在一个整数中，就像本例中使用的那样。

默认情况下，方法使用数组的第一个索引作为搜索的起点，使用数组的长度作为搜索的终点。所以在这种情况下，起始索引是`0`，结束索引是`6`。

您可以自己定义起始和结束索引，而不是使用默认的起始和结束索引。例如，如果您想执行从索引`2`到索引`4`的搜索，您可以按如下方式执行:

```
import java.util.Arrays;

public class Main {

	public static void main(String[] args) {
		char vowels[] = {'a', 'e', 'i', 'o', 'u'};

		char key = 'i';
		int startIndex = 2;
		int endIndex = 4;

		int foundItemIndex = Arrays.binarySearch(vowels, startIndex, endIndex, key);

		System.out.println("The given vowel is at index: " + foundItemIndex);

	}
} 
```

在这种情况下，`Arrays.binarySearch()`方法将您要搜索的数组作为第一个参数，起始索引作为第二个参数，结束索引作为第三个参数，键作为第四个参数。

只要您将结束索引保持在数组的长度内，该方法应该可以正常工作。但是，如果超过这个值，就会得到`Array index out of range`异常。

这很简单，对吗？如果找到，方法返回元素的索引。但是如果它没有找到给定的元素会发生什么呢？

## 当`Arrays.binarySearch()`没有找到给定的元素时会发生什么？

再次按照[公文](https://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html#binarySearch(byte[],%20byte))上的`Arrays.binarySearch()`方法:

> (方法返回)搜索键的索引，如果它包含在指定范围内的数组中。否则，`(-(*insertion point*) - 1)`。
> 
> *插入点*被定义为键将被插入数组的点:大于键的范围中第一个元素的索引，或者如果范围中所有元素都小于指定的键，则为`toIndex`(结束索引)。
> 
> 注意，这保证了当且仅当找到键时，返回值将为> = 0。

不是很清楚对吗？让我解释一下。第一行声明，如果在数组中找到，该方法将返回搜索关键字的索引。

但如果没有找到，输出将等于`(-(*insertion point*) - 1)`的值。这里，基于搜索关键字，`insertion point`可以有不同的值。

假设我们有一个数组`[5, 6, 7, 8, 9, 10]`和一个显然不在数组中的搜索关键字`0`。在这种情况下，搜索关键字小于数组的所有元素。但是第一个大于搜索关键字的元素是`5`。所以在这种情况下`insertion point`将是:

```
(-(the index of the first element larger than the search key) - 1) = (0 - 1) = -1
```

您可以将它实现到代码片段中，如下所示:

```
package arrays;

import java.util.Arrays;

public class Main {

	public static void main(String[] args) {		
		int numbers[] = {5, 6, 7, 8, 9, 10};

		System.out.println(Arrays.binarySearch(numbers, 0)); // -1
	}
} 
```

再次假设我们有一个数组`[5, 6, 7, 8, 9, 10]`和一个显然不在数组中的搜索关键字`12`。在这种情况下，搜索关键字大于数组的所有元素。所以在这种情况下`insertion point`将会是:

```
(-(the ending index (-(6) - 1) = (-6 - 1) = -7
```

请记住，当您没有手动定义结束索引时，该方法使用数组的长度作为结束索引，在本例中是`6`。

您可以将它实现到代码片段中，如下所示:

```
import java.util.Arrays;

public class Main {

	public static void main(String[] args) {		
		int numbers[] = {5, 6, 7, 8, 9, 10};

		System.out.println(Arrays.binarySearch(numbers, 12)); // -7
	}
} 
```

但是，如果您按如下方式手动定义起始和结束索引，结果将会改变:

```
import java.util.Arrays;

public class Main {

	public static void main(String[] args) {
		int numbers[] = {5, 6, 7, 8, 9, 10};

		int startIndex = 1;
		int endIndex = 3;

		System.out.println(Arrays.binarySearch(numbers, startIndex, endIndex, 5)); // -2
		System.out.println(Arrays.binarySearch(numbers, startIndex, endIndex, 10)); // -4

	}
} 
```

尝试自己计算这些值。您也可以对如下字符使用`Arrays.binarySearch()`方法:

```
import java.util.Arrays;

public class Main {

	public static void main(String[] args) {
		char vowels[] = {'a', 'e', 'i', 'o', 'u'};

		char key = 'i';
		int startIndex = 2;
		int endIndex = 4;

		System.out.println(Arrays.binarySearch(vowels, startIndex, endIndex, key));

	}
} 
```

当没有找到给定的搜索关键字时，同样的原则也适用于这种情况。但是当比较数组中的字符和给定的搜索关键字时，将使用相应字符的 [ASCII 码](https://www.ascii-code.com/)。所以`A (65)`会比`a (97)`小。当交叉检查程序的输出时，请记住这一点。

## 包扎

这差不多就是这个了。我希望你现在明白如何使用`Arrays.binarySearch()`方法。

如果你有任何问题或者只是想和我交流，我在 [Twitter](https://twitter.com/frhnhsin) 和 [LinkedIn](https://www.linkedin.com/in/farhanhasin/) 上。直接给我发信息。