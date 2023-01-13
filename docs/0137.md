# 如何在 React 中使用 array.map()析构对象属性

> 原文：<https://www.freecodecamp.org/news/destructure-object-properties-using-array-map-in-react/>

前端开发人员在 JavaScript 中使用最多的方法之一是`Array.prototype.map()`方法。

从必须在 DOM 中呈现一个项目列表到循环浏览一系列博客文章——以及更多——有用性一直在增加。

假设您在一个数组中有一个项目列表，需要作为 React 组件呈现在 web 页面上。映射数组中一系列项目的理想方式如下所示:

```
const shoppingList = ['Oranges', 'Cassava', 'Garri', 'Ewa', 'Dodo', 'Books']

export default function List() {
  return (
    <>
      {shoppingList.map((item, index) => {
        return (
          <ol>
            <li key={index}>{item}</li>
          </ol>
        )
      })}
    </>
  )
} 
```

上面的片段很好地实现了它的目的。但是，如果您必须映射具有多个属性的对象数组，该怎么办呢？比方说，一个像下面这样的数组？

```
const employees = [
  {
    name: 'Saka Manje',
    address: '14, cassava-garri-ewa street',
    gender: 'Male',
  },
  {
    name: 'Wawawa Warisii',
    address: '406, highway street',
    gender: 'Male',
  },
]
```

为了简洁起见，让我们坚持使用数组中的两个项目。现在，让我们使用与上一个片段中相同的方法。

```
export default function EmployeesList() {
  return (
    <>
      {employees.map((employee, index) => {
        return (
          <div key={index}>
            <p>{employee.name}</p>
            <p>{employee.address}</p>
            <p>{employee.gender}</p>
          </div>
        )
      })}
    </>
  )
}
```

虽然上面代码片段中的方法看起来非常好，但是您可能会想——“当我从一个端点获取深度嵌套的对象作为数据时会发生什么？”

## 如何析构对象属性

在上一节中，我们介绍了使用 JavaScript 的`.map()`方法在网页上从 API 端点呈现数据的传统方式。

在这一节中，我们将看看如何在不使用点符号访问数组中的属性的情况下获得相同的结果。

但是，在我们这么做之前，析构对象属性到底意味着什么？析构的目的是能够访问数组或对象中的变量，然后将它们赋给一个变量。你可以在下面看到一个例子。

```
const person = {
  name: 'Adrian Tojubole',
  role: 'Lead Engineer',
  salary: '$130k/year',
}

let { name, role, salary } = person

console.log(name); // Adrian Tojubole
console.log(role); // Lead Engineer
console.log(salary); // $130k/year 
```

您会注意到我们能够访问`person`对象的属性—`name`、`role`和`salary`。这取代了使用点符号来访问它们，就像下面的代码片段，这使得我们重复了这个过程。

```
console.log(person.name);
console.log(person.role);
console.log(person.salary);
```

这样一来，我们将采用这种模式，每当我们想在 React 中使用`.map()`方法时就使用它。以下面的对象数组为例:

```
const employeesData = [
  {
    name: 'Saka manje',
    address: '14, cassava-garri-ewa street',
    attributes: {
      height: '6ft',
      hairColor: 'Brown',
      eye: 'Black',
    },
    gender: 'Male',
  },
  {
    name: 'Adrian Toromagbe',
    address: '14, kogbagidi street',
    attributes: {
      height: '5ft',
      hairColor: 'Black',
      eye: 'Red',
    },
    gender: 'Male',
  },
]
```

在上面的代码片段中，您会注意到我们将一个对象作为属性嵌套在另一个对象中。现在，访问嵌套对象中的属性的初始方式如下所示:

```
employeesData.map(data => data.attributes.height);
```

但是，当您析构该对象中的属性时，语法看起来有点像这样:

```
employeesData.map(
  ({ name, address, attributes: { height, hairColor, eye }, gender }, index) => {
    return name, address, height, hairColor, eye
  }
)
```

上面的代码片段消除了这样做的过程:`employee.name`、`employee.attributes.height`等等。

现在，您已经知道它是如何工作的了，是时候将这个`.map()`放入 React 组件中，然后返回相应的属性。

```
export default function Employees() {
  return (
    <div>
      {employeesData.map(
        (
          { name, address, gender, attributes: { height, hairColor, eye } },
          index
        ) => {
          return (
            <div className="employees" key={index}>
              <p>{name}</p>
              <p>{address}</p>
              <p>{gender}</p>
              <p>{height}</p>
              <p>{eye}</p>
              <p>{hairColor}</p>
            </div>
          )
        }
      )}
    </div>
  )
} 
```

## 包扎

使用这种方法，您可以节省大量使用点符号访问对象属性的时间。随着时间的推移，这是很有用的，因为您可能会开始与 GraphQL APIs 进行交互，其中一些 API 通常具有作为数据返回的深度嵌套的对象。

你也可以析构数组属性。一个很好的例子是当我们使用一个流行的 React 钩子`useState`时，我们析构一个`value`和回调函数`setValue`的方式

```
const [count, setCount] = React.useState(0)
```

感谢您阅读本教程。希望对你有帮助。