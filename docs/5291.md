# React 组件的“黄金法则”如何帮助您编写更好的代码

> 原文：<https://www.freecodecamp.org/news/how-the-golden-rule-of-react-components-can-help-you-write-better-code-127046b478eb/>

#### 以及钩子是如何发挥作用的

最近，我采用了一种新的理念，改变了我制作组件的方式。这不一定是一个新的想法，而是一种微妙的新的思维方式。

#### 组件的黄金法则

> 以最自然的方式创建和定义组件，只考虑它们需要什么功能。

同样，这是一个微妙的说法，你可能认为你已经遵循它，但很容易违背这一点。

例如，假设您有以下组件:

![1*nF_5kuYHigZuwdq99vRJ8g](img/72ba7d37eabcf314eca718509175b2a0.png)

PersonCard

如果您正在“自然地”定义这个组件，那么您可能会用下面的 API 来编写它:

```
PersonCard.propTypes = {
  name: PropTypes.string.isRequired,
  jobTitle: PropTypes.string.isRequired,
  pictureUrl: PropTypes.string.isRequired,
};
```

这非常简单——只看它需要什么功能，你只需要一个名字、职务和图片 URL。

但是，假设您有一个根据用户设置显示“官方”图片的要求。您可能想写一个这样的 API:

```
PersonCard.propTypes = {
  name: PropTypes.string.isRequired,
  jobTitle: PropTypes.string.isRequired,
  officialPictureUrl: PropTypes.string.isRequired,
  pictureUrl: PropTypes.string.isRequired,
  preferOfficial: PropTypes.boolean.isRequired,
};
```

看起来组件需要这些额外的道具来运行，但实际上，组件看起来没有任何不同，也不需要这些额外的道具来运行。这些额外的道具所做的是将这个`preferOfficial`设置与你的组件结合起来，并使组件在上下文之外的任何使用都感觉非常不自然。

### 弥合差距

那么如果切换图片 URL 的逻辑不属于组件本身，那么它属于哪里呢？

一个`index`文件怎么样？

