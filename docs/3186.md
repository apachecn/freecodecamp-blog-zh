# 遍历 React.js 中的嵌套对象

> 原文：<https://www.freecodecamp.org/news/iterate-through-nested-object-in-react-js/>

如果你曾经使用过 API，你会知道它们返回的数据结构会很快变得复杂。

假设您从 React 项目中调用一个 API，响应如下所示:

```
Object1 {
     Object2 {
           propertyIWantToAcess:
           anotherpropertyIWantToAcess:
      }
}
```

您已经将组件状态中的数据存储为`this.state.myPosts`，并且可以通过以下方式访问外部对象的元素:

```
render() {
    console.log(this.state.myPosts);

    const data = this.state.myPosts;

    const display = Object.keys(data).map((d, key) => {
    return (
      <div className="my-posts">
        <li key={key}>
          {data.current_route}
        </li>
      </div>
      );
    });

    return(
      <div>
        <ul>
          { display }
        </ul>
      </div>
    );
  }
```

但问题是，你无法访问任何内部对象。

内部对象的值总是变化的，所以您不能硬编码它们的键，并遍历它们来获得正确的值。

## 可能的解决方案

直接处理复杂的 API 响应可能很困难，所以让我们退一步简化一下:

```
const visit = (obj, fn) => {
    const values = Object.values(obj)

    values.forEach(val => 
        val && typeof val === "object" ? visit(val, fn) : fn(val))
}

// Quick test
const print = (val) => console.log(val)

const person = {
    name: {
        first: "John",
        last: "Doe"
    },
    age: 15,
    secret: {
        secret2: {
            secret3: {
                val: "I ate your cookie"
            }
        }
    }
}

visit(person, print)
/* Output
John
Doe
15
I ate your cookie
*/
```

`lodash`库有简单的方法来完成同样的事情，但是这是用普通 JS 做同样事情的一种快速而肮脏的方法。

但是假设您想进一步简化，比如:

```
render() {
    // Logs data
    console.log(this.state.myPosts);

    const data = this.state.myPosts;

    // Stores nested object I want to access in posts variable
    const posts = data.content;

    // Successfully logs nested object I want to access
    console.log(posts);

    // Error, this will not allow me to pass posts variable to Object.keys
    const display = Object.keys(posts).map(key =>
      <option value={key}>{posts[key]}</option>
    )

    return(
      <div>
        {display}
      </div>
    );
 }
```

但是当你试图将`posts`传递给`Object.keys`时，就会得到一个错误`TypeError: can't convert undefined to object error`。

请记住，这个错误与 React 无关。将一个对象作为组件的子对象传递是非法的。

仅返回作为参数传入的对象的键。您需要多次调用它来遍历所有嵌套的键。

如果需要显示整个嵌套对象，一种选择是使用函数将每个对象转换为 React 组件，并将其作为数组传递:

```
let data= []

visit(obj, (val) => {
    data.push(<p>{val}</p>)  // wraps any non-object type inside <p>
})
...
return <SomeComponent> {data} </SomeComponent>
```

## 有用的软件包

另一个选择是使用类似于 [json-query](https://www.npmjs.com/package/json-query) 的包来帮助遍历嵌套的 json 数据。

下面是上面使用`json-query`的`render`函数的修改版本:

```
 render() {
   const utopian = Object.keys(this.state.utopianCash);
   console.log(this.state.utopianCash);

   var author = jsonQuery('[*][author]', { data: this.state.utopianCash }).value
   var title = jsonQuery('[*][title]', { data: this.state.utopianCash }).value
   var payout = jsonQuery('[*][total_payout_value]', { data: this.state.utopianCash }).value
   var postLink = jsonQuery('[*][url]', { data: this.state.utopianCash }).value
   var pendingPayout = jsonQuery('[*][pending_payout_value]', { data: this.state.utopianCash }).value
   var netVotes = jsonQuery('[*][net_votes]', { data: this.state.utopianCash }).value

   let display = utopian.map((post, i) => {
     return (
       <div className="utopian-items">
        <p>
          <strong>Author: </strong>
          {author[i]}
        </p>
        <p>
          <strong>Title: </strong>
            <a href={`https://www.steemit.com` + postLink[i]}>{title[i]}</a>
        </p>
        <p>
          <strong>Pending Payout: </strong>
            {pendingPayout[i]}
        </p>
        <p>
          <strong>Votes: </strong>
          {netVotes[i]}
        </p>
       </div>
     );
   });

    return (
      <div className="utopian-container">
        {display}
        <User />
      </div>
    );
  }
}
```