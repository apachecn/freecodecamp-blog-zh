# 如何在 C++中从容器中移除元素

> 原文：<https://www.freecodecamp.org/news/how-to-remove-elements-from-a-container-in-c/>

如何从容器中移除元素是一个常见的 C++面试问题，所以如果你仔细阅读这一页，你会得到一些印象分。

擦除-删除习语是一种 C++技术，用于从容器中删除满足特定标准的元素。然而，用传统的手写循环消除元素是可能的，但是擦除-移除习语有几个优点。

### **比较**

```
// Using a hand-written loop
std::vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
for (auto iter = v.cbegin(); iter < v.cend(); /*iter++*/)
{
    if (is_odd(*iter))
    {
        iter = v.erase(iter);
    }
    else
    {
        ++iter;
    }
}

// Using the erase–remove idiom
std::vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
v.erase(std::remove_if(v.begin(), v.end(), is_odd), v.end());
```

正如您所看到的，带有手写循环的代码需要更多的输入，但是它也有一个性能问题。每个`erase`调用必须将删除的元素之后的所有元素向前移动，以避免集合中的“间隙”。在同一个容器上多次调用`erase`会产生大量移动元素的开销。

另一方面，带有擦除-移除习语的代码不仅更具表现力，而且效率也更高。首先，使用`remove_if/remove`将不符合删除标准的所有元素移动到范围的前面，保持元素的相对顺序。所以在调用了`remove_if/remove`之后，对`erase`的一次调用就删除了该范围末尾的所有剩余元素。

### **例子**

```
#include <vector> // the general-purpose vector container
#include <iostream> // cout
#include <algorithm> // remove and remove_if

bool is_odd(int i)
{
    return (i % 2) != 0;
}

void print(const std::vector<int> &vec)
{
    for (const auto& i : vec)
        std::cout << i << ' ';
    std::cout << std::endl;
}

int main()
{
    // initializes a vector that holds the numbers from 1-10.
    std::vector<int> v = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
    print(v);

    // removes all elements with the value 5
    v.erase(std::remove(v.begin(), v.end(), 5), v.end());
    print(v);

    // removes all odd numbers
    v.erase(std::remove_if(v.begin(), v.end(), is_odd), v.end());
    print(v);

    // removes multiples of 4 using lambda
    v.erase(std::remove_if(v.begin(), v.end(), [](int n) { return (n % 4) == 0; }), v.end());
    print(v);

    return 0;
}

/*
Output:
1 2 3 4 5 6 7 8 9 10
1 2 3 4 6 7 8 9 10
2 4 6 8 10
2 6 10
*/
```

### **来源**

“删除习语”维基百科:免费百科全书。维基媒体基金会公司[en.wikipedia.org/wiki/Erase-remove_idiom](https://en.wikipedia.org/wiki/Erase%E2%80%93remove_idiom)

迈耶斯，斯科特(2001 年)。有效的 STL: 50 种改进标准模板库使用的具体方法。艾迪森-韦斯利。