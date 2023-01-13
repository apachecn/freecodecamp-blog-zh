# 我是如何发现 C++算法库并学会不重复发明轮子的

> 原文：<https://www.freecodecamp.org/news/how-i-discovered-the-c-algorithm-library-and-learned-not-to-reinvent-the-wheel-2398a34e23e3/>

前几天出于好奇，我查了一下 C++算法库。并发现了相当多的酷功能！

这着实让我吃惊。

为什么？我的意思是，在我的大学生活中，我大部分时间都在写 C++。尤其是因为我对竞争性编程又爱又恨。

非常不幸的是，我从未真正利用过 C++提供给我们的这个神奇的库。

天哪，我觉得自己太天真了！

所以我决定是时候停止天真，开始了解 C++算法的用处了——至少在更高的层面上。正如一位老人曾经说过的，*分享知识就是力量——*所以我来了。

*****免责声明***** *:我大量使用了 C++11 及以后版本的特性。如果您不太熟悉该语言的新版本，我在这里提供的代码片段可能会显得有点笨拙。另一方面，我们在这里讨论的库远比我在下面写的任何东西都要自给自足和优雅。随时发现错误并指出来。此外，在这篇文章中，我不能真正考虑 C++17 的许多附加功能，因为它的大部分功能还没有在 GCC 中实现。*

所以事不宜迟，让我们开始吧！

1.  `****all_of any_of none_of****`

这些函数简单地寻找容器元素的`****all****` *、* `****any****` 或`****none****` 是否遵循您定义的某些特定属性。查看下面的示例:

```
std::vector<int> collection = {3, 6, 12, 6, 9, 12};

// Are all numbers divisible by 3?
bool divby3 = std::all_of(begin(collection), end(collection), [](int x) {
    return x % 3 == 0;
});
// divby3 equals true, because all numbers are divisible by 3

// Is any number divisible by 2?
bool divby2 = std::any_of(begin(collection), end(collection), [](int x) {
    return x % 2 == 0;
});
// divby2 equals true because 6, 12 divisible by 2

// Is no number divisible by 6?
bool divby6 = std::none_of(begin(collection), end(collection), [](int x) {
    return x % 6 == 0;
});
// divby6 equals false because 6, 12 divisible by 6
```

注意在这个例子中，*的特定属性*是如何作为 lambda 函数传递的。

所以`****all_of, any_of, none_of****`在你的`****collection****`中寻找一些特定的属性。这些函数对于它们应该做什么是非常清楚的。随着 C++11 中 ****lambdas**** 的引入，它们使用起来相当方便。

2.`****for_each****`

我一直很习惯使用古老的`****for****` loop，这个可爱的东西从来没有穿过我的视线。基本上，`****for_each****`将一个函数应用于一个容器的范围。

```
std::vector<int> collection = {2,4,4,1,1,3,9};

// notice that we pass x as reference!
std::for_each(begin(collection), end(collection), [] (int &x) {
    x += 26;
});
```

如果您是一名 JavaScript 开发人员，上面的代码应该会引起您的注意。

3.`****count count_if****`

与开头描述的函数非常相似，`****count****`和`****count_if****`都在给定的数据集合中寻找特定的属性。

```
std::vector<int> collection={1, 9, 9, 4, 2, 6};

// How many 9s are there in collection?
int nines = std::count(begin(collection), end(collection), 9);
// How many elements of the collection are even?
int evens = std::count_if(begin(collection), end(collection), [](int x) {
    return x % 2 == 0;
});
// nines equals 2, evens equals 3
```

结果，你得到的 ****计数**** 与你给定的值相匹配，或者具有你以 lambda 函数形式提供的给定属性。

4.`****find_if****`

假设您希望找到集合中满足特定属性的第一个元素。可以用`****find_if****`。

```
std::vector<int> collection = {1, 2, 0, 5, 0, 3, 4};

// itr contains the iterator to the first element following the specific property
auto itr = std::find_if(begin(collection), end(collection), [](int x) {
    return x % 2==0; // the property
});
```

记住，如上例所示，你会得到 ****迭代器**** 到 ****第一个元素**** 匹配你给定的属性。那么如果你想用`****find_if****`找到所有匹配属性的元素呢？

![0_C0IjBIkmmXBEqCEk](img/54e07c4d560d5fa2e9c854a006a0deef.png)

