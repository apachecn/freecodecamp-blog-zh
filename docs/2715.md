# 命令式编程与声明式编程——用简单的英语解释区别

> 原文：<https://www.freecodecamp.org/news/imperative-vs-declarative-programming-difference/>

作为一名编码教师，我的职责是让程序员用新的方式思考。当我们从命令式编程切换到声明式编程时，思维发生了重大转变。

一旦我的学生学会了基本的 JavaScript，我们就复习函数式编程和声明式编码风格中使用的数组方法。这是他们的大脑开始爆裂、咝咝作响、像火上的棉花糖一样融化的地方。

![marshmellows-on-grill-crop-1](img/34b81e1d992c378d612df1baa647c033.png)

## 什么是命令式编程？

作为一个初学者，你可能大部分时候都是用命令式的风格来编码:你给计算机一组指令，让它按照一个容易遵循的顺序去做你想做的事情。

假设我们有一个世界上最常用的密码列表:

```
const passwords = [
   "123456",
   "password",
   "admin",
   "freecodecamp",
   "mypassword123",
];
```

我们的应用程序将在注册时检查用户的密码，不允许他们创建一个来自这个列表的密码。

但在此之前，我们想完善这个列表。我们已经有了不允许用户使用少于 9 个字符的密码注册的代码。因此，我们可以将该列表简化为 9 个字符或更多的密码，以加快检查速度。

我们必须写下:

```
// using the passwords constant from above

let longPasswords = [];
for (let i = 0; i < passwords.length; i++) {
   const password = passwords[i];
   if (password.length >= 9) {
      longPasswords.push(password);
   }
}

console.log(longPasswords); // logs ["freecodecamp", "mypassword123"];
```

1.  我们创建一个名为`longPasswords`的空列表。
2.  然后我们编写一个循环，它将运行与原始`passwords`列表中的密码一样多的次数。
3.  然后，我们在当前循环迭代的索引处获得密码。
4.  然后我们检查密码是否大于或等于 9 个字符。
5.  如果是，我们将它放入`longPasswords`列表。

命令式编程的优势之一是它很容易推理。像电脑一样，我们可以一步一步地跟随。

![steps-crop](img/8cc1841f0c1c82507e33de1941f0b704.png)

## 什么是声明式编程？

但是还有另一种思考编码的方式——作为一个不断定义事物是什么的过程。这被称为声明式编程。

命令式和声明式编程实现了相同的目标。它们只是思考代码的不同方式。它们各有利弊，有时两者都可以使用。

虽然命令式编程对于初学者来说更容易理解，但是声明式编程允许我们编写更可读的代码，以反映我们想要看到的东西。结合[好的变量名](https://github.com/10xcodecamp/javascript-conventions-and-code-style)，可以成为一个强大的工具。

因此，我们不是给计算机一步一步的指令，而是声明我们想要什么，并把它分配给某个过程的结果。

```
// using the passwords constant from above

const longPasswords = passwords.filter(password => password.length >= 9);

console.log(longPasswords); // logs ["freecodecamp", "mypassword123"];
```

`longPasswords`列表被定义(或声明)为仅过滤大于或等于 9 个字符的密码的`passwords`列表。

JavaScript 中的函数式编程方法使我们能够清晰地声明事物。

*   这是密码列表。
*   **这是一个只有长密码的列表。**(运行后`filter`)。)
*   **这是一个带 id 的密码列表。**(运行后`map`)。)
*   **这是一个单一的密码。**(运行后`find`)。)

声明式编程的优势之一是它迫使我们先问自己想要什么。正是在这些新事物的命名中，我们的代码变得更有表现力和更清晰。

当我们的开发伙伴来看我们的代码时，他们可以更容易地找到错误:

“您将这个变量称为‘index ’,这使我期望一个数字，但我看到它是返回一个数组的`filter`的结果。那是怎么回事？”

![women-coding-at-home-crop](img/1d8e29f94effb91ecda2f96981f8e3f9.png)

我鼓励学习者尽可能多地编写声明性代码，不断地定义(和重构来重新定义)事物是什么。

与其在你的头脑中保留一个完整的命令式过程，你可以在你的头脑中保留一个更加有形的**事物**和一个清晰的定义。

*迈克·泽特洛是* [*10x 代码营*](https://www.10xcodecamp.com/) *的首席教官。*