# JavaScript 循环:解释标签语句、继续语句和中断语句

> 原文：<https://www.freecodecamp.org/news/javascript-loops-label-statement-continue-statement-and-break-statement-explained/>

## **标签声明**

****标签语句**** 与`break`和`continue`语句一起使用，用于标识`break`和`continue`语句适用的语句。

我们将在下面详细讨论`break`和`continue`语句。

### **语法**

```
labelname:
  statements
```

### **用途**

如果不使用`labeled`语句，`break`语句只能跳出循环或`switch`语句。使用`labeled`语句允许`break`跳出任何代码块。

#### **例子**

```
foo: {
  console.log("This prints:");
  break foo;
  console.log("This will never print.");
}
console.log("Because execution jumps to here!")
/* output
This prints:
Because execution jumps to here! */
```

当与`continue`语句一起使用时，`labeled`语句允许您跳过循环迭代，其优势在于当您有嵌套的循环语句时，能够从内部循环跳到外部循环。如果不使用`labeled`语句，你只能跳出现有的循环迭代到`next iteration of the same loop.`

#### **例子**

```
// without labeled statement, when j==i inner loop jumps to next iteration
function test() {
  for (var i = 0; i < 3; i++) {
    console.log("i=" + i);
    for (var j = 0; j < 3; j++) {
      if (j === i) {
        continue;
      }
      console.log("j=" + j);
    }
  }
}

/* output
i=0 (note j=0 is missing)
j=1
j=2
i=1
j=0 (note j=1 is missing)
j=2
i=2
j=0
j=1 (note j=2 is missing)
*/

// using a labeled statement we can jump to the outer (i) loop instead
function test() {
  outer: for (var i = 0; i < 3; i++) {
    console.log("i=" + i);
    for (var j = 0; j < 3; j++) {
      if (j === i) {
        continue outer;
      }
      console.log("j=" + j);
    }
  }
}

/*
i=0 (j only logged when less than i)
i=1
j=0
i=2
j=0
j=1
*/
```

## **中断语句**

****break**** 语句终止当前循环，`switch`或`label`语句，并将程序控制权转移给终止语句后的语句。

```
break;
```

如果在带标签的语句中使用了 ****break**** 语句，语法如下:

```
break labelName;
```

### 例子

下面的函数有一个 ****break**** 语句，当 ****i**** 为 3 时终止`while`循环，然后返回值 ****3 * x**** 。

```
function testBreak(x) {
  var i = 0;

  while (i < 6) {
    if (i == 3) {
      break;
    }
    i += 1;
  }

  return i * x;
}
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/C7VM/0)

在下面的示例中，计数器设置为从 1 计数到 99；然而， ****break**** 语句在 14 次计数后终止循环。

```
for (var i = 1; i < 100; i++) {
  if (i == 15) {
    break;
  }
}
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/C7VO/0)

## 连续语句

****continue**** 语句终止执行当前或标记循环的当前迭代中的语句，并继续执行下一次迭代的循环。

```
continue;
```

如果 ****continue**** 语句用在带标签的语句中，语法如下:

```
continue labelName;
```

与 ****中断**** 语句相反， ****继续**** 并不完全终止循环的执行；相反:

*   在一个`while`循环中，它跳回该条件。
*   在一个`for`循环中，它跳转到更新表达式。

### 例子

以下示例显示了一个`while`循环，该循环具有一个 ****继续**** 语句，当 ****i**** 的值为 3 时执行该语句。因此， ****n**** 取值为 1、3、7 和 12。

```
var i = 0;
var n = 0;

while (i < 5) {
  i++;

  if (i === 3) {
    continue;
  }

  n += i;
  console.log (n);
}
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/C7hx/0)

在下面的示例中，循环从 1 到 9 进行迭代。由于将 ****继续**** 语句与表达式`(i < 5)`一起使用，所以跳过了 ****继续**** 和`for`主体结束之间的语句。

```
for (var i = 1; i < 10; i++) {
    if (i < 5) {
        continue;
    }
    console.log (i);
}
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/C7hs/0)