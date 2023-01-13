# 如何用 React 对表格数据进行排序

> 原文：<https://www.freecodecamp.org/news/sort-table-data-with-react/>

通常，当您有一个包含信息的表格时，您会希望能够按升序或降序对表格中的信息进行排序，尤其是在处理数字时。

在本教程中，我们将看到如何使用 ReactJS 做到这一点。

这是我们将要建造的:

[https://codepen.io/FlorinPop17/embed/preview/gVYYxe?height=300&slug-hash=gVYYxe&default-tabs=js,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/gVYYxe?height=300&slug-hash=gVYYxe&default-tabs=js,result&host=https://codepen.io)

我们有一份世界十大亿万富翁的名单，我们希望根据亿万富翁的净资产对名单进行排序。我从[theweek.co.uk](https://www.theweek.co.uk/people/57553/top-billionaires-who-richest-person-world)网站上获得了名单信息。

## 先决条件

在我们继续之前，让我们看看我们将在本教程中使用的东西:

1.  [字体真棒](https://fontawesome.com) -用于图标
2.  [粉底](https://foundation.zurb.com/) -用于一般造型。我们特别为表格样式使用这个，因为我们不想被本教程中的样式分散注意力
3.  [React js](https://reactjs.org)——请**注意**我不会在本教程中解释 React 的基础知识。继续我假设你以前用过它(尽管我们要做的事情一点也不难？)
4.  数据——如上所述，我们将得到一份世界十大亿万富翁的名单以及他们的净资产

## 数据

我们将创建一个数组，其中的对象包含亿万富翁的姓名以及他们的净资产(以十亿美元计):

```
const tableData = [
	{
		name: 'Amancio Ortega',
		net_worth: 62.7
	},
	{
		name: 'Bernard Arnault',
		net_worth: 76
	},
	{
		name: 'Bill Gates',
		net_worth: 96.5
	},
	{
		name: 'Carlos Sim Helu',
		net_worth: 64
	},
	{
		name: 'Jeff Bezos',
		net_worth: 131
	},
	{
		name: 'Larry Ellison',
		net_worth: 58
	},
	{
		name: 'Larry Page',
		net_worth: 50.8
	},
	{
		name: 'Mark Zuckerberg',
		net_worth: 62.3
	},
	{
		name: 'Michael Bloomberg',
		net_worth: 55.5
	},
	{
		name: 'Warren Buffet',
		net_worth: 82.5
	}
]; 
```

## 应用程序组件

该组件将是在页面上生成的主要组件。它只有一些文本+组件`<Table />`,并把我们上面声明的`tableData`传递给它。

```
const App = () => (
	<div className='text-center'>
		<h4>A list of top 10 richest billionaires.</h4>
		<p>
			Click on the icon next to "Net Worth" to see the sorting functionality
		</p>

		<Table data={tableData} />

		<small>
			* Data gathered from{' '}
			<a
				href='https://www.theweek.co.uk/people/57553/top-billionaires-who-richest-person-world'
				target='_blank'>
				theweek.co.uk
			</a>
		</small>
	</div>
);

ReactDOM.render(<App />, document.getElementById('app')); 
```

现在所有的问题都解决了，我们可以专注于重要的事情了？：

## 表格组件

它将是一个类组件，因为我们需要使用其中的状态，但首先让我们关注一下`render`方法。我们将`map`覆盖来自父组件的`data`，并为数组中的每个数据创建一个表行(`tr`)。除此之外，我们还有一个基本的表格结构(`table > thead + tbody`)。

```
class Table extends React.Component {
	render() {
		const { data } = this.props;

		return (
			data.length > 0 && (
				<table className='text-left'>
					<thead>
						<tr>
							<th>Name</th>
							<th>Net Worth</th>
						</tr>
					</thead>
					<tbody>
						{data.map(p => (
							<tr>
								<td>{p.name}</td>
								<td>${p.net_worth}b</td>
							</tr>
						))}
					</tbody>
				</table>
			)
		);
	}
} 
```

接下来，排序...

我们将有三种类型的排序:`'default'`、`'up'`(升序)、`'down'`(降序)。这些类型将在一个按钮的帮助下改变，该按钮将有一个 FontAwesome 图标，取决于当前激活的排序类型。让我们创建一个将为我们提供必要信息的对象:

```
const sortTypes = {
	up: {
		class: 'sort-up',
		fn: (a, b) => a.net_worth - b.net_worth
	},
	down: {
		class: 'sort-down',
		fn: (a, b) => b.net_worth - a.net_worth
	},
	default: {
		class: 'sort',
		fn: (a, b) => a
	}
}; 
```

如你所见，每种分类都有两个道具:

1.  `class` -这将由按钮中的图标使用，因为我们将看到哪个状态当前处于活动状态
2.  `fn` -这将是我们在表中显示数组之前用来对数组中的项目进行排序的`function`。基本上我们是在比较对象的`net_worth`属性

到目前为止很棒！？

我们还需要向`Table`组件添加一个`currentSort`状态，它将跟踪活动的排序类型，最后，我们将有一个`onSortChange`方法，每次点击排序按钮时都会调用它，它将改变`currentSort`值。

很多话？，但是让我们看看代码，我打赌你会明白的？：

```
class Table extends React.Component {
	// declaring the default state
	state = {
		currentSort: 'default'
	};

	// method called every time the sort button is clicked
	// it will change the currentSort value to the next one
	onSortChange = () => {
		const { currentSort } = this.state;
		let nextSort;

		if (currentSort === 'down') nextSort = 'up';
		else if (currentSort === 'up') nextSort = 'default';
		else if (currentSort === 'default') nextSort = 'down';

		this.setState({
			currentSort: nextSort
		});
	};

	render() {
		const { data } = this.props;
		const { currentSort } = this.state;

		return (
			data.length > 0 && (
				<table className='text-left'>
					<thead>
						<tr>
							<th>Name</th>
							<th>
								Net Worth
								<button onClick={this.onSortChange}>
									<i className={`fas fa-${sortTypes[currentSort].class}`} />
								</button>
							</th>
						</tr>
					</thead>
					<tbody>
						{[...data].sort(sortTypes[currentSort].fn).map(p => (
							<tr>
								<td>{p.name}</td>
								<td>${p.net_worth}b</td>
							</tr>
						))}
					</tbody>
				</table>
			)
		);
	}
} 
```

注意，我们没有改变原来的`data`数组，而是用`...` (spread)操作符创建了另一个数组，然后我们使用`sortTypes`对象给出的`fn`对数组进行相应的排序。

## 结论

差不多就是这样！现在您知道了如何根据列中的值对表进行排序。恭喜你！

编码快乐！

*原贴于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/07/sort-table-data-with-react/)*