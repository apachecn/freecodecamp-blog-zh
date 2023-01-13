# 又一个关于反应道具的帖子

> 原文：<https://www.freecodecamp.org/news/yet-another-post-about-react-props/>

你可以说这个话题已经[完成](https://flaviocopes.com/react-pass-props-to-children/) [到](https://medium.com/better-programming/passing-data-to-props-children-in-react-5399baea0356) [死亡](https://zhenyong.github.io/react/docs/jsx-spread.html)，但是最近我开始使用一种我不记得在其他地方遇到过的技巧。虽然它不是特别聪明，但它很简洁。所以请原谅我在这个话题上又发了一个帖子...

# 支持冗长的方式

> “不要害怕！我们不会让你成为作家，而有一个诚实的贸易学习，或制砖。”

我将基于一个使用 [jsheatmap](https://www.npmjs.com/package/jsheatmap) 的 [React 应用程序](https://pokermap.netlify.com/)来举例，这是我为生成热图数据而编写的一个包。热图的呈现是通过一个`<table>`完成的，其中每个单元格的背景颜色被设置为一个 RGB 值，该值是 jsheatmap 从一组给定的输入值中生成的。

```
const HeatMapTable = () => {
  const [players, setPlayers] = useState(2);
  const [suited, setSuited] = useState(false)
  const [ties, setTies] = useState(false)
  const [data, setData] = useState(getNewData(players, suited, ties))
```

有一个 PlayersRow 组件，它包含允许用户设置确定特定扑克赔率所需的玩家数量的控件。它不仅需要初始值，还需要一个设置器来设置新值。这些属性(道具)是`players`和`setPlayers`。

当将组件包含在其容器(HeatMapTable)中时，可以使用历史悠久的技术将这些属性作为显式属性进行传递。

```
<Players players={players} setPlayers={setPlayers} />
```

很基本的东西。

# 道具简洁的道

> “求求你，先生，我还要一些。”

在这种情况下(经常发生)，用于保存属性值的变量通常与 React 组件的属性名称相同。这允许使用更简洁的语法将道具传递给孩子。

在刚刚展示的例子中，有两个道具；你可能会有更多。一种技术是使用一个中间对象来保存子组件所需的道具，然后使用 spread 操作符将道具对象“扩展”成属性值。

```
const props = {players, setPlayers, anotherProp, yetAnotherProp, etc};
<Players {...props} />
```

这相当于更加冗长的:

```
<Players players={players} setPlayers={setPlayers} anotherProp={anotherProp} yetAnotherProp={yetAnotherProp} etc={etc} />
```

# 支持思考方式

> “唉！大自然的面孔很少能让我们独自欣赏它们的美丽！”

事实证明，中间变量并不是真正需要的，因为你可以这样做:

```
<Players {...{players, setPlayers, anotherProp, yetAnotherProp, etc}} />
```

或者甚至是用扩展操作符混合中间对象的东西:

```
const props = {anotherProp, yetAnotherProp, etc};
<Players {...{players, setPlayers, ...props}} />
```

正如我所说的，这并不聪明，它只是使用 spread 操作符来扩展 JavaScript 内部声明的对象，用外部的`{}`表示。

* * *

原来就这些！我发现自己越来越经常使用的一个小技巧。