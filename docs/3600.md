# React.js 针对初学者——道具和状态解释

> 原文：<https://www.freecodecamp.org/news/react-js-for-beginners-props-state-explained/>

React.js 是每个前端开发者都应该知道的使用最广泛的 JavaScript 库之一。理解什么是道具和状态以及它们之间的区别是学习 React 的一大步。

在这篇博文中，我将解释什么是道具和状态，我还将澄清一些关于它们的最常见的问题:

*   什么是道具？
*   你是怎么用道具传递数据的？
*   什么是状态？
*   如何更新组件的状态？
*   当状态改变时会发生什么？
*   我可以在每个组件中使用状态吗？
*   道具和状态有什么区别？

> 如果你是一个 React 的初学者，我有一个关于初学者的教程系列。

## 什么是道具？

Props 是 properties 的缩写，用于在 React 组件之间传递数据。React 的组件之间的数据流是单向的(仅从父组件到子组件)。

### 你是怎么用道具传递数据的？

下面是一个如何使用 props 传递数据的示例:

```
class ParentComponent extends Component {    
    render() {    
        return (        
            <ChildComponent name="First Child" />    
        );  
    }
}

const ChildComponent = (props) => {    
    return <p>{props.name}</p>; 
};
```

首先，我们需要从父组件定义/获取一些数据，并将其分配给子组件的“prop”属性。

```
<ChildComponent name="First Child" /> 
```

“Name”在这里是一个已定义的属性，包含文本数据。然后我们可以用道具传递数据，就像我们给函数一个参数一样:

```
const ChildComponent = (props) => {  
  // statements
};
```

最后，我们使用点符号来访问属性数据并呈现它:

```
return <p>{props.name}</p>;
```

**你也可以看我的视频看看道具怎么用:**

[https://www.youtube.com/embed/KvapOdsFK5A?feature=oembed](https://www.youtube.com/embed/KvapOdsFK5A?feature=oembed)

## 什么是状态？

React 有另一个特殊的内置对象，称为 state，它允许组件创建和管理自己的数据。因此，与 props 不同，组件不能用状态传递数据，但它们可以在内部创建和管理数据。

下面是一个展示如何使用状态的示例:

```
class Test extends React.Component {    
    constructor() {    
        this.state = {      
            id: 1,      
            name: "test"    
        };  
    }    

    render() {    
        return (      
            <div>        
              <p>{this.state.id}</p>        
              <p>{this.state.name}</p>      
            </div>    
        );  
    }
}
```

### 如何更新组件的状态？

状态不应该直接修改，但是可以用一个叫做`setState( )`的特殊方法修改。

```
this.state.id = “2020”; // wrong

this.setState({         // correct  
    id: "2020"
});
```

### 当状态改变时会发生什么？

好了，为什么一定要用`setState( )`？为什么我们甚至需要状态对象本身？如果你在问这些问题，不要担心——你很快就会明白状态:)让我来回答。

状态的改变基于用户输入、触发事件等等而发生。此外，React 组件(带状态)基于状态中的数据进行渲染。状态保存初始信息。

因此，当状态改变时，React 得到通知并立即重新呈现 DOM—**不是整个 DOM，而只是具有更新状态的组件。**这也是 React 快的原因之一。

React 是如何得到通知的？你猜对了:用`setState( )`。`setState( )`方法触发更新部分的重新呈现过程。React 得到通知，知道要更改哪个(哪些)部分，并快速完成，而无需重新呈现整个 DOM。

总之，在使用 state 时，我们需要注意两点:

*   不应直接修改状态-应使用`setState( )`
*   状态会影响应用程序的性能，因此不应该不必要地使用它

### 我可以在每个组件中使用状态吗？

关于状态，你可能会问的另一个重要问题是，我们到底在哪里可以使用它。早期，state 只能用在**类组件**中，不能用在功能组件中。

这就是为什么功能组件也被称为无状态组件。然而，在引入了 **React Hooks** 之后，state 现在可以在类和函数组件中使用。

如果你的项目没有使用 React 钩子，那么你只能在类组件中使用状态。

### 道具和状态有什么区别？

最后，让我们回顾一下，看看道具和状态之间的主要区别:

*   组件用 props 从外部接收数据，而它们可以用 state 创建和管理自己的数据
*   属性用于传递数据，而状态用于管理数据
*   来自 props 的数据是只读的，不能被从外部接收数据的组件修改
*   状态数据可以由它自己的组件修改，但是是私有的(不能从外部访问)
*   道具只能从父组件传递到子组件(单向流动)
*   修改状态应该使用`setState ( )`方法

React.js 是当今使用最广泛的 JavaScript 库之一，每个前端开发人员都应该知道。

希望这篇文章对你理解道具和状态有所帮助。关于 React 还有其他一些重要的内容，我将在后面的文章中继续讨论。

**如果你想了解更多关于 web 开发的知识，欢迎在 YouTube 上关注我**[](https://www.youtube.com/channel/UC1EgYPCvKCXFn8HlpoJwY3Q)****！****

**感谢您的阅读！**