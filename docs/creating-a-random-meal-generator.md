# 如何创建一个随机用餐生成器

> 原文：<https://www.freecodecamp.org/news/creating-a-random-meal-generator/>

上周，我决定接受新的挑战。我称之为:100 天 100 个项目的挑战。

挑战的目的是每天创建一个项目。请将其视为#100DaysOfCode 挑战的下一步。

项目可以是:

*   一个应用程序
*   一个组件
*   一个网站
*   一个游戏
*   图书馆
    等等...

使用的编程语言也不重要，但我需要在晚上 11:59(我的时间)之前完成项目，否则我会“惩罚”自己，给 5 个人 5 美元(总共 25 美元)——前 5 个人在 Twitter 上指出我错过了最后期限。？

如果你想加入，你可以在这里阅读更多关于这个挑战和其他变化的内容。

**注意**:失败了不一定要给 5 块钱，给自己定一些其他的“惩罚”就行了。此外，如果你不想接受 100 天挑战，还有其他天数更少的方案(**7 天 7 个项目**和**30 天 30 个项目**)。

* * *

对于 [#100Days100Projects](https://florin-pop.com/blog/2019/09/100-days-100-projects) 中的第一个项目，我考虑使用一个公共 API 来获取一些将在网页中显示的数据——这是 API 的常见做法。

为此，我选择使用 [TheMealDB](https://www.themealdb.com) 的公共 API，以便通过按一个按钮获得一些随机的食物。直截了当的东西！？

在 [CodePen](https://codepen.io/FlorinPop17/full/WNeggor) 上查看我们将在本文中构建的内容的现场版本:

[https://codepen.io/FlorinPop17/embed/preview/WNeggor?height=300&slug-hash=WNeggor&default-tabs=css,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/WNeggor?height=300&slug-hash=WNeggor&default-tabs=css,result&host=https://codepen.io)

一如既往，让我们从头开始:

## HTML

```
<div class="container">
	<div class="row text-center">
		<h3>
			Feeling hungry?
		</h3>
		<h5>Get a random meal by clicking below</h5>
		<button class="button-primary" id="get_meal">Get Meal ?</button>
	</div>
	<div id="meal" class="row meal"></div>
</div> 
```

我们有一小段文字，但两个最重要的部分是:

*   `#get_meal`按钮和
*   `#meal` div

我们将使用`button`向 API 发出请求。这将发回一些数据，我们将把这些数据放入充当容器的`#meal`div——在本例中。

通常在 HTML 之后，我会直接进入 CSS。但是我们还没有完整的标记，因为它将被填充到 **JavaScript** 部分，所以这是我们接下来要做的。

## JavaScript

如上所述，我们需要`button`和容器`div`:

```
const get_meal_btn = document.getElementById('get_meal');
const meal_container = document.getElementById('meal'); 
```

接下来，在我们深入研究代码之前，让我们看看 API 将返回什么。为此，请打开以下网址:[https://www.themealdb.com/api/json/v1/1/random.php](https://www.themealdb.com/api/json/v1/1/random.php)。

正如你从 URL 中看到的，我们正从这个 API 中获得一顿**随机**大餐(刷新查看*随机性*)。当我们向该端点发出一个 **GET** 请求时(比如从浏览器访问它)，它会发回一个 JSON 响应，我们可以解析这个响应以检索我们想要的数据。

数据看起来像这样:

```
{
	meals: [
		{
			idMeal: '52873',
			strMeal: 'Beef Dumpling Stew',
			strDrinkAlternate: null,
			strCategory: 'Beef',
			strArea: 'British',
			strInstructions: 'Long description',
			strMealThumb:
				'https://www.themealdb.com/images/media/meals/uyqrrv1511553350.jpg',
			strTags: 'Stew,Baking',
			strYoutube: 'https://www.youtube.com/watch?v=6NgheY-r5t0',
			strIngredient1: 'Olive Oil',
			strIngredient2: 'Butter',
			strIngredient3: 'Beef',
			strIngredient4: 'Plain Flour',
			strIngredient5: 'Garlic',
			strIngredient6: 'Onions',
			strIngredient7: 'Celery',
			strIngredient8: 'Carrots',
			strIngredient9: 'Leek',
			strIngredient10: 'Swede',
			strIngredient11: 'Red Wine',
			strIngredient12: 'Beef Stock',
			strIngredient13: 'Bay Leaf',
			strIngredient14: 'Thyme',
			strIngredient15: 'Parsley',
			strIngredient16: 'Plain Flour',
			strIngredient17: 'Baking Powder',
			strIngredient18: 'Suet',
			strIngredient19: 'Water',
			strIngredient20: '',
			strMeasure1: '2 tbs',
			strMeasure2: '25g',
			strMeasure3: '750g',
			strMeasure4: '2 tblsp ',
			strMeasure5: '2 cloves minced',
			strMeasure6: '175g',
			strMeasure7: '150g',
			strMeasure8: '150g',
			strMeasure9: '2 chopped',
			strMeasure10: '200g',
			strMeasure11: '150ml',
			strMeasure12: '500g',
			strMeasure13: '2',
			strMeasure14: '3 tbs',
			strMeasure15: '3 tblsp chopped',
			strMeasure16: '125g',
			strMeasure17: '1 tsp ',
			strMeasure18: '60g',
			strMeasure19: 'Splash',
			strMeasure20: '',
			strSource:
				'https://www.bbc.co.uk/food/recipes/beefstewwithdumpling_87333',
			dateModified: null
		}
	];
} 
```

基本上我们得到了一个数组`meals`，但是其中只有一个条目——随机生成的条目。这个项目包含了我们想要在这个小应用程序中展示的所有数据。比如:

*   餐名(在`strMeal`下)
*   膳食分类(在`strCategory`下)
*   用餐图像(在`strMealThumb`下)
*   youtube 上的一段食谱视频(在`strYoutube`下)
*   配料和尺寸(在`strIngredientsX`和`strMeasureX`下)- X 代表第 n 种配料及其尺寸)。这有点尴尬，因为我希望这里有一个包含这些信息的数组，但他们选择将它作为对象道具添加。在井上...？需要注意的重要一点是，最多有 20 个成分/指标，尽管它们并没有全部填写——其中一些可能是空的，所以我们需要考虑到这一点。

现在我们有了按钮，我们将为`click`事件添加一个事件监听器。在内部，我们将向 API 发出一个请求:

```
get_meal_btn.addEventListener('click', () => {
	fetch('https://www.themealdb.com/api/json/v1/1/random.php')
		.then(res => res.json())
		.then(res => {
			createMeal(res.meals[0]);
		})
		.catch(e => {
			console.warn(e);
		});
}); 
```

我们使用[获取](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) API 来完成请求。我们只需传入我们想要向其发出 **GET** 请求的 API 的 url，然后我们将得到一个承诺。

一旦这个问题得到解决，我们就会有一个响应(`res`)。这个`res`还没有达到我们想要的状态，所以我们要对它调用`.json()`方法。最后我们有了这个美丽的物体。耶！？

如上所述，API 返回了`meals`数组，但其中只有一个条目。所以我们将把该项(在索引`0`处)传递给我们的`createMeal`函数，我们将在接下来定义它。

我将粘贴下面的整个代码块，稍后我们将详细介绍，请稍等片刻。？

```
const createMeal = meal => {
	const ingredients = [];

	// Get all ingredients from the object. Up to 20
	for (let i = 1; i <= 20; i++) {
		if (meal[`strIngredient${i}`]) {
			ingredients.push(
				`${meal[`strIngredient${i}`]} - ${meal[`strMeasure${i}`]}`
			);
		} else {
			// Stop if there are no more ingredients
			break;
		}
	}

	const newInnerHTML = `
		<div class="row">
			<div class="columns five">
				<img src="${meal.strMealThumb}" alt="Meal Image">
				${
					meal.strCategory
						? `<p><strong>Category:</strong> ${meal.strCategory}</p>`
						: ''
				}
				${meal.strArea ? `<p><strong>Area:</strong> ${meal.strArea}</p>` : ''}
				${
					meal.strTags
						? `<p><strong>Tags:</strong> ${meal.strTags
								.split(',')
								.join(', ')}</p>`
						: ''
				}
				<h5>Ingredients:</h5>
				<ul>
					${ingredients.map(ingredient => `<li>${ingredient}</li>`).join('')}
				</ul>
			</div>
			<div class="columns seven">
				<h4>${meal.strMeal}</h4>
				<p>${meal.strInstructions}</p>
			</div>
		</div>
		${
			meal.strYoutube
				? `
		<div class="row">
			<h5>Video Recipe</h5>
			<div class="videoWrapper">
				<iframe width="420" height="315"
				src="https://www.youtube.com/embed/${meal.strYoutube.slice(-11)}">
				</iframe>
			</div>
		</div>`
				: ''
		}
	`;

	meal_container.innerHTML = newInnerHTML;
}; 
```

基本上，整个函数的目的是获取 JSON 响应，解析它，并将其转换成 HTML 组件。为此，我们需要做几件事情，因为数据还没有完全按照我们想要的方式格式化。

首先，我们得到所有的**配料**和它们的**尺寸**。如上所述，最多有 20 种成分，但它们在对象中被分成各自的属性，如:`strIngredient1`、`strIngredient2`等...(我仍然不知道他们为什么这样做，但是...？).

因此，我们正在创建一个从`1`到`20`的`for`循环，并检查`meal`是否有相应的`ingredient` - `measure`对。如果是，我们就把它放入`ingredients`数组。如果没有更多的成分，我们将使用`break`条件停止 for 循环。

接下来，我们将创建`newInnerHTML`字符串，它将保存整个 HTML 标记。在其中，我们解析我们想要显示的剩余属性。

**注意**有些属性可能不可用。因此，我们使用[三元运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)来检查我们是否有显示相应标签的数据。如果我们没有它，那么我们将返回一个空字符串，页面上不会显示任何内容。`category`和`area`是这些类型属性的例子。

标签以逗号分隔的字符串形式出现，如:`'tag1,tag2,tag3'`。所以我们需要用逗号将它`split`化，用逗号和空格将它`join`化，这样看起来更好(`'tag1, tag2, tag3'` ❤️).)至少对我来说是这样。？

为了显示`ingredients`，我们对数组进行映射，并为每个成分/度量对创建一个`<li>`。最后，我们将数组重新连接起来，形成一个字符串。(这是你在反应堆里会做的事情，但没有 T2 那部分？).

还有一个 Youtube 视频*字符串*(可能)正在返回视频的 URL。但是为了在页面中嵌入视频，我们只需要提取视频 ID。为此，我们使用`.slice(-11)`来获取字符串的最后 11 个字符，因为这是 ID 隐藏的地方？。

最后，我们将整个`newInnerHTML`设置为`meal_container`的`innerHTML` - >，这将用所有这些信息填充 div！

每次我们按下`Get Meal`按钮，整个过程都会重复。

## CSS

最后一部分是做一点造型，对吧？？

对于 **CSS** 我想使用一些新的东西，所以我尝试了一下[骨骼库。如果您有一个小项目，并且不想被所有这些类淹没，这是很有用的，因为它只有几个处理一些基本样式(例如按钮)和响应部分的类。](http://getskeleton.com/)

```
@import url('https://fonts.googleapis.com/css?family=Muli&display=swap');

* {
	box-sizing: border-box;
}

body {
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	padding: 30px 0;
	min-height: calc(100vh - 60px);
}

img {
	max-width: 100%;
}

p {
	margin-bottom: 5px;
}

h3 {
	margin: 0;
}

h5 {
	margin: 10px 0;
}

li {
	margin-bottom: 0;
}

.meal {
	margin: 20px 0;
}

.text-center {
	text-align: center;
}

.videoWrapper {
	position: relative;
	padding-bottom: 56.25%;
	padding-top: 25px;
	height: 0;
}

.videoWrapper iframe {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
} 
```

你可以看到 CSS 非常简单。唯一值得一提的部分是`.videoWrapper` CSS 声明。这确保了 YouTube 嵌入是响应性的。(这是从 [CSS-Tricks](https://css-tricks.com/NetMag/FluidWidthVideo/Article-FluidWidthVideo.php) 得到的)-谢谢各位！？)

## 结论

瞧！我们完了！？

您现在应该知道如何使用公共 API 来获取一些数据，然后可以轻松地将这些数据插入到页面中！干得好！？

这是我为 [#100Days100Projects](https://florin-pop.com/blog/2019/09/100-days-100-projects) 挑战赛做的第一个项目。你可以通过点击[这里](https://florin-pop.com/blog/2019/09/100-days-100-projects)查看我还建了什么项目，挑战的规则是什么(如果你想加入的话)。

你可以阅读更多我关于 www.florin-pop.com 的文章。

编码快乐！？