An abstract art to look at if you are getting bored. ([Steve Johnson](https://unsplash.com/@steve_j?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral))

5.`****generate****`

这个函数本质上是根据你提供的 ****生成器**** 来改变你的集合的值，或者它的一个范围。生成器是形式
`****T f();****`的函数，其中`****T****`是与我们的集合兼容的类型。

```
std::vector<int> collection={1, 2, 0, 5, 0, 3, 4};

int counter=0;

// notice that we are capturing counter by reference
std::generate(begin(collection), end(collection), [&]() {
    return counter++;
});

// collection gets replaced by values starting from 0
// modified collection = {0,1,2,3,4,5,6}
```

在上面的例子中，请注意，我们实际上是在原地改变我们的集合*。这里的生成器是我们提供的 lambda 函数。*

6.`****shuffle****`

从标准的 C++17 中，`****random_shuffle****` 被删除了。现在我们更喜欢更有效的`****shuffle****`，因为它利用了头球`****random****`。

```
std::vector<int> collection = {1, 2, 13, 5, 12, 3, 4};

std::random_device rd;
std::mt19937 rand_gen(rd());
std::shuffle(begin(collection), end(collection), rand_gen);
```

注意，我们使用的是 [Mersenne Twister](https://en.wikipedia.org/wiki/Mersenne_Twister) ，这是 C++11 中引入的伪随机数生成器。

随着`****random****` 库的引入和更好方法的引入，随机数生成器在 C++中已经变得更加成熟。

7.`****nth_element****`

这个函数非常有用，因为它有一个有趣的复杂性。

假设您想知道集合的第*n*个元素是否已排序，但是您不想对集合进行排序以执行*****O(n log(n))******操作。*

*你会怎么做？*

*那么`****nth_element****`就是你的朋友。它在*****【n】******中找到想要的元素。**

```
*`std::vector<int> collection = {1, 2, 13, 5, 12, 3, 4};

auto median_pos = collection.begin() + collection.size() / 2;
std::nth_element(begin(collection), median_pos, end(collection));

// note that the original vector will be changed due to the operations
// done by nth_element`*
```

*有趣的是，`****nth_element****`可能会也可能不会将你的收藏分类。它会按照任何顺序找到第 n 个元素。这里有一个关于 [StackOverflow](https://stackoverflow.com/questions/10352442/whats-the-practical-difference-between-stdnth-element-and-stdsort) 的有趣讨论。*

*同样，你可以添加你自己的比较函数(就像我们在前面的例子中添加的 lambdas)来使它更有效。*

*8.`****equal_range****`*

*假设你有一个有序的整数集合。您希望找到所有元素都有特定值的范围。例如:*

```
*`// sorted collection
std::vector<int> collection={1, 2, 5, 5, 5, 6, 9, 12};

// we are looking for a range where all elements equal to 5
auto range = std::equal_range(begin(collection), end(collection), 5);

// the required range is printed like this
std::cout << (range.first - begin(collection)) << " " <<
  			 (range.second - begin(collection)) << std::endl;`*
```

*在这段代码中，我们在`****vector****`中寻找一个包含所有`****5****`的 ****范围**** 。答案是`****(2~4)****`。*

*当然，我们可以将这个函数用于我们自己的自定义属性。您需要确保您拥有的属性与数据的顺序一致。参考见[这篇文章。](https://en.cppreference.com/w/cpp/algorithm/equal_range)*

*最后，`****lower_bound****`和`****upper_bound****`都可以帮助你达到和使用`****equal_range****`一样的效果。*

*9.`****merge inplace_merge****`*

*假设你有两个排序的集合(这是多么有趣的事情啊，对吧？)，您希望合并它们，并且您还希望合并后的集合保持排序。你会怎么做？*

*您可以将第二个集合添加到第一个集合中，并再次对结果进行排序，这将添加一个额外的****O(log(n))*****因子。相反，我们可以只使用`****merge****`。**

```
**`std::vector<int> c1 = {1, 2, 5, 5, 5, 6, 9, 12};
std::vector<int> c2 = {2, 4, 4, 5, 7, 15};

std::vector<int> result; // contains merged elements
std::merge(begin(c1), end(c1), begin(c2), end(c2), std::back_inserter(result));

// result = {1, 2, 2, 4, 4, 5, 5, 5, 5, 6, 7, 9, 12, 15}`**
```

**另一方面，你还记得在实现*合并排序的时候，*我们需要合并数组的两边吗？`****inplace_merge****`可以方便地用于那方面。**

**根据 [cppreference](https://en.cppreference.com/w/cpp/algorithm/inplace_merge) 中给出的例子，看看这个小小的*合并排序*:**

```
**`void merge_sort(auto l, auto r)
{
    if(r - l > 1)
    {
        auto mid = l+(r-l)/2;
        merge_sort(l, mid);
        merge_sort(mid, r);
        std::inplace_merge(l, mid, r);
    }
}

std::vector<int> collection = {2, 4, 4, 1, 1, 3, 9};
merge_sort(begin(collection), end(collection));`**
```

**多酷啊！**

**![0_zgexhkawrSJNYfNM](img/48d6d68162bbb4679c112463e4de619d.png)

Speaking of cool, here is a cool guy. ? ([Dawid Zawiła](https://unsplash.com/@davealmine?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral))** 

**10.`****minmax minmax_element****`**

**`****minmax****`返回给定两个值或给定列表的最小值和最大值。它返回一对，还可以提供您自己的比较方法的功能。`****minmax_element****`对你的容器做同样的事情。**

```
**`int a = 9, b = 12;

// out.first contains the minimum element, out.second is the maximum one
auto out = std::minmax(a, b);

std::vector<int> collection = {6, 5, 3, 2, 1, 4, 6, 7};
auto result = std::minmax_element(begin(collection), end(collection));

// you can also add compare function as the third argument
// (result.first - collection.begin()) is the index of the minimum element
// (result.second - collection.begin()) is the index of the maximum element`**
```

**11.`****accumulate partial_sum****`**

**`****accumulate****`做它所说的，它*使用初始值和一个二元运算函数，在给定范围内累加你的集合的*值。自己看:**

```
**`std::vector<int> collection = {6, 5, 3, 2, 1, 4, 6, 7};

// Note that we are providing 0 as the initial value, as it should be.
// std::plus<int>() tells that the function should do sums
int sum = std::accumulate(begin(collection), end(collection), 0, std::plus<int>());

// What would happen if initial value was 0 instead of 1 in this call?
int prod = std::accumulate(begin(collection), end(collection), 1, std::multiplies<int>());

// You can also use your custom binary operation.
int custom = std::accumulate(begin(collection), end(collection), 0, [](int x, int y) {
    return x+y;
});`**
```

**那么`****custom****`的值是怎么算出来的呢？**

**开始时，accumulate 将初始值(0)传递给参数`****x****`，集合中的第一个值(6)传递给参数`****y****`，进行运算，然后将其赋给累加值。在第二次调用中，它将累加值传递给`****x****`，并将集合中的下一个元素传递给`****y****`，如此继续。**

**`****partial_sum****`做的事情很像 accumulate，但是它也将第一个`****n****` 项的结果保存在目标容器中。**

```
**`std::vector<int> collection = {6, 5, 3, 2, 1, 4, 6, 7};
std::vector<int> sums, mults;

// contains the partial sum of collection in result
std::partial_sum(begin(collection), end(collection), std::back_inserter(sums));

// contains the partial product
std::partial_sum(begin(collection), end(collection), std::back_inserter(mults), std::multiplies<int>());`**
```

**当然，正如您所料，您可以使用自己的自定义操作。**

**12.`****adjacent_difference****`**

**你想找到你的值的相邻差异，你可以简单地使用这个函数。**

```
**`std::vector<int> collection = {6, 5, 3, 2, 1, 4, 6, 7};
std::vector<int> diffs;
std::adjacent_difference(begin(collection), end(collection), std::back_inserter(diffs));
// The first element of diffs will be same as the first element of collection`**
```

**很简单，对吧？**

**但是它可以做得更多。看看这个:**

```
**`std::vector<int> fibs(10, 1);
std::adjacent_difference(begin(fibs), end(fibs) - 1, begin(fibs) + 1, std::plus<>{});`**
```

**这两条线是做什么的？他们找到了前 10 个斐波那契数列！你明白了吗？？**

* * *

**今天到此为止。感谢阅读！我希望你学到了新东西。**

**在不久的将来，我一定会再为你们带来一些新的东西。**

**干杯！**