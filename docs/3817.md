# C 语言中的三元运算符讲解

> 原文：<https://www.freecodecamp.org/news/c-ternary-operator/>

程序员使用**三元运算符**代替更长的 **if** 和 **else** 条件语句进行决策。

三元运算符有三个参数:

1.  第一个是比较论证
2.  第二个是真实比较的结果
3.  第三个是错误比较的结果

这有助于将三元运算符视为一种速记方式或编写 if-else 语句。下面是一个简单的决策例子，使用 ****if**** 和 ****else**** :

```
int a = 10, b = 20, c;

if (a < b) {
    c = a;
}
else {
    c = b;
}

printf("%d", c);
```

这个例子需要 10 多行代码，但这不是必须的。您可以使用三元运算符，只用 3 行代码编写上述程序。

### **语法**

`condition ? value_if_true : value_if_false`

如果满足`condition`，则该语句评估为`value_if_true`，否则评估为`value_if_false`。

下面是使用三元运算符重写的上述示例:

```
int a = 10, b = 20, c;

c = (a < b) ? a : b;

printf("%d", c);
```

上面示例的输出应该是:

```
10
```

`c`被设置为等于`a`，因为条件`a < b`为真。

记住参数`value_if_true`和`value_if_false`必须是同一类型，而且必须是简单的表达式而不是完整的语句。

三元运算符可以像 if-else 语句一样嵌套。考虑以下代码:

```
int a = 1, b = 2, ans;
if (a == 1) {
    if (b == 2) {
        ans = 3;
    } else {
        ans = 5;
    }
} else {
    ans = 0;
}
printf ("%d\n", ans);
```

下面是上面使用嵌套三元运算符重写的代码:

```
int a = 1, b = 2, ans;
ans = (a == 1 ? (b == 2 ? 3 : 5) : 0);
printf ("%d\n", ans);
```

上面两组代码的输出应该是:

```
3
```