# 如何在 React 中使用适配器设计模式

> 原文：<https://www.freecodecamp.org/news/adapter-design-in-react/>

当您在 React 或任何其他工具中编码时，您可能希望或需要使用第三方库。让我们讨论一种方法，它将确保您使用的第三方库能够很好地与您的应用程序融合。

作为应用程序开发人员，我们没有必要每次开始一个新项目都重新发明轮子。在大多数情况下，我们将使用第三方库，该库提供我们正在寻找的功能的稳定版本。

## **将库视为插件**

在软件开发中，当您使用第三方库时，您必须考虑以下几点:

*   应用程序应该是与库无关的。将来，你可能会决定使用不同的库。这样做不会损坏任何东西。
*   **保证数据模型的一致性。**应用程序的数据模型很可能与库的数据模型不兼容。在这种情况下，需要进行一些数据转换。
*   确保最小的依赖性。应用程序可能不需要使用库提供的所有功能。您应该只使用您需要的功能。

从本质上讲，所有这些都表明你不应该过度依赖这个库。你应该把库当作插件，在需要的时候可以很容易地附加或分离。让我们来谈谈如何做到这一点。

## **React 中的适配器模式**

确保解决上述所有问题的一种方法是使用适配器模式。

> 适配器模式将一个类的接口转换成客户期望的另一个接口。适配器允许类一起工作，否则由于不兼容的接口而无法一起工作。([来源](https://www.geeksforgeeks.org/adapter-pattern/))

为了在 React 中应用这一点，我们必须引入一个第三方库的包装器。这个包装器将作为适配器，确保应用程序始终拥有对我们打算包装的功能的稳定引用。

在 React 中，有两种包装器类型:

1.  **组件包装器**–包装库组件
2.  **函数包装器**–包装库函数

在本文中，我们将更多地关注组件包装器。让我们看一个例子。

## **运行中的组件包装器**

对于我们的例子，我们将为第三方图表库 [React Flow](https://reactflow.dev/) 创建一个适配器。

React 流库公开了许多功能，但是对于我们的示例，我们只需要执行以下操作:

1.  呈现基本图节点
2.  当选择一个节点时作出反应
3.  当不再有任何选择时做出反应

为此，我们将首先实现`Diagram Adapter`:

```
import ReactFlow, { isNode } from "react-flow-renderer";

const DiagramAdapter = ({ nodes, onActivateNode, onDeactivateAll }) => {
    const onSelectionChange = (elements) => {
        if (elements) {
            const selectedNodes = elements.filter((els) => isNode(els));

            if (selectedNodes.length > 0) {
                onActivateNode(selectedNodes[0].id);
            }
        }
    };

    const onPaneClick = () => onDeactivateAll();

    return (
        <div style={{ height: 650 }}>
            <ReactFlow
                elements={nodes}
                onSelectionChange={onSelectionChange}
                onPaneClick={onPaneClick} />
        </div>
    );
}

export default DiagramAdapter; 
```

The DiagramAdapter Component

在上面的代码中，我们包装了`ReactFlow`组件，并为它附加了一些事件监听器。然后，这些事件监听器将转换事件数据，并调用适配器的父组件传递的相应的`onActivateNode`和`onDeactivateAll`函数。

这样，父组件甚至不需要知道我们正在使用什么库。它只知道`onActivateNode`和`onDeactivateAll`可供使用。

作为参考，我们可以这样使用适配器:

```
function App() {
  const nodes = [
    {
      id: "node_0",
      position: { x: 150, y: 25 },
      data: { label: "Start" }
    },
    {
      id: "node_1",
      position: { x: 150, y: 225 },
      data: { label: "End" }
    },
    {
      id: "node_0-node_1", type: "step", source: "node_0", target: "node_1"
    }
  ];

  const onActivateNode = (node) => {
    console.log("Activated", node);
  };

  const onDeactivateAll = (node) => {
    console.log("Deactivated all");
  };

  return (
    <DiagramAdapter 
        nodes={nodes}
        onActivateNode={onActivateNode}
        onDeactivateAll={onDeactivateAll} />
  );
} 
```

The App Component

### 更现实的例子

更真实的例子，你可以在这里查看我的一个学习项目。这是一个使用 React 和 ReactFlow 创建的简单的低代码应用程序生成器。

适配器代码可在`/src/Editor/DiagramAdapter.js`找到。而父组件可以在`src/Editor/Canvas.js`找到。

## **结论**

恭喜你！我们已经在 React 应用程序上成功地使用了适配器设计模式。

我们现在可以享受应用程序与第三方库分离的好处了:

*   与库无关的应用程序
*   数据模型一致性
*   最小依赖性

我希望你今天从我这里学到了新的东西！如果您有任何其他方法可以在 React 应用程序上应用适配器设计或任何其他类似的设计模式，请告诉我。期待收到你的来信。