# 初级开发人员编写超级清晰可读代码指南

> 原文：<https://www.freecodecamp.org/news/the-junior-developers-guide-to-writing-super-clean-and-readable-code-cd2568e08aae/>

编写代码是一回事，但编写干净、可读的代码是另一回事。但是什么是“干净的代码？”我为初学者编写了这篇简短的*干净代码指南*来帮助你掌握和理解干净代码的艺术。

想象你正在读一篇文章。有一个开头段落，让你对文章内容有一个简要的概述。有标题，每个标题都有一堆段落。段落是由相关的信息组成的，这些信息被组织在一起并排序，这样文章就“流畅”了，读起来也很好。

现在，想象这篇文章没有任何标题。有段落，但是很长，顺序混乱。你不能略读这篇文章，而必须真正深入到内容中去，才能对文章的内容有所了解。这可能会非常令人沮丧！

你的代码应该读起来像一篇好文章。把你的类/文件想象成标题，把你的方法想象成段落。句子是代码中的语句。以下是干净代码的一些特征:

1.  干净的代码是集中的——每个函数、每个类和模块应该做一件事，并且把它做好。
2.  它应该是优雅的——干净的代码应该简单易读。读它应该会让你微笑。这应该会让您想到“我确切地知道这段代码在做什么”
3.  干净的代码会被处理掉。有人花时间让它简单有序。他们对细节给予了适当的关注。他们在乎。
4.  测试应该通过——破损的代码并不干净！

说到今天的大问题——作为一名初级开发人员，你实际上如何编写干净的代码？以下是我的一些入门建议。

### 使用一致的格式和缩进

如果行距不一致，字体大小不一样，而且到处都是换行符，书就很难读。这同样适用于您的代码。

为了使代码清晰易读，请确保缩进、换行符和格式一致。这里有一个好的和坏的例子:

#### 好人

```
function getStudents(id) { 
     if (id !== null) { 
        go_and_get_the_student(); 
     } else { 
        abort_mission(); 
     } 
}
```

*   一眼就能看出函数中有一个`if/else`语句
*   大括号和一致的缩进使得很容易看到代码块的开始和结束位置
*   大括号是一致的——注意`function`和`if`的左大括号是如何在同一条线上的

#### 坏事

```
function getStudents(id) {
if (id !== null) {
go_and_get_the_student();} 
    else 
    {
        abort_mission();
    }
    }
```

哇哦。这里有太多的错误。

*   到处都是缩进——你不知道函数在哪里结束，或者`if/else`块在哪里开始(是的，这里有一个 if/else 块！)
*   大括号令人困惑，而且不一致
*   行距不一致

这是一个有点夸张的例子，但是它展示了使用一致的缩进和格式的好处。我不知道你怎么想，但是“好”的例子对我来说更好看！

好消息是，有许多 IDE 插件可以用来自动格式化代码。哈利路亚！

