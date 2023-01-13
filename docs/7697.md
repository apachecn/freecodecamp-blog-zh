# 使用状态机增强您的反应

> 原文：<https://www.freecodecamp.org/news/boost-your-react-with-state-machines-1e9641b0aa43/>

混合 React 和状态机对于开发人员来说是极大的生产力提升。它还改善了通常不稳定的开发人员/设计人员协作。

状态机的概念非常简单:一个组件一次可以处于一种状态，并且有有限的状态数。

你说这对 UI 开发有什么帮助？

### 问题是

让我们考虑一个简单的文本编辑组件，如下所示:

![1*qH9LyaKS94HYKOfvhR1jGw](img/1c727f1e7379831b77e87821f1dd577b.png)

这种组件的可能“状态”是(从左到右):

*   显示值
*   编辑值
*   正在保存
*   保存错误

组件模型的简单形状有 5 个属性:

```
state: {
  processing: true, // Will be true when saving is in progress
  error: null,      // Will be not null when a save error occurs
  value: null,      // The read only display value
  edition: false,   // Are we in edit mode?
  editValue: null,  // The currently edited but not saved value
}
```

这些性质的适当组合将给出我们上面已经确定的 4 种状态。

问题是，实际上这个州有 2⁵ = 32 种可能的组合。这意味着有 28 种错误的方式来使用状态属性。

上述组件的一个典型错误是在成功保存后没有重置错误。因此，最终用户将保存，得到一个`“Something went wrong”`错误消息，纠正错误，再次保存并进入显示模式。到目前为止一切顺利。除了再次进入编辑模式时…错误信息仍然存在。真实的故事。我见过几次没有经验的开发人员这样做。

我们的组件非常简单，但它表明了一个可悲的事实:

对原始状态属性的操作意味着组件的健壮性只依赖于属性的正确使用，这意味着…对于每个修改代码的开发人员…在整个项目生命周期中。

我们都知道结局是什么！

### 解决方案

考虑使用“状态机”的不同方法。这些状态是:

```
state: {
  display: {
    processing: false,
    error: null,
    value: “Awesome”,
    edition: false,
    editValue: null,
  },
  saving: {
    processing: true,
    error: null,
    value: “Awesome”,
    edition: true, // Keep the edit view active until save is finished
    editValue: “Awesome Edit”, 
  },
  edit: {
    processing: false,
    error: null,
    value: “Awesome”,
    edition: true,
    editValue: “Awesome Editing”,
  },
  save_error: {
    processing: false,
    error: “Value should be at least 4 characters”,
    value: “Awesome”,
    edition: true, // Keep the edit box open
    editValue: “Awe”,
  }
}
```

这比第一种方法更冗长，但它提供了许多好处:

*   只需查看状态机，就可以看到组件的所有状态。状态有逻辑名称，每个原始属性的用法都有自己的文档。团队中的新开发人员会马上有宾至如归的感觉。
*   如何扩展组件的约定很清楚:创建一个新的状态并适当地设置原始属性。当组件中实现了状态机时，没有人敢使用 raw `setState()`。
*   最后但同样重要的是，与 UI/UX 团队的交接过程变得尽可能顺利。你需要为你的机器的每一个状态设计一个可视化的设计，可能还需要一些动画来过渡。就是这样。清晰且易于追踪。

上面例子的一个最简单的工作版本是:

```
import React, {Component, PropTypes} from 'react';

export default class InputStateMachine extends Component {
    constructor(props) {
        super(props);

        this.handleSubmit = this.handleSubmit.bind(this);
        this.goToState = this.goToState.bind(this);
        this.save = this.save.bind(this);

        this.state = {
            name: 'display',
            machine: this.generateState('display', props.initialValue)
        };
    }

    generateState(stateName, stateParam) {

        const previousState = this.state ? {...this.state.machine} : {};

        switch(stateName) {
            case 'display':
                return {
                    processing: false,
                    error: null,
                    value: stateParam || previousState.value,
                    editing: false,
                    editValue: null,
                };
            case 'saving':
                return {
                    processing: true,
                    error: null, // Reset any previous error
                    value: previousState.value,
                    editing: true, // Keep the edit view active until save is finished
                    editValue: previousState.editValue,
                };
            case 'edit':
                return {
                    processing: false,
                    error: null,
                    value: previousState.value,
                    editing: true,
                    editValue: stateParam,
                };
            case 'save_error':
                return {
                    processing: false,
                    error: stateParam,
                    value: previousState.value,
                    editing: true, // Keep the edit box open
                    editValue: previousState.editValue,
                };
            case 'loading': // Same as default
            default:
                return {
                    processing: true,
                    error: null,
                    value: null,
                    editing: false,
                    editValue: null,
                };
        }
    }

    goToState(stateName, stateParam)  {
        this.setState({
            name: stateName,
            machine: this.generateState(stateName, stateParam)
        });
    }

    handleSubmit(e) {
        this.goToState('edit', e.target.value);
    };

    save(valueToSave) {
        this.goToState('saving');

        // Simulate saving the data ...
        setTimeout(() => this.goToState('display', valueToSave), 2000);
    };

    render() {
        const {processing, error, value, editing, editValue} = this.state.machine;

        if(processing) {
            return <p>Processing ...</p>
        } else if(editing) {
            return (
                <div>
                    <input type="text" onChange={this.handleSubmit} value={editValue || value} />
                    {error && <p>Error: {error}</p>}
                    <button onClick={() => this.save(editValue)}>Save</button>
                </div>
            );
        } else {
            return (
                <div>
                    <p>{value}</p>
                    <button onClick={() => this.goToState('edit', value)}>Edit</button>
                </div>
            );
        }
    }
}
```

用法是:

```
<InputStateMachine initialValue="Hello" /> 
```

使用状态机时，需要编写一些样板代码:

*   创建一个设置状态名称和内容的实用方法。跟踪当前的状态名称，以便于调试。
*   保持生成状态的方法的纯洁性，并使用它在构造函数中初始化状态
*   在你的渲染方法中，析构`this.state.machine`而不是`this.state`
*   状态可能需要难以处理的参数。根据经验，如果您的状态生成需要 3 个以上的参数，那么您的组件就不应该使用状态机模式

一些库旨在解决这个样板文件问题，但是开销很小，以至于它不值得对您的项目产生新的依赖。

### 结论

状态机模式是改善 UI 组件可读性和从可视化设计到维护的开发过程的好方法。

不过要小心！不要孤注一掷，将此应用于您拥有的所有组件！你的应用需要保持灵活性，并处理突发的复杂性。对于更高级别的组件，状态的数量可能会迅速增加，在这种情况下，状态机没有任何好处。

但是一定要在你的标准/基本组件库上使用这个模式！这是应用程序中存在时间最长的部分。最终，团队中的每个开发人员都会接触到它，并从状态机提供的指导和健壮性中受益。

感谢阅读！