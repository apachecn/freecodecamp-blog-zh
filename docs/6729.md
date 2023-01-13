# 当“这个”失去上下文时该怎么办

> 原文：<https://www.freecodecamp.org/news/what-to-do-when-this-loses-context-f09664af076f/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

避免`this`丢失上下文的最好方法是根本不使用`this`。然而，这并不总是一种选择。我们可能继承了使用`this`的代码，或者我们可能与使用`this`的库一起工作。

对象文字、构造函数和`class` es 在原型系统上构建对象。原型系统使用`this`伪参数让函数访问其他对象属性。

我们来看看一些情况。

### 嵌套函数

`this`丢失嵌套函数中的上下文。[考虑下面的代码](https://jsfiddle.net/cristi_salcescu/n7zh5gvw/):

```
class Service {
  constructor(){
    this.numbers = [1,2,3];
    this.token = "token";
  }

  doSomething(){
    setTimeout(function doAnotherThing(){
      this.numbers.forEach(function log(number){
      //Cannot read property 'forEach' of undefined
          console.log(number);
          console.log(this.token);
      });
    }, 100);
  }
}

let service = new Service();
service.doSomething();
```

`doSomething()`方法有两个嵌套函数:`doAnotherThing()`和`log()`。当`service.doSomething()`被调用时，`this`在嵌套函数中失去上下文。

#### 绑定()

解决这个问题的一个方法是使用`bind()`。[看下一个代码](https://jsfiddle.net/cristi_salcescu/2r4ncoum/):

```
doSomething(){
   setTimeout(function doAnotherThing(){
      this.numbers.forEach(function log(number){
         console.log(number);
         console.log(this.token);
      }.bind(this));
    }.bind(this), 100);
  }
```

`bind()`创建一个新版本的函数，该函数在被调用时已经设置了`this`值。注意，我们需要对每个嵌套函数使用`.bind(this)`。

`function doAnotherThing(){ /*…*/}.bind(this)`创建一个版本的`doAnotherThing()`，它从`doSomething()`获取`this`值。

#### 那个/自己

另一种选择是声明并使用一个新变量`that/self`，它存储来自`doSomething()`方法的`this`的值。

[参见下面的代码](https://jsfiddle.net/cristi_salcescu/6ajx1hbp/):

```
doSomething(){
   let that = this;
   setTimeout(function doAnotherThing(){
      that.numbers.forEach(function log(number){
         console.log(number);
         console.log(that.token);
      });
    }, 100);
  }
```

我们需要在嵌套函数中使用`this`的所有方法中声明`let that = this`。

#### 箭头功能

arrow 函数提供了解决这个问题的另一种方法。[下面是代码](https://jsfiddle.net/cristi_salcescu/ejdb19su/):

```
doSomething(){
   setTimeout(() => {
     this.numbers.forEach(number => {
         console.log(number);
         console.log(this.token);
      });
    }, 100);
  }
```

箭头功能没有自己的`this`。它从其父节点获取`this`值。这种修复的唯一问题是我们容易丢失函数名。函数名很重要，因为它通过表达函数意图来提高可读性。

[下面是同样的代码](https://jsfiddle.net/cristi_salcescu/by096fza/)，用函数推断变量名:

```
doSomething(){    
   let log = number => {
     console.log(number);
     console.log(this.token);
   }

   let doAnotherThing = () => {
     this.numbers.forEach(log);
   }

   setTimeout(doAnotherThing, 100);
}
```

方法作为回调

当方法被用作回调时，`this`丢失上下文。

[考虑下一个类](https://jsfiddle.net/cristi_salcescu/f3t2vmex/):

```
class Service {
  constructor(){
    this.token = "token"; 
  }

  doSomething(){
    console.log(this.token);//undefined
  } 
}
let service = new Service();
```

现在，让我们看看方法`service.doSomething()`被用作回调的一些情况。

```
//callback on DOM event
$("#btn").click(service.doSomething);

//callback for timer
setTimeout(service.doSomething, 0);

//callback for custom function
run(service.doSomething);

function run(fn){
  fn();
}
```

在所有以前的情况下`this`失去上下文。

#### 绑定()

我们可以使用`bind()`来修复这个问题。[查看下一段代码片段](https://jsfiddle.net/cristi_salcescu/1904jbh8/):

```
//callback on DOM event
$("#btn").click(service.doSomething.bind(service));

//callback for timer
setTimeout(service.doSomething.bind(service), 0);

//callback for custom function
run(service.doSomething.bind(service));
```

#### 箭头功能

另一个选择是创建一个调用`service.doSomething()`的新函数。

```
//callback on DOM event
$("#btn").click(() => service.doSomething());

//callback for timer
setTimeout(() => service.doSomething(), 0);

//callback for custom function
run(() => service.doSomething());
```

### 反应组分

在 React 组件中，当方法被用作 UI 事件的回调时，`this`会丢失上下文。

[考虑以下组件](https://jsfiddle.net/cristi_salcescu/q59zjx1p/):

```
class TodoAddForm extends React.Component {
  constructor(){
      super();
      this.todos = [];
  }

  componentWillMount() {
    this.setState({desc: ""});
  }

  add(){
    let todo = {desc: this.state.desc}; 
    //Cannot read property 'state' of undefined
    this.todos.push(todo);
  }

  handleChange(event) {
     //Cannot read property 'setState' of undefined
     this.setState({desc: event.target.value});
  }

  render() {
    return <form>
      <input onChange={this.handleChange} value={this.state.desc} type="text"/>
      <button onClick={this.add} type="button">Save</button>
    </form>;
  }
}

ReactDOM.render(
  <TodoAddForm />,
  document.getElementById('root'));
```

解决这个问题的一个方法是使用`bind(this)`在构造函数中创建新函数。

```
constructor(){
   super();
   this.todos = [];
   this.handleChange = this.handleChange.bind(this);
   this.add = this.add.bind(this);
}
```

### 不使用"`this"`

没有`this`，没有丢失上下文的问题。可以使用工厂函数创建对象。[查看此代码](https://jsfiddle.net/cristi_salcescu/xvsk6twc/):

```
function Service() {  
  let numbers = [1,2,3];
  let token = "token";

  function doSomething(){
   setTimeout(function doAnotherThing(){
     numbers.forEach(function log(number){
        console.log(number);
        console.log(token);
      });
    }, 100);
  }

  return Object.freeze({
    doSomething
  });
}
```

这一次，当该方法用作回调时，上下文不会丢失。

```
 let service = Service();
service.doSomething();

//callback on DOM event
$("#btn").click(service.doSomething);

//callback for timer
setTimeout(service.doSomething, 0);

//callback for custom function
run(service.doSomething);
```

### 结论

`this`在不同的情况下会失去上下文。

that/self 模式和 arrow 函数是我们用来解决上下文问题的工具。

工厂函数提供了完全不使用`this`创建对象的选项。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)