我们采用了一种文件夹结构，其中每个组件都放在一个以自己命名的文件夹中，其中的`index`文件负责在您的“自然”组件和外部世界之间架起一座桥梁。我们称这个文件为“容器”(灵感来自 [React Redux 的“容器”组件概念](https://redux.js.org/basics/usage-with-react#presentational-and-container-components))。

```
/PersonCard
  -PersonCard.js ------ the "natural" component
  -index.js ----------- the "container"
```

我们将**容器**定义为一段代码，它在您的自然组件和外部世界之间架起了一座桥梁。为此，我们有时也称这些东西为“注射器”。

你的**自然组件**是如果只给你看一张要求你制作的图片，你会创建的代码(没有你如何获得数据的细节，或者它会被放在应用程序中的什么地方——你所知道的就是它应该能工作)。

**外部世界**是一个关键字，我们将使用它来指代您的应用程序拥有的任何资源(例如 Redux store ),这些资源可以被转换以满足您的自然组件的道具。

**本文目标:**我们怎样才能保持组件的“天然”而不被外界的垃圾污染？为什么这样更好？

> ***注:*** *尽管受到[丹的阿布拉莫夫](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)和 [React Redux 的](https://redux.js.org/basics/usage-with-react#presentational-and-container-components)术语的启发，我们对“容器”的定义还是略有不同。*

> 丹·阿布拉莫夫的集装箱和我们的唯一区别只是在概念上。Dan's 说有两种组件:表示组件和容器组件。我们进一步说，有组件，然后有容器。

> *即使我们用组件实现容器，我们也不认为容器是概念层面上的组件。这就是为什么我们建议把你的容器放在`index`文件中——因为它是你的自然组件和外部世界之间的桥梁，并不是独立的。*

虽然本文主要关注组件，但是容器占据了本文的大部分。

为什么？

制作天然组件——简单，甚至有趣。将您的组件连接到外部世界—有点困难。

在我看来，你用来自外界的垃圾污染你的自然组成部分有三个主要原因:

1.  奇怪的数据结构
2.  组件范围之外的需求(就像上面的例子)
3.  更新或装载时激发事件

接下来的几节将尝试用不同类型的容器实现的例子来涵盖这些情况。

### 使用奇怪的数据结构

有时为了呈现所需的信息，您需要将数据链接在一起，并将其转换成更合理的形式。因为没有更好的词，所以“怪异”的数据结构仅仅是不适合组件使用的数据结构。

将奇怪的数据结构直接传递到组件中并在组件内部进行转换是非常诱人的，但是这会导致混淆，并且通常很难测试组件。

最近，当我负责创建一个组件，从我们用来支持特定类型表单的特定数据结构中获取数据时，我就陷入了这个陷阱。

![1*hFOPWOxkedUEb851jdAXjA](img/22be716a197f541bbd6132af6857cafa.png)

The component itself

```
ChipField.propTypes = {
  field: PropTypes.object.isRequired,      // <-- the "weird" data structure
  onEditField: PropTypes.func.isRequired,  // <-- and a weird event too
};
```

组件接受了这个奇怪的数据结构作为道具。实际上，如果我们再也不用去碰它，这可能还不错，但是当我们被要求在与这个数据结构无关的地方再次使用它时，这就成了一个真正的问题。

因为组件需要这种数据结构，所以不可能重用它，重构起来也很混乱。我们最初编写的测试也令人困惑，因为它们嘲笑了这种奇怪的数据结构。我们很难理解这些测试，当我们最终重构时，也很难重写它们。

不幸的是，奇怪的数据结构是不可避免的，但是使用容器是处理它们的好方法。这里的一个要点是，以这种方式构建您的组件为您提供了提取组件并将其升级为可重用组件的*选项*。如果你把一个奇怪的数据结构传递给一个组件，你就失去了这个选择。

> ***注意:*** *我并不是建议你做的所有组件从一开始就要通用。建议是考虑你的组件在基础层面上做什么，然后弥补差距。因此，您更有可能拥有*选项*，以最少的工作将您的组件升级为可重用的组件。*

#### 使用功能组件实现容器

如果您严格地映射 props，一个简单的实现选项是使用另一个函数组件:

```
import React from 'react';
import PropTypes from 'prop-types';

import getValuesFromField from './helpers/getValuesFromField';
import transformValuesToField from './helpers/transformValuesToField';

import ChipField from './ChipField';

export default function ChipFieldContainer({ field, onEditField }) {
  const values = getValuesFromField(field);

  function handleOnChange(values) {
    onEditField(transformValuesToField(values));
  }

  return <ChipField values={values} onChange={handleOnChange} />;
}

// external props
ChipFieldContainer.propTypes = {
  field: PropTypes.object.isRequired,
  onEditField: PropTypes.func.isRequired,
};
```

像这样的组件的文件夹结构看起来像这样:

```
/ChipField
  -ChipField.js ------------------ the "natural" chip field
  -ChipField.test.js
  -index.js ---------------------- the "container"
  -index.test.js
  /helpers ----------------------- a folder for the helpers/utils
    -getValuesFromField.js
    -getValuesFromField.test.js
    -transformValuesToField.js
    -transformValuesToField.test.js
```

你可能会想“这工作太多了”——如果你是，那我明白了。看起来这里有更多的工作要做，因为有更多的文件和一点间接性，但是这里有你遗漏的部分:

```
import { connect } from 'react-redux';

import getPictureUrl from './helpers/getPictureUrl';

import PersonCard from './PersonCard';

const mapStateToProps = (state, ownProps) => {
  const { person } = ownProps;
  const { name, jobTitle, customPictureUrl, officialPictureUrl } = person;
  const { preferOfficial } = state.settings;

  const pictureUrl = getPictureUrl(preferOfficial, customPictureUrl, officialPictureUrl);

  return { name, jobTitle, pictureUrl };
};

const mapDispatchToProps = null;

export default connect(
  mapStateToProps,
  mapDispatchToProps,
)(PersonCard);
```

无论是在组件外部还是在组件内部转换数据，工作量都是一样的。不同之处在于，当您在组件之外转换数据时，您可以更明确地测试您的转换是否正确，同时还可以分离关注点。

### 满足组件范围之外的需求

就像上面的 Person Card 示例一样，当您采用这种“黄金法则”时，您很可能会意识到某些需求超出了实际组件的范围。那么，你如何实现这些目标呢？

你猜对了:集装箱？

您可以创建容器，做一点额外的工作来保持组件的自然性。当你这样做的时候，你会得到一个更简单的更集中的组件和一个更好测试的容器。

让我们实现一个 PersonCard 容器来说明这个例子。

#### 使用高阶组件实现容器

React Redux 使用[高阶组件](https://reactjs.org/docs/higher-order-components.html)来实现从 Redux 商店推送和映射道具的容器。既然我们从 React Redux 得到了这个术语，那么 [React Redux 的`connect`是一个容器](https://redux.js.org/basics/usage-with-react#implementing-container-components)就不足为奇了。

无论您是使用函数组件来映射道具，还是使用高阶组件来连接 Redux store，黄金法则和容器的工作都是一样的。首先，写出你的自然分量，然后用高阶分量来弥补这个差距。

上面的文件夹结构:

```
/PersonCard
  -PersonCard.js ----------------- natural component
  -PersonCard.test.js
  -index.js ---------------------- container
  -index.test.js
  /helpers
    -getPictureUrl.js ------------ helper
    -getPictureUrl.test.js
```

> ***注:*** *这种情况下，给`getPictureUrl`找个帮手也不会太实际。这个逻辑被分离出来只是为了说明你可以。您可能还注意到，无论容器实现如何，文件夹结构都没有区别。*

如果你以前用过 Redux，上面的例子你可能已经很熟悉了。同样，这条黄金法则不一定是新想法，而是一种微妙的新思维方式。

此外，当您实现具有更高阶组件的容器时，您还能够在功能上将更高阶组件组合在一起——将属性从一个更高阶组件传递到下一个组件。历史上，我们将多个高阶组件链接在一起实现一个容器。

> ***2019 注:****React 社区似乎正在远离高阶组件作为一种模式。*

> 我也会推荐同样的东西。我在使用它们时的经验是，它们可能会让不熟悉功能组合的团队成员感到困惑，并且它们可能会导致所谓的“包装地狱”,即组件被包装太多次，从而导致严重的性能问题。

> *这里有一些这方面的相关文章和资源:[钩谈](https://youtu.be/dpw9EHDh2bM?t=710) (2018) [重组谈](https://youtu.be/zD_judE-bXk?t=1101)(2016)[用一个渲染道具！](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce)(2017)[何时不使用渲染道具](https://blog.kentcdodds.com/when-to-not-use-render-props-5397bbeff746) (2018)。*

### 你答应给我钩子

#### 使用钩子实现容器

为什么钩子会出现在这篇文章中？因为使用钩子实现容器变得容易多了。

如果你不熟悉 React hooks，那么我建议你在 2018 年 React Conf 大会上观看丹·阿布拉莫夫和瑞恩·弗洛伦斯介绍该概念的讲座。

要点是钩子是 React 团队对具有[高阶组件](https://reactjs.org/docs/higher-order-components.html)和[相似模式](https://reactjs.org/docs/render-props.html)的问题的回应。在大多数情况下，React 挂钩是这两种模式的最佳替代模式。

这意味着实现容器可以用一个函数组件和钩子来完成？

在下面的例子中，我们使用钩子`useRoute`和`useRedux`来表示“外部世界”,并且我们使用助手`getValues`来将外部世界映射到自然组件可用的`props`。我们还使用助手`transformValues`将组件的输出转换到由`dispatch`表示的外部世界。

```
import React from 'react';
import PropTypes from 'prop-types';

import { useRouter } from 'react-router';
import { useRedux } from 'react-redux';

import actionCreator from 'your-redux-stuff';

import getValues from './helpers/getVaules';
import transformValues from './helpers/transformValues';

import FooComponent from './FooComponent';

export default function FooComponentContainer(props) {
  // hooks
  const { match } = useRouter({ path: /* ... */ });
  // NOTE: `useRedux` does not exist yet and probably won't look like this
  const { state, dispatch } = useRedux();

  // mapping
  const props = getValues(state, match);

  function handleChange(e) {
    const transformed = transformValues(e);
    dispatch(actionCreator(transformed));
  }

  // natural component
  return <FooComponent {...props} onChange={handleChange} />;
}

FooComponentContainer.propTypes = { /* ... */ };
```

这里是参考文件夹结构:

```
/FooComponent ----------- the whole component for others to import
  -FooComponent.js ------ the "natural" part of the component
  -FooComponent.test.js
  -index.js ------------- the "container" that bridges the gap
  -index.js.test.js         and provides dependencies
  /helpers -------------- isolated helpers that you can test easily
    -getValues.js
    -getValues.test.js
    -transformValues.js
    -transformValues.test.js
```

### 容器中的点火事件

我发现自己偏离自然组件的最后一种场景是当我需要触发与改变道具或安装组件相关的事件时。

例如，假设你的任务是制作一个仪表板。设计团队给你一个仪表板的模型，你把它转换成一个 React 组件。现在，您必须用数据填充这个仪表板。

您注意到，当您的组件挂载时，您需要调用一个函数(例如`dispatch(fetchAction)`)来实现这一点。

在这样的场景中，我发现自己添加了`componentDidMount`和`componentDidUpdate`生命周期方法，并添加了`onMount`或`onDashboardIdChanged`道具，因为我需要一些事件来触发，以便将我的组件链接到外部世界。

遵循黄金法则，这些`onMount`和`onDashboardIdChanged`道具是非自然的，因此应该放在容器中。

钩子的好处是它使得调度事件`onMount`或者属性改变更加简单！

**装载上的触发事件:**

要在挂载时触发事件，用空数组调用`useEffect`。

```
import React, { useEffect } from 'react';
import PropTypes from 'prop-types';
import { useRedux } from 'react-redux';

import fetchSomething_reduxAction from 'your-redux-stuff';
import getValues from './helpers/getVaules';
import FooComponent from './FooComponent';

export default function FooComponentContainer(props) {
  // hooks
  // NOTE: `useRedux` does not exist yet and probably won't look like this
  const { state, dispatch } = useRedux();

  // dispatch action onMount
  useEffect(() => {
    dispatch(fetchSomething_reduxAction);
  }, []); // the empty array tells react to only fire on mount
  // https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects

  // mapping
  const props = getValues(state, match);

  // natural component
  return <FooComponent {...props} />;
}

FooComponentContainer.propTypes = { /* ... */ }; 
```

**道具变化触发事件:**

能够在重新渲染之间观察你的属性，并在属性改变时调用你给它的函数。

在`useEffect`之前，我发现自己添加了不自然的生命周期方法和`onPropertyChanged`道具，因为我没有办法在组件之外进行属性区分:

```
import React from 'react';
import PropTypes from 'prop-types';

/**
 * Before `useEffect`, I found myself adding "unnatural" props
 * to my components that only fired events when the props diffed.
 *
 * I'd find that the component's `render` didn't even use `id`
 * most of the time
 */
export default class BeforeUseEffect extends React.Component {
  static propTypes = {
    id: PropTypes.string.isRequired,
    onIdChange: PropTypes.func.isRequired,
  };

  componentDidMount() {
    this.props.onIdChange(this.props.id);
  }

  componentDidUpdate(prevProps) {
    if (prevProps.id !== this.props.id) {
      this.props.onIdChange(this.props.id);
    }
  }

  render() {
    return // ...
  }
}
```

现在有了`useEffect`,有了一种非常轻量级的方式来触发道具变化，我们实际的组件不必添加对其功能不必要的道具。

```
import React, { useEffect } from 'react';
import PropTypes from 'prop-types';
import { useRedux } from 'react-redux';

import fetchSomething_reduxAction from 'your-redux-stuff';
import getValues from './helpers/getVaules';
import FooComponent from './FooComponent';

export default function FooComponentContainer({ id }) {
  // hooks
  // NOTE: `useRedux` does not exist yet and probably won't look like this
  const { state, dispatch } = useRedux();

  // dispatch action onMount
  useEffect(() => {
    dispatch(fetchSomething_reduxAction);
  }, [id]); // `useEffect` will watch this `id` prop and fire the effect when it differs
  // https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects

  // mapping
  const props = getValues(state, match);

  // natural component
  return <FooComponent {...props} />;
}

FooComponentContainer.propTypes = {
  id: PropTypes.string.isRequired,
}; 
```

> ***免责声明:****`useEffect`之前，有很多方法可以在容器内部使用其他高阶组件进行属性区分(比如[重新组合生命周期](https://github.com/acdlite/recompose/blob/3db12ce7121a050b533476958ff3d66ded1c4bb8/docs/API.md#lifecycle))或者创建一个生命周期组件，比如 [react router 在内部做的事情](https://github.com/ReactTraining/react-router/blob/89a72d58ac55b2d8640c25e86d1f1496e4ba8d6c/packages/react-router/modules/Lifecycle.js)，但是这些方法要么让团队感到困惑，要么非常规。*

### 这里有什么好处？

#### 组件保持有趣

对我来说，创建组件是前端开发中最有趣、最满足的部分。你可以把你团队的想法和梦想变成真实的体验，我想这是一种我们都有同感的好感觉。

永远不会出现你的组件的 API 和体验被“外界”毁掉的场景。没有额外的支持，你的组件就会变成你想象的样子——这是我最喜欢的黄金法则的好处。

#### 更多的测试和重用机会

当您采用这样的体系结构时，您实际上是将一个新的数据 y 层带到了表面上。在这一“层”中，您可以切换到更关心进入组件的数据的正确性和组件如何工作的位置。

无论你是否意识到，这一层已经存在于你的应用程序中，但它可能与表示逻辑相结合。我发现，当我将这一层浮出水面时，我可以进行大量的代码优化，并重用大量的逻辑，否则我会在不知道共性的情况下重写这些逻辑。

我认为随着定制钩子的加入，这一点会变得更加明显。定制钩子为我们提供了一种更简单的方法来提取逻辑和订阅外部变化——这是辅助函数所不能做到的。

#### 最大化团队吞吐量

在团队中工作时，您可以将容器和组件的开发分开。如果您事先就 API 达成一致，您可以同时处理:

1.  Web API(即后端)
2.  从 web API(或类似的)获取数据，并将数据转换为组件的 API
3.  组件

### 有什么例外吗？

很像真正的黄金法则，这个黄金法则也是一个黄金法则。在某些场景中，编写看似不自然的组件 API 来降低某些转换的复杂性是有意义的。

一个简单的例子就是道具的名字。如果工程师以“更自然”为由重命名数据键，事情会变得更加复杂。

这种想法很有可能走得太远，以至于过早地过度概括，这也可能是一个陷阱。

### 底线

或多或少，这条“黄金法则”只是以一种新的方式重新定义了现有的表示组件和容器组件的概念。如果你在基本层面上评估你的组件需要什么，那么你可能会得到更简单和更易读的部分。

谢谢大家！