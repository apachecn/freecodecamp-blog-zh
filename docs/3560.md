# Python While 循环语句解释

> 原文：<https://www.freecodecamp.org/news/python-while-loop-statement-explained/>

## **While 循环语句**

Python 与其他流行语言一样利用了`while`循环。`while`循环评估一个条件，如果条件为真，则执行一段代码。代码块重复执行，直到条件变为假。

基本语法是:

```
counter = 0
while counter < 10:
   # Execute the block of code here as
   # long as counter is less than 10
```

下面显示了一个示例:

```
days = 0    
week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
while days < 7:
   print("Today is " + week[days])
   days += 1
```

输出:

```
Today is Monday
Today is Tuesday
Today is Wednesday
Today is Thursday
Today is Friday
Today is Saturday
Today is Sunday
```

以上代码的逐行解释:

1.  变量“天数”被设置为值 0。
2.  变量 week 被分配给包含一周中所有日期的列表。
3.  当循环开始时
4.  代码块将被执行，直到条件返回“真”。
5.  条件是“天数< 7”，rougly 说运行 while 循环，直到变量天数小于 7
6.  因此，当 days=7 时，while 循环停止执行。
7.  days 变量在每次迭代中都会更新。
8.  当 while 循环第一次运行时,“今天是星期一”一行被打印到控制台上，变量 days 变成等于 1。
9.  因为变量 days 等于小于 7 的 1，所以 while 循环再次执行。
10.  当控制台打印出“今天是星期天”时，变量 days 现在等于 7，while 循环停止执行。

#### **更多信息:**

*   [Python `while`语句文档](https://docs.python.org/3/reference/compound_stmts.html#the-while-statement)