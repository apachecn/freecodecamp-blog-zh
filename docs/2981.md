# Java 数组方法——如何用 Java 打印一个数组

> 原文：<https://www.freecodecamp.org/news/java-array-methods-how-to-print-an-array-in-java/>

数组是一种用于存储相同类型数据的数据结构。数组将它们的元素存储在连续的内存位置。

在 Java 中，数组是对象。类对象的所有方法都可以在一个数组中调用。我们可以在一个数组中存储固定数量的元素。

让我们声明一个简单的原始类型的数组:

```
int[] intArray = {2,5,46,12,34};
```

现在让我们试着用`System.out.println()`方法打印它:

```
System.out.println(intArray);
// output: [I@74a14482
```

Java 为什么不打印我们的数组？引擎盖下发生了什么？

`System.out.println()`方法通过调用`String.valueOf()`将我们传递的对象转换成一个字符串。如果我们看看`String.valueOf()`方法的实现，我们会看到:

```
public static String valueOf(Object obj) {
    return (obj == null) ? "null" : obj.toString();
}
```

如果传入的对象是`null`，则返回 null，否则调用`obj.toString()`。最终，`System.out.println()`调用`toString()`来打印输出。

如果该对象的类没有覆盖`Object.toString()`的实现，它将调用`Object.toString()`方法。

`Object.toString()`返回`getClass().getName()+****‘@’****+Integer.toHexString(hashCode())`。简单来说就是返回:“类名@对象的哈希码”。

在我们之前的输出`[I@74a14482`中，`[`表示这是一个数组，`I`代表 int(数组的类型)。`74a14482`是数组的哈希码的无符号十六进制表示。

每当我们创建自己的定制类时，重写`Object.toString()`方法是一个最佳实践。

我们不能使用普通的`System.out.println()`方法在 Java 中打印数组。相反，我们可以通过以下方式打印数组:

1.  循环:for 循环和 for-each 循环
2.  `Arrays.toString()`方法
3.  `Arrays.deepToString()`方法
4.  `Arrays.asList()`方法
5.  Java 迭代器接口
6.  Java 流 API

让我们一个一个来看。

# 1.循环:for 循环和 for-each 循环

以下是 for 循环的一个示例:

```
int[] intArray = {2,5,46,12,34};

for(int i=0; i<intArray.length; i++){
    System.out.print(intArray[i]);
    // output: 25461234
}
```

所有的包装器类都覆盖了`Object.toString()`，并返回它们的值的字符串表示。

这是一个 for-each 循环:

```
int[] intArray = {2,5,46,12,34};

for(int i: intArray){
    System.out.print(i);
    // output: 25461234
}
```

# 2.数组. toString()方法

`Arrays.toString()`是属于`java.util`包的数组类的静态方法。它返回指定数组内容的字符串表示形式。我们可以用这种方法打印一维数组。

使用`String.valueOf()`方法将数组元素转换成字符串，如下所示:

```
int[] intArray = {2,5,46,12,34};
System.out.println(Arrays.toString(intArray));
// output: [2, 5, 46, 12, 34]
```

对于数组的引用类型，我们必须确保引用类型类覆盖了`Object.toString()`方法。

例如:

```
public class Test {
    public static void main(String[] args) {
        Student[] students = {new Student("John"), new Student("Doe")};

        System.out.println(Arrays.toString(students));
        // output: [Student{name='John'}, Student{name='Doe'}]
    }
}

class Student {
    private String name;

    public Student(String name){
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{" + "name='" + name + '\'' + '}';
    }
}
```

此方法不适用于多维数组。它使用`Object.toString()`将多维数组转换成字符串，T0 描述了它们的身份而不是它们的内容。

例如:

```
// creating multidimensional array
int[][] multiDimensionalArr = { {2,3}, {5,9} };

System.out.println(Arrays.toString(multiDimensionalArr));
// output: [[I@74a14482, [I@1540e19d]
```

在`Arrays.deepToString()`的帮助下，我们可以打印多维数组。

