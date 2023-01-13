# C++地图举例说明

> 原文：<https://www.freecodecamp.org/news/c-plus-plus-map-explained-with-examples/>

`map`是一个以键值对的形式存储元素的容器。它类似于 Java 中的集合、PHP 中的关联数组或 JavaScript 中的对象。

以下是使用`map`的主要好处:

*   仅存储唯一的键，并且键本身是按排序的顺序排列的
*   因为键已经按顺序排列，所以搜索元素非常快
*   每个键只有一个值

这里有一个例子:

```
#include <iostream>
#include <map>

using namespace std;

int main (){
  map<char,int> first;

  //initializing
  first['a']=10;
  first['b']=20;
  first['c']=30;
  first['d']=40;

   map<char, int>::iterator it;
   for(it=first.begin(); it!=first.end(); ++it){
      cout << it->first << " => " << it->second << '\n';
   }

  return 0;
}
```

输出:

```
a => 10
b => 20
c => 30
d => 40
```

### 创建一个`map`对象

`map<string, int> myMap;`

### 插入

用插入成员函数插入数据。

```
myMap.insert(make_pair("earth", 1));
myMap.insert(make_pair("moon", 2));
```

我们还可以使用运算符[]将数据插入 std::map，即

`myMap["sun"] = 3;`

### 访问`map`元素

要访问 map 元素，您必须为它创建迭代器。这里有一个如前所述的例子。

```
map<char, int>::iterator it;
for(it=first.begin(); it!=first.end(); ++it){
  cout << it->first << " => " << it->second << '\n';
}
```