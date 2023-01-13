# C - Struct 和 Typedef 中的结构化数据类型举例说明

> 原文：<https://www.freecodecamp.org/news/structured-data-types-in-c-struct-and-typedef-explained-with-examples/>

在您的编程经历中，您可能会觉得需要定义自己的数据类型。在 C 语言中，这是通过两个关键字完成的:`struct`和`typedef`。结构和联合将使您有机会将不同的数据类型存储到一个集合中。

## **声明新的数据类型**

```
typedef struct student_structure{
    char* name;
    char* surname;
    int year_of_birth;
}student;
```

在这段小代码之后，`student`将是一个新的保留关键字，你将能够创建类型为`student`的变量。请记住，这种新的变量将被结构化，这意味着定义一个物理分组的变量列表，放在内存块中的一个名称下。

## **新数据类型用法**

现在让我们创建一个新的`student`变量并初始化它的属性:

```
 student stu;

   strcpy( stu.name, "John");
   strcpy( stu.surname, "Snow"); 
   stu.year_of_birth = 1990;

   printf( "Student name : %s\n", stu.name);
   printf( "Student surname : %s\n", stu.surname);
   printf( "Student year of birth : %d\n", stu.year_of_birth);
```

正如您在这个示例中看到的，您需要为新数据类型中包含的所有变量赋值。要访问一个结构变量，你可以使用类似于`stu.name`中的点。向结构赋值还有一种更简单的方法:

```
typedef struct{
   int    x;
   int    y;
}point;

point image_dimension = {640,480};
```

或者如果您喜欢按照不同的顺序设置它的值:

```
point img_dim = { .y = 480, .x = 640 };
```

## **工会 vs 结构**

联合是在与结构相同的 was 中声明的，但它们是不同的，因为在任何时候联合中只能使用一项。

```
typedef union{
      int circle;
      int triangle;
      int ovel;
}shape;
```

在只应用一个条件和只使用一个变量的情况下，您应该使用`union`。请不要忘记，我们也可以使用我们全新的数据类型:

```
typedef struct{
      char* model;
      int year;
}car_type;

typedef struct{
      char* owner;
      int weight;
}truck_type;

typedef union{
  car_type car;
  truck_type truck;
}vehicle;
```

## **再来几招**

*   当你使用`&`操作符创建一个指向结构的指针时，你可以使用特殊的`->`中缀操作符来遵从它。例如，在 C 语言中处理链表时，这是非常常用的
*   新定义的类型可以像其他基本类型一样用于几乎所有事情。例如，尝试创建一个类型为`student`的数组，看看它是如何工作的。
*   结构可以被复制或赋值，但不能进行比较！

## 更多信息:

*   [C 初学者手册:在几个小时内学会 C 编程语言基础知识](https://www.freecodecamp.org/news/the-c-beginners-handbook/)
*   [C-Integer、Floating Point 和 Void 中的数据类型解释](https://www.freecodecamp.org/news/structured-data-types-in-c-struct-and-typedef-explained-with-examples/www.freecodecamp.org/news/data-types-in-c-integer-floating-point-and-void-explained/)
*   [C 中的 malloc:解释了 C 中的动态内存分配](https://www.freecodecamp.org/news/structured-data-types-in-c-struct-and-typedef-explained-with-examples/www.freecodecamp.org/news/malloc-in-c-dynamic-memory-allocation-in-c-explained/)