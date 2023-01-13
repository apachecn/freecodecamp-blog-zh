# C++ Vector——如何在 c++的构造函数中初始化 Vector

> 原文：<https://www.freecodecamp.org/news/cpp-vector-how-to-initialize-a-vector-in-a-constructor/>

当您在编程中处理变量或数据的集合时，通常会将它们存储在特定的数据类型中。

在 C++中，你可以把它们存储在数组、结构、向量、字符串等等。虽然这些数据结构有其独特的特性，但我们将主要关注初始化向量的各种方法。

在此之前，我们先简单谈谈向量，以及在 C++中处理数据集合时是什么让它们脱颖而出。

## C++中的向量是什么？

与 C++中分配给集合的内存是静态的数组不同，向量让我们创建更动态的数据结构。

这是 C++中的一个数组:

```
#include <iostream>
using namespace std;

int main() {
    string names[2] = {"Jane", "John"};
    cout << names[1];
    // John
}
```

上面代码中的数组被创建并分配了足够的空间来容纳两个条目。试图通过新索引分配新值会给我们带来错误。

有了向量，事情就有点不同了。当向量被定义时，我们不必指定它的容量。在底层，向量的分配内存随着向量大小的变化而变化。

## C++中向量的语法

声明一个`vector`不同于初始化它。声明一个`vector`意味着创建一个新的`vector`，而初始化包括将项目传递到`vector`。

下面是语法的样子:

```
vector <data_type> vector_name
```

每个新的`vector`必须以**向量**关键字开始声明。后面是尖括号，包含向量可以接受的数据类型，如字符串、整数等。最后，向量的名字，我们可以随便叫它什么。

请注意，您必须将`include <vector>`放在文件的顶部才能使用矢量。

## 如何在 C++中初始化一个向量

在这一节中，我们将回顾在 C++中初始化`vector`的不同方法。我们将把它们分成几个小节，每个小节都有一些例子。

先说最基础的。

### 如何在 C++中使用`push_back()`方法初始化一个向量

`push_back()`是 C++中可以用来与向量交互的众多方法之一。它接受作为参数传入的新项。这允许我们把新的条目推到一个`vector`的最后一个索引。

这里有一个例子:

```
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> myVector;

	myVector.push_back(5);
	myVector.push_back(10);
	myVector.push_back(15);

	for (int x : myVector)
		cout << x << " ";
		// 5 10 15 
} 
```

在上述代码中，我们创建了一个空的`vector` : `vector<int> myVector;`。

使用`push_back()`，我们向`vector`传递了三个新数字。

我们循环了这些新号码，并把它们记录到控制台上。

### 在 C++中声明向量时如何初始化向量

就像数组一样，我们可以在声明向量时给它赋值。

这里有一个例子:

```
#include <iostream>
#include <vector>
using namespace std;

int main() {
    vector<int> myVector{ 5, 10, 15 };

	for (int x : myVector)
		cout << x << " ";
		// 5 10 15 
} 
```

在这个例子中，声明和初始化是同时进行的。

在声明`vector`的时候，我们传入了三个数字，然后循环并打印出来。

你会注意到我把`int`放在了尖括号之间。这是为了表明`vector`将保存的数据是特定的整数。

### 如何在 C++中从数组初始化一个向量

在这一节中，我们将首先创建并初始化一个数组。然后，我们将使用两个 vector 方法——`begin()`和`end()`将数组中的所有项目复制到我们的 vector 中。

让我们看看它是如何工作的。

```
#include <iostream>
#include <vector>
using namespace std;

int main() {
    int myArray[] = { 5, 10, 15 };

    vector<int> myVector(begin(myArray), end(myArray));

	for (int x : myVector)
		cout << x << " ";
		// 5 10 15 
} 
```

我们也可以使用上面相同的方法从另一个向量初始化一个`vector`。您必须定义第一个`vector`，然后使用`begin()`和`end()`方法将其值复制到第二个`vector`中。

### 如何在 C++中通过指定大小和值来初始化向量

我们可以在声明时指定一个`vector`的大小和项目。这主要是在要求`vector`始终具有特定值的情况下。

这里有一个例子:

```
#include <iostream>
#include <vector>
using namespace std;

int main() {

  int num_of_items = 5; 

  vector<int> myVector(num_of_items, 2); 

  	for (int x : myVector)
		cout << x << " ";
// 		2 2 2 2 2 
}
```

在上面的代码中，我们首先定义了一个变量，并向它传递了一个值 5。这相当于`vector`将拥有的最大数值。

然后我们声明了我们的`vector` : `vector myVector(num_of_items, 2);`。第一个参数是变量的最大条目数，而第二个参数是要存储在`vector`中的实际条目。

我们循环浏览并注销了`vector`中的项目。我们把 2 印了五次。

### 如何在 C++中使用构造函数初始化向量

我们也可以在构造函数中初始化向量。我们可以让这些值变得有点动态。这样，我们就不必硬编码 vector 的项目。

这里有一个例子:

```
#include <iostream>
#include <vector>
using namespace std;

class Vector {
	vector<int> myVec;

public:
	Vector(vector<int> newVector) {
	    myVec = newVector;
	}

	void print() {
		for (int i = 0; i < myVec.size(); i++)
			cout << myVec[i] << " ";
	}

};

int main() {
    vector<int> vec;

	vec.push_back(5);
	vec.push_back(10);
	vec.push_back(15);

	Vector vect(vec);
	vect.print();
	// 5 10 15 
} 
```

让我们来分解代码。

```
class Vector {
	vector<int> myVec;

public:
	Vector(vector<int> newVector) {
	    myVec = newVector;
	}

	void print() {
		for (int i = 0; i < myVec.size(); i++)
			cout << myVec[i] << " ";
	}

};
```

我们创建了一个名为 **Vector** 的类。然后我们创建了一个名为`myVec`的向量变量。

之后，我们定义了我们的构造函数。构造函数有两个方法——一个接收初始化的`vector`,另一个输出`vector`中的项目。

```
int main() {
    vector<int> vec;

	vec.push_back(5);
	vec.push_back(10);
	vec.push_back(15);

	Vector vect(vec);
	vect.print();
	// 5 10 15 
}
```

最后，正如你在上面的代码中看到的，我们创建了一个新的`vector`并向它推送新的条目。然后，我们继续将这些项目记录到控制台。

`main`中的逻辑是在**向量**类中创建的，该类也有一个构造函数。

## 结论

在本文中，我们讨论了 C++中的向量。

我们从区分数组和向量开始。数组的大小是静态的，而向量则是动态的，可以随着条目的增加而扩展。

然后我们回顾了一些方法，我们可以用它们在 C++中初始化一个`vector`,每个部分都有例子。

编码快乐！