*   VS 代码:[更漂亮](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
*   原子:[原子美化](https://atom.io/packages/atom-beautify)
*   崇高的文字:[美化](https://packagecontrol.io/packages/HTML-CSS-JS%20Prettify)

### 使用清晰的变量名和方法名

在开始，我谈到了你的代码易于阅读的重要性。这其中很大一个方面是你选择的命名(这是我作为初级开发人员时犯的[错误之一](https://www.chrisblakely.dev/7-mistakes-i-made-as-a-junior-developer/))。让我们看一个好的命名的例子:

```
function changeStudentLabelText(studentId){                  
     const studentNameLabel = getStudentName(studentId); 
}
function getStudentName(studentId){ 
     const student = api.getStudentById(studentId); 
     return student.name; 
}
```

这段代码片段在很多方面都很有用:

*   这些函数的名字很清楚，参数也很好。当开发人员读到这里时，他们心里很清楚，“如果我用一个`studentId`调用`getStudentName()`方法，我将得到一个学生名”——如果他们不需要，他们不必导航到`getStudentName()`方法！
*   在`getStudentName()`方法中，变量和方法调用再次被清楚地命名——很容易看到方法调用了一个`api`,得到了一个`student`对象，并返回了`name`属性。轻松点。

为初学者编写干净的代码时，选择好的名字比你想象的要难。随着应用程序的增长，使用这些约定来确保代码易于阅读:

*   选择一种命名风格并保持一致。`camelCase`或`under_scores`中的一个，但不是两个！
*   给你的函数、方法和变量起名字，用它做什么，或者它是什么。如果你的方法*得到了*什么东西，就把`get`放在名字里。例如，如果你的变量*存储了*一辆汽车的颜色，就称它为`carColour`。

**额外提示**——如果你不能说出你的函数或方法，那么这个函数做得太多了。继续把它分解成更小的函数！例如，如果你最终调用了你的函数`updateCarAndSave()`，创建两个方法`updateCar()`和`saveCar()`。

### 必要时使用注释

有一种说法是，“代码应该是自文档化的”，这基本上意味着，代替使用注释，你的代码应该读得足够好，减少对注释的需求。这是一个有效的观点，我想这在一个完美的世界里是有意义的。然而，编码的世界远非完美，所以有时注释是必要的。

文档注释是描述特定函数或类功能的注释。如果你正在写一个库，这将对使用你的库的开发者有所帮助。这里有一个来自 useJSDoc 的例子:

```
/** * Solves equations of the form a * x = b 
* @example * 
// returns 2 * globalNS.method1(5, 10); 
* @example * 
// returns 3 * globalNS.method(5, 15); 
* @returns {Number} Returns the value of x for the equation. */ globalNS.method1 = function (a, b) { return b / a; };
```

澄清注释适用于任何可能需要维护、重构或扩展代码的人(包括未来的自己)。通常情况下，澄清注释可以被避免，以支持“自文档化代码”。下面是一个澄清注释的示例:

```
/* This function calls a third party API. Due to some issue with the API vender, the response returns "BAD REQUEST" at times. If it does, we need to retry */ 
function getImageLinks(){ 
     const imageLinks = makeApiCall(); 
     if(imageLinks === null){ 
        retryApiCall(); 
     } else { 
        doSomeOtherStuff(); 
     } 
}
```

这里有一些你应该尽量避免的评论。它们没有提供太多的价值，可能会误导人，而且只会使代码变得混乱。

没有附加值的多余评论:

```
// this sets the students age 
function setStudentAge();
```

误导性评论:

```
//this sets the fullname of the student 
function setLastName();
```

有趣或侮辱性的评论:

```
// this method is 5000 lines long but it's impossible to refactor so don't try 
function reallyLongFunction();
```

### 记住干的原则(不要重复自己)

干燥原理陈述如下:

> "每一项知识都必须在系统中有一个单一的、明确的、权威的表示."

最简单地说，这基本上意味着您应该致力于减少存在的重复代码的数量。(注意，我说的是“*减少”*，而不是*“消除”——*在某些情况下，重复代码并不是世界末日！)

维护和修改重复的代码可能是一场噩梦。让我们看一个例子:

```
function addEmployee(){ 
    // create the user object and give the role
    const user = {
        firstName: 'Rory',
        lastName: 'Millar',
        role: 'Admin'
    }

    // add the new user to the database - and log out the response or error
    axios.post('/user', user)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
}

function addManager(){  
    // create the user object and give the role
    const user = {
        firstName: 'James',
        lastName: 'Marley',
        role: 'Admin'
    }
    // add the new user to the database - and log out the response or error
    axios.post('/user', user)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
}

function addAdmin(){    
    // create the user object and give the role
    const user = {
        firstName: 'Gary',
        lastName: 'Judge',
        role: 'Admin'
    }

    // add the new user to the database - and log out the response or error
    axios.post('/user', user)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
}
```

假设您正在为客户创建一个人力资源 web 应用程序。该应用程序允许管理员通过 API 向数据库添加用户角色。有 3 个角色；员工、经理和管理员。让我们看看可能存在的一些函数:

酷！代码运行正常，一切正常。过了一会儿，我们的客户过来说:

> *嘿！我们希望显示的错误消息包含句子“有一个错误”。另外，更烦人的是，我们想将 API 端点从`/user`改为`/users`。谢谢！*

所以在我们开始编码之前，让我们先退后一步。还记得在这篇为初学者编写干净代码的文章的开头，我说过“干净的代码应该是有重点的”。即做一件事并把它做好？这就是我们当前代码的一个小问题。进行 API 调用和处理错误的代码是重复的——这意味着我们必须在 3 个地方更改代码以满足新的需求。烦人！

那么，如果我们把这个重构为*更专注*呢？请看以下内容:

```
function addEmployee(){ 
    // create the user object and give the role
    const user = {
        firstName: 'Rory',
        lastName: 'Millar',
        role: 'Admin'
    }

    // add the new user to the database - and log out the response or error
    saveUserToDatabase(user);
}

function addManager(){  
    // create the user object and give the role
    const user = {
        firstName: 'James',
        lastName: 'Marley',
        role: 'Admin'
    }
    // add the new user to the database - and log out the response or error
    saveUserToDatabase(user);
}

function addAdmin(){    
    // create the user object and give the role
    const user = {
        firstName: 'Gary',
        lastName: 'Judge',
        role: 'Admin'
    }

    // add the new user to the database - and log out the response or error
    saveUserToDatabase(user);
}

function saveUserToDatabase(user){
    axios.post('/users', user)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log("there was an error " + error);
  });
}
```

我们已经将创建 API 调用的逻辑移到了它自己的方法`saveUserToDatabase(user)`(这是一个好名字吗？你决定！)其他方法*将调用*来保存用户。现在，如果我们需要再次更改 API 逻辑，我们只需更新 1 个方法。同样，如果我们必须添加另一个创建用户的方法，那么通过 api 将用户保存到数据库的方法已经存在。万岁！

### 一个利用我们目前所学进行重构的例子

让我们闭上眼睛，假装我们正在做一个计算器应用程序。有些函数允许我们分别进行加、减、乘和除。结果被输出到控制台。

这是我们目前掌握的情况。在继续之前，看看你是否能发现问题:

```
function addNumbers(number1, number2)
{
    const result = number1 + number2;
        const output = 'The result is ' + result;
        console.log(output);
}

// this function substracts 2 numbers
function substractNumbers(number1, number2){

    //store the result in a variable called result
    const result = number1 - number2;
    const output = 'The result is ' + result;
    console.log(output);
}

function doStuffWithNumbers(number1, number2){
    const result = number1 * number2;
    const output = 'The result is ' + result;
    console.log(output);
}

function divideNumbers(x, y){
    const result = number1 / number2;
    const output = 'The result is ' + result;
    console.log(output);
}
```

有哪些问题？

*   缩进是不一致的——我们使用什么样的缩进格式并不重要，只要它是一致的就行
*   第二个函数有一些多余的注释——我们可以通过读取函数名和函数中的代码来判断发生了什么，所以我们真的需要注释吗？
*   第三个和第四个函数没有使用好的命名— `doStuffWithNumbers()`不是最好的函数名，因为它没有说明它是做什么的。`(x, y)`也不是描述性变量——`x & y`是函数吗？数字？香蕉？
*   方法*不止做一件事——*执行计算，还显示输出。我们可以将*显示*逻辑拆分成一个单独的方法——按照**干原理**

现在，我们将使用我们在这本初学者简明代码指南中学到的知识来重构一切，这样我们的新代码看起来就像:

```
function addNumbers(number1, number2){
	const result = number1 + number2;
	displayOutput(result)
}

function substractNumbers(number1, number2){
	const result = number1 - number2;
	displayOutput(result)
}

function multiplyNumbers(number1, number2){
	const result = number1 * number2;
	displayOutput(result)
}

function divideNumbers(number1, number2){
	const result = number1 * number2;
	displayOutput(result)
}

function displayOutput(result){
	const output = 'The result is ' + result;
	console.log(output);
}
```

*   我们已经修复了缩进，使其保持一致
*   调整了函数和变量的命名
*   删除了不需要的注释
*   将`displayOutput()`逻辑移到它自己的方法中——如果输出需要改变，我们只需要改变一个地方

恭喜你。你现在可以在面试中以及在[撰写你的杀手简历](https://www.chrisblakely.dev/how-to-write-an-awesome-junior-developer-resume-in-a-few-simple-steps/)时谈论你是如何知道干净代码原则的！

### 不要“过度清理”你的代码

我经常看到开发人员在清理代码时走得太远。注意不要试图过多地清理你的代码，因为这可能会产生相反的效果，并且实际上使你的代码更难阅读和维护。如果开发人员为了做一个简单的改变而不得不经常在许多文件/方法之间跳转，这也会对生产力产生影响。

注意干净的代码，但是不要在项目的早期过多考虑它。确保你的代码能够工作，并且经过了充分的测试。在重构阶段，你应该真正考虑如何使用 DRY 原则清理你的代码。

在这本简明的初学者代码指南中，我们学习了如何:

*   使用一致的格式和缩进
*   使用清晰的变量名和方法名
*   必要时使用注释
*   使用干燥原则(不要重复自己)

如果你喜欢这个指南，一定要看看罗伯特·C·马丁写的 [*干净的代码:敏捷软件工艺手册*。如果你对编写干净的代码和突破初级开发人员的水平很认真，我强烈推荐这本书。](https://amzn.to/2U7JO4N)

感谢阅读！

要让初级开发人员直接获得最新的指南和课程，请务必加入邮件列表，网址为 [www.chrisblakely.dev](https://www.chrisblakely.dev/#sign-up) ！