# 3.Arrays.deepToString()方法

`Arrays.deepToString()`返回指定数组“深层内容”的字符串表示。

如果一个元素是一个原始类型的数组，通过调用`Arrays.toString()`的适当重载，它被转换成一个字符串。

下面是多维数组基本类型的一个示例:

```
// creating multidimensional array
int[][] multiDimensionalArr = { {2,3}, {5,9} };

System.out.println(Arrays.deepToString(multiDimensionalArr));
// output: [[2, 3], [5, 9]]
```

如果一个元素是引用类型的数组，通过递归调用`Arrays.deepToString()`将它转换成一个字符串。

```
Teacher[][] teachers = 
{{ new Teacher("John"), new Teacher("David") }, {new Teacher("Mary")} };

System.out.println(Arrays.deepToString(teachers));
// output: 
[[Teacher{name='John'}, Teacher{name='David'}],[Teacher{name='Mary'}]]
```

我们必须在教师课上重写`Object.toString()`。

如果你想知道它是如何递归的，这里有`Arrays.deepToString()`方法的[源代码](http://hg.openjdk.java.net/jdk8u/jdk8u/jdk/file/be44bff34df4/src/share/classes/java/util/Arrays.java#l4611)。

****注意:**** 引用型一维数组也可以用这种方法打印。例如:

```
Integer[] oneDimensionalArr = {1,4,7};

System.out.println(Arrays.deepToString(oneDimensionalArr));
// output: [1, 4, 7]
```

# 4.Arrays.asList()方法

此方法返回由指定数组支持的固定大小的列表。

```
Integer[] intArray = {2,5,46,12,34};

System.out.println(Arrays.asList(intArray));
// output: [2, 5, 46, 12, 34]
```

我们已经将类型从 int 改为 Integer，因为 List 是一个保存对象列表的集合。当我们把一个数组转换成一个列表时，它应该是一个引用类型的数组。

Java 调用`Arrays.**asList**(intArray).toString()`。这种技术在内部使用列表中元素类型的`toString()`方法。

另一个例子是我们的定制教师类:

```
Teacher[] teacher = { new Teacher("John"), new Teacher("Mary") };

System.out.println(Arrays.asList(teacher));
// output: [Teacher{name='John'}, Teacher{name='Mary'}]
```

**注意:**我们不能用这种方法打印多维数组。例如:

```
Teacher[][] teachers = 
{{ new Teacher("John"), new Teacher("David") }, { new Teacher("Mary") }};

System.out.println(Arrays.asList(teachers));

// output: [[Lcom.thano.article.printarray.Teacher;@1540e19d, [Lcom.thano.article.printarray.Teacher;@677327b6]
```

# 5.Java 迭代器接口

类似于 for-each 循环，我们可以使用迭代器接口遍历数组元素并打印它们。

迭代器对象可以通过调用集合上的`iterator()`方法来创建。该对象将用于迭代该集合的元素。

下面是一个如何使用迭代器接口打印数组的例子:

```
Integer[] intArray = {2,5,46,12,34};

// creating a List of Integer
List<Integer> list = Arrays.asList(intArray);

// creating an iterator of Integer List
Iterator<Integer> it = list.iterator();

// if List has elements to be iterated
while(it.hasNext()) {
    System.out.print(it.next());
    // output: 25461234
}
```

# 6.Java 流 API

流 API 用于处理对象集合。流是一系列的对象。流不改变原始的数据结构，它们只根据请求的操作提供结果。

在`forEach()`终端操作的帮助下，我们可以遍历流中的每个元素。

例如:

```
Integer[] intArray = {2,5,46,12,34};

Arrays.stream(intArray).forEach(System.out::print);
// output: 25461234
```

现在我们知道了如何用 Java 打印一个数组。

感谢您的阅读。

在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Aziz Acharki](https://unsplash.com/@acharki95?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的封面图片。

你可以看我在[介质](https://medium.com/@mvthanoshan9/object-oriented-programming-principles-in-java-820919dced1a)上的其他文章。

****快乐编码！****