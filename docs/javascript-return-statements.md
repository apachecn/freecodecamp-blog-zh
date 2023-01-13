# JavaScript 返回语句

> 原文：<https://www.freecodecamp.org/news/javascript-return-statements/>

## **简介**

当一个 ****返回**** 语句在一个函数中被调用时，该函数停止执行。如果指定，将给定值返回给函数调用方。如果省略表达式，则返回`undefined`。

```
 return expression;
```

函数可以返回:

*   原始值(字符串、数字、布尔值等。)
*   对象类型(数组、对象、函数等。)

不要在新的一行中返回没有使用括号的内容。这是一个 JavaScript 怪癖，结果将是不确定的。在多行中返回内容时，尽量使用括号。

```
function foo() {
    return 
      1;
}

function boo() {
    return (
      1
    );
}

foo(); --> undefined
boo(); --> 1
```

## **例题**

以下函数返回其参数的平方， ****x**** ，其中 ****x**** 是一个数字。

```
 function square(x) {
       return x * x;
    }
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/C7VT/0)

以下函数返回其参数的乘积， ****arg1**** 和 ****arg2**** 。

```
 function myfunction(arg1, arg2){
       var r;
       r = arg1 * arg2;
       return(r);
    }
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/C7VU/0)

当函数`return`是一个值时，可以使用赋值操作符(`=`)将该值赋给一个变量。在下面的示例中，函数返回参数的平方。当函数解析或结束时，它的值就是`return` ed 值。然后将该值赋给变量`squared2`。

```
 function square(x) {
        return x * x;
    }

    let squared2 = square(2); // 4
```

如果没有显式的 return 语句，意味着函数缺少关键字`return`，那么函数会自动返回`undefined`。

在下面的例子中，`square`函数缺少`return`关键字。当调用函数的结果被赋给一个变量时，该变量的值为`undefined`。

```
 function square(x) {
        let y = x * x;
    }

    let squared2 = square(2); // undefined
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/M8BE)