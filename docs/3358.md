# 如何使用 React 挂钩和上下文 API 构建一个简单的神奇宝贝 Web 应用程序

> 原文：<https://www.freecodecamp.org/news/building-a-simple-pokemon-web-app-with-react-hooks-and-context-api/>

经过七年使用 Ruby、Python 和 vanilla JavaScript 的全栈开发，这些天我主要使用 JavaScript、Typescript、React 和 Redux。

JavaScript 社区很棒，发展非常快。很多东西都是“一夜之间”创造出来的，通常是象征性的，但有时也是字面上的意思。这一切都让它很难跟上时代。

总觉得 JavaScript 聚会要迟到了。我也想去，尽管我不太喜欢派对。

仅仅使用 React 和 Redux 一年，我觉得我需要学习新的东西，比如钩子和上下文 API 来管理状态。在阅读了一些关于它的文章后，我想尝试这些概念，所以我创建了一个简单的项目作为实验室来试验这些东西。

我从小就对神奇宝贝充满热情。在 Game Boy 上玩游戏并征服所有联赛总是很有趣。现在作为一名开发者，我想尝试一下[神奇宝贝 API](https://pokeapi.co/) 。

我决定构建一个简单的 web 页面，在页面的不同部分共享数据。该页面将有三个主要部分:

*   一个盒子，里面有所有现存神奇宝贝的清单
*   一个盒子，里面有所有捕获的神奇宝贝的清单
*   一个输入框，用于向列表中添加新的神奇宝贝

并且每个盒子将具有以下行为或动作:

*   对于第一个盒子里的每个神奇宝贝，我可以捕捉它们并发送到第二个盒子
*   对于第二个盒子中的每个神奇宝贝，我可以释放它们并发送到第一个盒子
*   作为一个游戏之神，我能够通过填写输入并将其发送到第一个盒子来创建神奇宝贝

所以我想实现的所有特性都很清楚——列表和动作。

## 上市口袋妖怪

我想首先构建的基本功能是列出神奇宝贝。所以对于一个对象数组，我想列出并显示每个对象的`name`属性。

我从第一个盒子开始:现有的神奇宝贝。

起初我认为我不需要神奇宝贝 API——我可以模仿这个列表，看看它是否有效。有了`useState`，我可以声明我的组件状态并使用它。

我们用模拟神奇宝贝列表的默认值来定义它，只是为了测试它:

```
const [pokemons] = useState([
  { id: 1, name: 'Bulbasaur' },
  { id: 2, name: 'Charmander' },
  { id: 3, name: 'Squirtle' }
]); 
```

这里我们有三个神奇宝贝的列表。`useState`钩子提供了一对条目:当前状态和一个让您更新这个已创建状态的函数。

现在有了神奇宝贝的状态，我们就可以对它进行映射，并渲染出每个神奇宝贝的名字。

```
{pokemons.map((pokemon) => <p>{pokemon.name}</p>)} 
```

它只是一个在段落标签中返回每个神奇宝贝名称的地图。

这是实现的整个组件:

```
import React, { useState } from 'react';

const PokemonsList = () => {
  const [pokemons] = useState([
    { id: 1, name: 'Bulbasaur' },
    { id: 2, name: 'Charmander' },
    { id: 3, name: 'Squirtle' }
  ]);

  return (
    <div className="pokemons-list">
      <h2>Pokemons List</h2>

      {pokemons.map((pokemon) =>
        <div key={`${pokemon.id}-${pokemon.name}`}>
          <p>{pokemon.id}</p>
          <p>{pokemon.name}</p>
        </div>)}
    </div>
  )
}

export default PokemonsList; 
```

这里有一点小小的改动:

*   我在神奇宝贝的`id`和`name`的组合中加入了`key`
*   我还为`id`属性渲染了一段(我只是在测试它。但是我们稍后会删除它。)

太好了！现在我们有了第一份名单。

我想做同样的实现，但现在是为了捕捉到的神奇宝贝。但是对于捕获的神奇宝贝，我首先要创建一个空列表，因为当“游戏”开始时，我不会有任何捕获的神奇宝贝，对吗？对！

```
const [pokemons] = useState([]); 
```

就是这样，真的很简单！

整个组件看起来与另一个组件相似:

```
import React, { useState } from 'react';

const CapturedPokemons = () => {
  const [pokemons] = useState([]);

  return (
    <div className="pokedex">
      <h2>Captured Pokemons</h2>

      {pokemons.map((pokemon) =>
        <div key={`${pokemon.id}-${pokemon.name}`}>
          <p>{pokemon.id}</p>
          <p>{pokemon.name}</p>
        </div>)}
    </div>
  )
}

export default CapturedPokemons; 
```

这里我们使用了`map`，但是因为数组是空的，所以它不呈现任何东西。

现在我有了两个主要组件，我可以在`App`组件中一起使用它们:

```
import React from 'react';
import './App.css';

import PokemonsList from './PokemonsList';
import Pokedex from './Pokedex';

const App = () => (
  <div className="App">
    <PokemonsList />
    <Pokedex />
  </div>
);

export default App; 
```

## 捕获和释放

这是我们的应用程序的第二部分，我们可以捕捉和释放神奇宝贝。让我们回顾一下预期的行为。

对于可用神奇宝贝列表中的每只神奇宝贝，我想启用一个动作来捕捉它们。捕获操作会将它们从它们所在的列表中移除，并将它们添加到捕获的神奇宝贝列表中。

释放动作将具有类似的行为。但是不是从可用列表移动到捕获列表，而是相反。我们会将它们从捕获列表移动到可用列表。

因此，两个盒子都需要共享数据，才能将神奇宝贝添加到另一个列表中。我们如何做到这一点，因为它们是应用程序中的不同组件？我们来谈谈 React 上下文 API。

上下文 API 被设计为为一个已定义的 React 组件树创建全局数据。由于数据是全局的，我们可以在这个定义的树中的组件之间共享它。所以让我们用它在两个盒子之间分享我们简单的口袋妖怪数据。

注意:“上下文主要用于不同嵌套层次的许多组件需要访问某些数据的时候。”-反应医生。

使用 API，我们简单地创建一个新的上下文，如下所示:

```
import { createContext } from 'react';

const PokemonContext = createContext(); 
```

现在，有了`PokemonContext`，我们可以使用它的提供者。它将作为组件树的组件包装器。它向这些组件提供全局数据，并使它们能够订阅与该上下文相关的任何更改。看起来是这样的:

```
<PokemonContext.Provider value={/* some value */}> 
```

`value` prop 只是这个上下文提供给包装组件的一个值。我们应该向可用列表和捕获列表提供什么？

*   `pokemons`:在可用列表中列出
*   `capturedPokemons`:在捕获列表中列出
*   `setPokemons`:能够更新可用列表
*   `setCapturedPokemons`:能够更新抓取列表

正如我在前面的`useState`部分提到的，这个钩子总是提供一对:状态和更新这个状态的函数。这个函数处理和更新上下文状态。换言之，他们是`setPokemons`和`setCapturedPokemons`。怎么会？

```
const [pokemons, setPokemons] = useState([
  { id: 1, name: 'Bulbasaur' },
  { id: 2, name: 'Charmander' },
  { id: 3, name: 'Squirtle' }
]); 
```

现在我们有了`setPokemons`。

```
const [capturedPokemons, setCapturedPokemons] = useState([]); 
```

现在我们也有了`setCapturedPokemons`。

有了所有这些值，我们现在可以将它们传递给提供者的`value` prop。

```
import React, { createContext, useState } from 'react';

export const PokemonContext = createContext();

export const PokemonProvider = (props) => {
  const [pokemons, setPokemons] = useState([
    { id: 1, name: 'Bulbasaur' },
    { id: 2, name: 'Charmander' },
    { id: 3, name: 'Squirtle' }
  ]);

  const [capturedPokemons, setCapturedPokemons] = useState([]);

  const providerValue = {
    pokemons,
    setPokemons,
    capturedPokemons,
    setCapturedPokemons
  };

  return (
    <PokemonContext.Provider value={providerValue}>
      {props.children}
    </PokemonContext.Provider>
  )
}; 
```

我创建了一个`PokemonProvider`来包装所有这些数据和 API，以创建上下文并返回带有定义值的上下文提供者。

但是我们如何向组件提供所有这些数据和 API 呢？我们需要做两件主要的事情:

*   将组件包装到这个上下文提供者中
*   在每个组件中使用上下文

让我们先把它们包起来:

```
const App = () => (
  <PokemonProvider>
    <div className="App">
      <PokemonsList />
      <Pokedex />
    </div>
  </PokemonProvider>
); 
```

我们通过使用`useContext`并传递创建的`PokemonContext`来使用上下文。像这样:

```
import { useContext } from 'react';
import { PokemonContext } from './PokemonContext';

useContext(PokemonContext); // returns the context provider value we created 
```

我们希望能够捕捉到可用的神奇宝贝，所以让`setCapturedPokemons`函数 API 更新捕捉到的神奇宝贝会很有用。

当每个神奇宝贝被捕获时，我们需要将它从可用列表中删除。`setPokemons`这里也需要。为了更新每个列表，我们需要最新的数据。所以基本上我们需要来自上下文提供者的一切。

我们需要构建一个带有捕捉神奇宝贝动作的按钮:

*   `<button>`用一个`onClick`标记调用`capture`函数并传递神奇宝贝

```
<button onClick={capture(pokemon)}>+</button> 
```

*   `capture`功能将更新`pokemons`和`capturedPokemons`列表

```
const capture = (pokemon) => (event) => {
  // update captured pokemons list
  // update available pokemons list
}; 
```

要更新`capturedPokemons`，我们可以用当前的`capturedPokemons`和要捕捉的神奇宝贝调用`setCapturedPokemons`函数。

```
setCapturedPokemons([...capturedPokemons, pokemon]); 
```

而要更新`pokemons`列表，只需过滤将要捕捉的神奇宝贝即可。

```
setPokemons(removePokemonFromList(pokemon)); 
```

`removePokemonFromList`只是一个简单的功能，通过移除捕获的神奇宝贝来过滤神奇宝贝。

```
const removePokemonFromList = (removedPokemon) =>
  pokemons.filter((pokemon) => pokemon !== removedPokemon) 
```

组件现在看起来怎么样？

```
import React, { useContext } from 'react';
import { PokemonContext } from './PokemonContext';

export const PokemonsList = () => {
  const {
    pokemons,
    setPokemons,
    capturedPokemons,
    setCapturedPokemons
  } = useContext(PokemonContext);

  const removePokemonFromList = (removedPokemon) =>
    pokemons.filter(pokemon => pokemon !== removedPokemon);

  const capture = (pokemon) => () => {
    setCapturedPokemons([...capturedPokemons, pokemon]);
    setPokemons(removePokemonFromList(pokemon));
  };

  return (
    <div className="pokemons-list">
      <h2>Pokemons List</h2>

      {pokemons.map((pokemon) =>
        <div key={`${pokemon.id}-${pokemon.name}`}>
          <div>
            <span>{pokemon.name}</span>
            <button onClick={capture(pokemon)}>+</button>
          </div>
        </div>)}
    </div>
  );
};

export default PokemonsList; 
```

它将看起来非常类似于捕获的神奇宝贝组件。代替`capture`，它将是一个`release`函数:

```
import React, { useContext } from 'react';
import { PokemonContext } from './PokemonContext';

const CapturedPokemons = () => {
  const {
    pokemons,
    setPokemons,
    capturedPokemons,
    setCapturedPokemons,
  } = useContext(PokemonContext);

  const releasePokemon = (releasedPokemon) =>
    capturedPokemons.filter((pokemon) => pokemon !== releasedPokemon);

  const release = (pokemon) => () => {
    setCapturedPokemons(releasePokemon(pokemon));
    setPokemons([...pokemons, pokemon]);
  };

  return (
    <div className="captured-pokemons">
      <h2>CapturedPokemons</h2>

      {capturedPokemons.map((pokemon) =>
        <div key={`${pokemon.id}-${pokemon.name}`}>
          <div>
            <span>{pokemon.name}</span>
            <button onClick={release(pokemon)}>-</button>
          </div>
        </div>)}
    </div>
  );
};

export default CapturedPokemons; 
```

## 降低复杂性

现在我们使用`useState`钩子、上下文 API 和上下文提供者`useContext`。更重要的是，我们可以在神奇宝贝盒子之间共享数据。

管理状态的另一种方法是使用`useReducer`作为`useState`的替代。

减速器生命周期是这样工作的:`useReducer`提供了一个`dispatch`功能。通过这个函数，我们可以在组件内部分派一个`action`。`reducer`接收动作和状态。它理解动作的类型，处理数据，并返回一个新的状态。现在，新状态可以在组件中使用了。

作为一个练习，为了更好地理解这个钩子，我试着用它代替`useState`。

`useState`是在`PokemonProvider`里面。我们可以在这个数据结构中重新定义可用的和捕获的神奇宝贝的初始状态:

```
const defaultState = {
  pokemons: [
    { id: 1, name: 'Bulbasaur' },
    { id: 2, name: 'Charmander' },
    { id: 3, name: 'Squirtle' }
  ],
  capturedPokemons: []
}; 
```

并将该值传递给`useReducer`:

```
const [state, dispatch] = useReducer(pokemonReducer, defaultState); 
```

`useReducer`接收两个参数:减速器和初始状态。让我们现在建造`pokemonReducer`。

缩减器接收当前状态和被调度的动作。

```
const pokemonReducer = (state, action) => // returns the new state based on the action type 
```

这里我们得到动作类型并返回一个新的状态。动作是一个对象。看起来是这样的:

```
{ type: 'AN_ACTION_TYPE' } 
```

但也可能更大:

```
{
  type: 'AN_ACTION_TYPE',
  pokemon: {
    name: 'Pikachu'
  }
} 
```

在这种情况下，我们将把一个神奇宝贝传递给动作对象。让我们暂停一分钟，想一想我们想在减速器内部做什么。

在这里，我们通常更新数据和处理动作。动作是调度的，所以动作就是行为。而来自我们 app 的行为是*捕捉*和*释放*！这些是我们在这里需要处理的行动。

这是我们的减速器的样子:

```
const pokemonReducer = (state, action) => {
  switch (action.type) {
    case 'CAPTURE':
      // handle capture and return new state
    case 'RELEASE':
      // handle release and return new state
    default:
      return state;
  }
}; 
```

如果我们的动作类型是`CAPTURE`，我们用一种方式处理它。如果我们的动作类型是`RELEASE`，我们用另一种方式处理它。如果动作类型与这些类型都不匹配，就返回当前状态。

当我们捕获神奇宝贝时，我们需要更新两个列表:从可用列表中删除神奇宝贝，并将其添加到捕获列表中。这个状态就是我们需要从 reducer 返回的。

```
const getPokemonsList = (pokemons, capturedPokemon) =>
  pokemons.filter(pokemon => pokemon !== capturedPokemon)

const capturePokemon = (pokemon, state) => ({
  pokemons: getPokemonsList(state.pokemons, pokemon),
  capturedPokemons: [...state.capturedPokemons, pokemon]
}); 
```

`capturePokemon`函数只返回更新后的列表。`getPokemonsList`从可用列表中删除捕获的神奇宝贝。

我们在减速器中使用了这个新功能:

```
const pokemonReducer = (state, action) => {
  switch (action.type) {
    case 'CAPTURE':
      return capturePokemon(action.pokemon, state);
    case 'RELEASE':
      // handle release and return new state
    default:
      return state;
  }
}; 
```

现在的`release`功能！

```
const getCapturedPokemons = (capturedPokemons, releasedPokemon) =>
  capturedPokemons.filter(pokemon => pokemon !== releasedPokemon)

const releasePokemon = (releasedPokemon, state) => ({
  pokemons: [...state.pokemons, releasedPokemon],
  capturedPokemons: getCapturedPokemons(state.capturedPokemons, releasedPokemon)
}); 
```

`getCapturedPokemons`从捕获列表中移除释放的神奇宝贝。`releasePokemon`函数返回更新后的列表。

我们的减速器现在看起来像这样:

```
const pokemonReducer = (state, action) => {
  switch (action.type) {
    case 'CAPTURE':
      return capturePokemon(action.pokemon, state);
    case 'RELEASE':
      return releasePokemon(action.pokemon, state);
    default:
      return state;
  }
}; 
```

只有一个小的重构:动作类型！这些是字符串，我们可以将它们提取到一个常量中，并提供给调度程序。

```
export const CAPTURE = 'CAPTURE';
export const RELEASE = 'RELEASE'; 
```

减速器:

```
const pokemonReducer = (state, action) => {
  switch (action.type) {
    case CAPTURE:
      return capturePokemon(action.pokemon, state);
    case RELEASE:
      return releasePokemon(action.pokemon, state);
    default:
      return state;
  }
}; 
```

整个 reducer 文件如下所示:

```
export const CAPTURE = 'CAPTURE';
export const RELEASE = 'RELEASE';

const getCapturedPokemons = (capturedPokemons, releasedPokemon) =>
  capturedPokemons.filter(pokemon => pokemon !== releasedPokemon)

const releasePokemon = (releasedPokemon, state) => ({
  pokemons: [...state.pokemons, releasedPokemon],
  capturedPokemons: getCapturedPokemons(state.capturedPokemons, releasedPokemon)
});

const getPokemonsList = (pokemons, capturedPokemon) =>
  pokemons.filter(pokemon => pokemon !== capturedPokemon)

const capturePokemon = (pokemon, state) => ({
  pokemons: getPokemonsList(state.pokemons, pokemon),
  capturedPokemons: [...state.capturedPokemons, pokemon]
});

export const pokemonReducer = (state, action) => {
  switch (action.type) {
    case CAPTURE:
      return capturePokemon(action.pokemon, state);
    case RELEASE:
      return releasePokemon(action.pokemon, state);
    default:
      return state;
  }
}; 
```

现在实现了缩减器，我们可以将它导入到我们的提供者中，并在`useReducer`钩子中使用它。

```
const [state, dispatch] = useReducer(pokemonReducer, defaultState); 
```

当我们在`PokemonProvider`中时，我们希望为消费组件提供一些值:捕获和释放动作。

这些函数只需要调度正确的动作类型，将神奇宝贝传递给 reducer 即可。

*   `capture`函数:它接收神奇宝贝并返回一个新函数，该函数调度类型为`CAPTURE`的动作和被捕获的神奇宝贝。

```
const capture = (pokemon) => () => {
  dispatch({ type: CAPTURE, pokemon });
}; 
```

*   `release`函数:它接收神奇宝贝并返回一个新函数，该函数调度类型为`RELEASE`的动作和释放的神奇宝贝。

```
const release = (pokemon) => () => {
  dispatch({ type: RELEASE, pokemon });
}; 
```

现在，有了实现的状态和动作，我们可以向消费组件提供这些值。只需更新提供商价值主张。

```
const { pokemons, capturedPokemons } = state;

const providerValue = {
  pokemons,
  capturedPokemons,
  release,
  capture
};

<PokemonContext.Provider value={providerValue}>
  {props.children}
</PokemonContext.Provider> 
```

太好了！现在回到组件。让我们用这些新动作。所有的捕获和释放逻辑都封装在我们的 provider 和 reducer 中。我们的组件现在很干净了。`useContext`将看起来像这样:

```
const { pokemons, capture } = useContext(PokemonContext); 
```

整个组件:

```
import React, { useContext } from 'react';
import { PokemonContext } from './PokemonContext';

const PokemonsList = () => {
  const { pokemons, capture } = useContext(PokemonContext);

  return (
    <div className="pokemons-list">
      <h2>Pokemons List</h2>

      {pokemons.map((pokemon) =>
        <div key={`${pokemon.id}-${pokemon.name}`}>
          <span>{pokemon.name}</span>
          <button onClick={capture(pokemon)}>+</button>
        </div>)}
    </div>
  )
};

export default PokemonsList; 
```

对于捕获的神奇宝贝组件，它看起来与`useContext`非常相似:

```
const { capturedPokemons, release } = useContext(PokemonContext); 
```

整个组件:

```
import React, { useContext } from 'react';
import { PokemonContext } from './PokemonContext';

const Pokedex = () => {
  const { capturedPokemons, release } = useContext(PokemonContext);

  return (
    <div className="pokedex">
      <h2>Pokedex</h2>

      {capturedPokemons.map((pokemon) =>
        <div key={`${pokemon.id}-${pokemon.name}`}>
          <span>{pokemon.name}</span>
          <button onClick={release(pokemon)}>-</button>
        </div>)}
    </div>
  )
};

export default Pokedex; 
```

没有逻辑。只是 UI。非常干净。

## 神奇宝贝之神——创造者

现在我们有了两个列表之间的通信，我想建立第三个盒子。这将是我们如何创造新的神奇宝贝。但它只是一个简单的输入和提交按钮。

当我们将一个神奇宝贝的名字添加到输入中并按下按钮时，它会发出一个动作将这个神奇宝贝添加到可用列表中。

因为我们需要访问可用列表来更新它，所以我们需要共享状态。所以我们的组件将被我们的`PokemonProvider`和其他组件包装在一起。

```
const App = () => (
  <PokemonProvider>
    <div className="main">
      <PokemonsList />
      <Pokedex />
    </div>
    <PokemonForm />
  </PokemonProvider>
); 
```

现在让我们构建`PokemonForm`组件。形式非常简单:

```
<form onSubmit={handleFormSubmit}>
  <input type="text" placeholder="pokemon name" onChange={handleNameOnChange} />
  <input type="submit" value="Add" />
</form> 
```

我们有一个表单、一个输入和一个按钮。总的来说，我们还有一个函数来处理表单提交，另一个函数来处理关于更改的输入。

每当用户输入或删除一个字符时，`handleNameOnChange`就会被调用。我想建立一个地方州，一个口袋妖怪名字的代表。有了这个状态，我们可以在提交表单时使用它来调度。

因为我们想尝试钩子，所以我们将使用`useState`来处理这个本地状态。

```
const [pokemonName, setPokemonName] = useState();

const handleNameOnChange = (e) => setPokemonName(e.target.value); 
```

每当用户与输入交互时，我们使用`setPokemonName`来更新`pokemonName`。

而`handleFormSubmit`是分派新的神奇宝贝添加到可用列表的功能。

```
const handleFormSubmit = (e) => {
  e.preventDefault();
  addPokemon({
    id: generateID(),
    name: pokemonName
  });
}; 
```

`addPokemon`是我们后面要构建的 API。它接收神奇宝贝的 id 和名字。名字就是我们定义的本地状态，`pokemonName`。

`generateID`只是我构建的一个生成随机数的简单函数。看起来是这样的:

```
export const generateID = () => {
  const a = Math
    .random()
    .toString(36)
    .substring(2, 15)

  const b = Math
    .random()
    .toString(36)
    .substring(2, 15)

  return a + b;
}; 
```

`addPokemon`将由我们构建的上下文 API 提供。这样，该功能可以接收新的神奇宝贝并将其添加到可用列表中。看起来是这样的:

```
const addPokemon = (pokemon) => {
  dispatch({ type: ADD_POKEMON, pokemon });
}; 
```

它将分派这个动作类型`ADD_POKEMON`并传递神奇宝贝。

在我们的 reducer 中，我们为`ADD_POKEMON`添加了案例，并处理 state 以将新的神奇宝贝添加到 state。

```
const pokemonReducer = (state, action) => {
  switch (action.type) {
    case CAPTURE:
      return capturePokemon(action.pokemon, state);
    case RELEASE:
      return releasePokemon(action.pokemon, state);
    case ADD_POKEMON:
      return addPokemon(action.pokemon, state);
    default:
      return state;
  }
}; 
```

并且`addPokemon`功能将是:

```
const addPokemon = (pokemon, state) => ({
  pokemons: [...state.pokemons, pokemon],
  capturedPokemons: state.capturedPokemons
}); 
```

另一种方法是破坏状态，只改变神奇宝贝的属性，如下所示:

```
const addPokemon = (pokemon, state) => ({
  ...state,
  pokemons: [...state.pokemons, pokemon],
}); 
```

回到我们的组件，我们只需要确保`useContext`基于`PokemonContext`提供`addPokemon`调度 API:

```
const { addPokemon } = useContext(PokemonContext); 
```

整个组件看起来像这样:

```
import React, { useContext, useState } from 'react';
import { PokemonContext } from './PokemonContext';
import { generateID } from './utils';

const PokemonForm = () => {
  const [pokemonName, setPokemonName] = useState();
  const { addPokemon } = useContext(PokemonContext);

  const handleNameOnChange = (e) => setPokemonName(e.target.value);

  const handleFormSubmit = (e) => {
    e.preventDefault();
    addPokemon({
      id: generateID(),
      name: pokemonName
    });
  };

  return (
    <form onSubmit={handleFormSubmit}>
      <input type="text" placeholder="pokemon name" onChange={handleNameOnChange} />
      <input type="submit" value="Add" />
    </form>
  );
};

export default PokemonForm; 
```

现在我们有了可用的神奇宝贝列表、捕获的神奇宝贝列表和第三个创建新神奇宝贝的盒子。

## 神奇宝贝效果

现在我们的应用程序几乎完成了，我们可以用 PokéAPI 中的神奇宝贝列表替换模拟的神奇宝贝列表。

所以，在函数组件内部，我们不能做任何像日志或订阅这样的副作用。这就是`useEffect`钩子存在的原因。有了这个钩子，我们可以获取神奇宝贝(一个副作用)，并添加到列表中。

从 PokéAPI 获取的内容如下所示:

```
const url = "https://pokeapi.co/api/v2/pokemon";
const response = await fetch(url);
const data = await response.json();
data.results; // [{ name: 'bulbasaur', url: 'https://pokeapi.co/api/v2/pokemon/1/' }, ...] 
```

`results`属性是获取的神奇宝贝列表。有了这些数据，我们就可以将它们添加到神奇宝贝列表中。

让我们获取`useEffect`中的请求代码:

```
useEffect(() => {
  const fetchPokemons = async () => {
    const response = await fetch(url);
    const data = await response.json();
    data.results; // update the pokemons list with this data
  };

  fetchPokemons();
}, []); 
```

为了能够使用`async-await`，我们需要创建一个函数，稍后再调用它。空数组是一个参数，用于确保`useEffect`知道它将查找以重新运行的依赖关系。

默认行为是运行每个已完成渲染的效果。如果我们在这个列表中添加一个依赖项，`useEffect`只会在依赖项改变时重新运行，而不是在所有完成的渲染中运行。

现在我们已经获取了神奇宝贝，我们需要更新列表。这是一种行动，一种新的行为。我们需要再次使用 dispatch，在 reducer 中实现一个新的类型，并在上下文提供者中更新状态。

在`PokemonContext`中，我们创建了`addPokemons`函数来为使用它的消费组件提供 API。

```
const addPokemons = (pokemons) => {
  dispatch({ type: ADD_POKEMONS, pokemons });
}; 
```

它接收神奇宝贝并调度一个新动作:`ADD_POKEMONS`。

在 reducer 中，我们添加了这个新类型，expect pokémon，并调用一个函数将 pokémon 添加到可用列表状态。

```
const pokemonReducer = (state, action) => {
  switch (action.type) {
    case CAPTURE:
      return capturePokemon(action.pokemon, state);
    case RELEASE:
      return releasePokemon(action.pokemon, state);
    case ADD_POKEMON:
      return addPokemon(action.pokemon, state);
    case ADD_POKEMONS:
      return addPokemons(action.pokemons, state);
    default:
      return state;
  }
}; 
```

`addPokemons`函数只是将神奇宝贝添加到列表中:

```
const addPokemons = (pokemons, state) => ({
  pokemons: pokemons,
  capturedPokemons: state.capturedPokemons
}); 
```

我们可以通过使用状态析构和对象属性值简写来重构这一点:

```
const addPokemons = (pokemons, state) => ({
  ...state,
  pokemons,
}); 
```

因为我们现在向消费组件提供了这个函数 API，所以我们可以使用`useContext`来获取它。

```
const { addPokemons } = useContext(PokemonContext); 
```

整个组件看起来像这样:

```
import React, { useContext, useEffect } from 'react';
import { PokemonContext } from './PokemonContext';

const url = "https://pokeapi.co/api/v2/pokemon";

export const PokemonsList = () => {
  const { state, capture, addPokemons } = useContext(PokemonContext);

  useEffect(() => {
    const fetchPokemons = async () => {
      const response = await fetch(url);
      const data = await response.json();
      addPokemons(data.results);
    };    

    fetchPokemons();
  }, [addPokemons]);

  return (
    <div className="pokemons-list">
      <h2>Pokemons List</h2>

      {state.pokemons.map((pokemon) =>
        <div key={pokemon.name}>
          <div>
            <span>{pokemon.name}</span>
            <button onClick={capture(pokemon)}>+</button>
          </div>
        </div>)}
    </div>
  );
};

export default PokemonsList; 
```

## 包扎

这是我尝试分享我在一个小型项目中尝试使用钩子时所学到的东西。

我们学习了如何用`useState`处理局部状态，用上下文 API 构建全局状态，如何用`useReducer`重写和替换`useState`，以及如何在`useEffect`中产生副作用。

声明:这只是一个用于学习目的的实验项目。我可能没有使用钩子的最佳实践，也没有使它们在大项目中具有可伸缩性。

我希望这是一本好书！继续学习，继续编码！

你可以在我的博客上发表类似这样的文章。

我的 [Twitter](https://twitter.com/leandrotk_) 和 [Github](https://github.com/leandrotk) 。

## 资源

*   [反应文档:上下文](https://reactjs.org/docs/context.html)
*   [反应文件:挂钩](https://reactjs.org/docs/hooks-reference.html)
*   [口袋妖怪挂钩 side-project:源代码](https://github.com/leandrotk/pokehooks-labs)
*   [通过构建 App 学习 React](https://alterclass.io/?ref=5ec57f513c1321001703dcd2)