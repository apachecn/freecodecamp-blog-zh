# 属性改变时重新渲染 React 组件

> 原文：<https://www.freecodecamp.org/news/re-render-react-component-when-its-props-changes/>

假设您有一个 React 和 Redux 项目，包含两个组件，一个父组件和一个子组件。

父组件传递一些道具给子组件。当子组件收到这些属性时，它应该调用一些 Redux 动作，这些动作改变了一些从父组件异步传递过来的属性。

这里有两个组件:

### 父组件

```
class Parent extends React.Component {
  getChilds(){
    let child = [];
    let i = 0;
    for (let keys in this.props.home.data) {
      child[i] = (
        <Child title={keys} data={this.props.home.data[keys]} key={keys} />
      );
      i++;
      if (i === 6) break;
    }

    return Rows;
  }
  render(){
return (
  <div>
     <h1>I am gonna call my child </h1>
    {this.getChilds()}
 </div>
)
```

### 子组件

```
class Child extends React.Component {
 componentDidMount(){
  if(this.props.data.items.length === 0){
    // calling an action to fill this.props.data.items array with data
   this.props.getData(this.props.data.id);
  }
 }
  getGrandSon(){
  let grandSons = [];
  if(this.props.data.items.length > 0){
   grandSons = this.props.data.items.map( item => <GrandSon item={item} />);
  }
  return grandSons;
 }
  render(){
    return (
      <div>
       <h1> I am the child component and I will call my own child </h1>
      {this.getGrandSon()}
    </div>
     )
 }
}
```

`redux-store`被正确更新，但是子组件没有重新渲染。

填充数据通常不是`Child`的责任。相反，它应该接收已经由`Parent`准备好的数据。还有，道具是自动更新的。

尽管如此，从一个`Child`组件更新和操作数据还是很有用的，尤其是在涉及 Redux 的时候。

React 可能执行浅层比较，即使状态明显改变，也可能不会重新呈现。

作为一种变通方法，您可以在`mapStateToProps`中这样做:

```
const mapStateToProps = state => {
  return { 
    id: state.data.id,
   items: state.data.items
  }
}
```