# 如何在 C#中从列表中移除项目

> 原文：<https://www.freecodecamp.org/news/how-to-remove-an-item-from-a-list-in-c/>

在 C#中构建应用程序时，您可能需要存储和操作数据集。**列表< T >** 类是**系统的成员。Collections.Generic** 命名空间，并使用它来存储相同数据类型的多个对象。

**List < T >** 类表示可以使用索引访问的强类型对象列表的集合。它的类包含各种搜索、排序和操作列表的方法。

在这篇文章中，我们将学习如何用 C#从列表中移除一个条目。从列表中移除项目有不同的方法。在这里，您将学习如何使用 **Remove()** 和 **RemoveAt()** 方法从**列表< T >** 类中移除一个项目。

## 如何使用 Remove()方法从列表中移除项目

让我们假设您已经有一个名字字符串的现有列表。

```
using System.Collections.Generic;

namespace Collections
{
    public class Program
    {
        static void Main(string[] args)
        {
            List<string> FirstName = new List<string>() { "John", "Jane", "Josh", "Debby", "Gilbert", "Joe" };
        }
    }
} 
```

使用 **Remove()** 方法删除列表中第一个出现的条目。它接受一个参数作为项，并移除该项的第一个匹配项。

下面是一段代码，展示了如何使用 **Remove()** 方法从列表中删除一个项目:

```
using System.Collections.Generic;

namespace Collections
{
    public class Program
    {
        static void Main(string[] args)
        {
            List<string> FirstName = new List<string>() { "John", "Jane", "Josh", "Debby", "Gilbert", "Joe" };

            //Iterating through the list before calling the Remove() method
            foreach(string names in FirstName)
            {
                Console.WriteLine(names);                
            }

            //Remove method
            FirstName.Remove("John");            

            //Iterating through the list after calling the Remove() method
            foreach (string names in FirstName)
            {
                Console.WriteLine(names);
            }
        }
    }
} 
```

在上面的代码中， **FirstName。Remove("John")** 删除值为 **"John"** 的第一个项目。然后我们遍历列表，查看操作前后列表的内容。

## 如何使用 RemoveAt()方法从列表中移除一个项目

使用我们创建的列表， **RemoveAt()** 方法将一个索引作为参数，并移除该索引处的项目。下面的代码片段展示了如何使用 **Remove()** 方法从列表中删除一个条目。

```
using System.Collections.Generic;

namespace Collections
{
    public class Program
    {
        static void Main(string[] args)
        {
            List<string> FirstName = new List<string>() { "John", "Jane", "Josh", "Debby", "Gilbert", "Joe" };

            //Iterating through the list before calling the RemoveAt() method
            foreach(string names in FirstName)
            {
                Console.WriteLine(names);                
            }

            //RemoveAt() method
            FirstName.RemoveAt(1);            

            //Iterating through the list after calling the RemoveAt() method
            foreach (string names in FirstName)
            {
                Console.WriteLine(names);
            }
        }
    }
} 
```

在上面的代码中， **FirstName。RemoveAt(1)** 删除索引 1 处的项目。有必要知道 **RemoveAt()** 方法采用一个从零开始的索引号(这意味着位置/索引从 0 开始，而不是 1)。然后我们遍历列表，查看列表操作前后的列表内容。

## 结论

在本文中，我们讨论了如何在 C#中从一个**列表< T >** 类中移除一个项目。我们还用例子演示了两种方法。

我希望这篇文章是有帮助的。**编码快乐！**