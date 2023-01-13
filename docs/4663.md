# 使用普通 JavaScript 进行即时搜索

> 原文：<https://www.freecodecamp.org/news/instant-search-with-vanilla-javascript/>

*原贴于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/06/vanilla-javascript-instant-search/)*

[每周编码挑战](https://florin-pop.com/blog/2019/03/weekly-coding-challenge/)第 15 周的**主题**是:

## 即时搜索

我们生活在一个快节奏的世界，我们希望一切都快，包括搜索结果，这就是为什么即时搜索功能几乎成为一个“必须拥有”的功能，而不是一个“最好拥有”的功能。

在本文中，我们将构建这个[特性](https://codepen.io/FlorinPop17/full/qzNxGa/)，但是只使用普通的 JavaScript :smiley:。

下面是我们将在本文中构建的内容的实时预览。让我们搜索世界各国，看看它们的人口和国旗:

[https://codepen.io/FlorinPop17/embed/preview/qzNxGa?height=300&slug-hash=qzNxGa&default-tabs=js,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/qzNxGa?height=300&slug-hash=qzNxGa&default-tabs=js,result&host=https://codepen.io)

Weekly Coding Challenge #15 - Instant Search with Vanilla JavaScript

**注意**:你可以使用 React、Vue、Angular 或任何其他框架/库来创建这个功能，尽管用普通的 JavaScript 来创建更有趣。？

## HTML

我们的 HTML 中需要两件东西:

1.  一个`search`字段
2.  一个我们将显示搜索结果的 div

```
<input type="text" id="search" placeholder="Search for a Country" />
<div id="results"></div> 
```

通常我们接下来会进入样式部分，但是在这种情况下，考虑到我们还没有完整的标记(您马上就会看到)，我们将首先进入 JS 部分。？

## JavaScript

让我们在实际键入任何代码之前制定一个计划。我们需要做的事情是:

1.  收集世界上所有国家的列表
2.  显示国家列表
3.  基于搜索查询更新结果

很棒，是吧？？

### 第一步-获取数据

我发现了一个不错的 API，我们可以用它来获取我们需要的国家列表。就是: [RestCountries.eu](https://restcountries.eu/) 。

我们将使用内置的[获取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 来检索所有国家，并将它们存储在一个变量中:`countries`。

```
let countries;

const fetchCountries = async () => {
    countries = await fetch(
        'https://restcountries.eu/rest/v2/all?fields=name;population;flag'
    ).then(res => res.json());
}; 
```

如你所见，我们创建了一个异步函数；你可以在这里阅读更多关于这个[的内容。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

另外，请注意，我们只需要 API 中的 3 个字段(名称、人口和标志)，因此这是我们通过设置`fields`查询参数从 API 中获得的内容。

### 第二步-显示数据

现在，我们需要创建列表的结构来显示数据，为此我们将在一个函数中创建我们需要的所有元素(`ul`、`li`、标题等)，并将它们插入到我们在 HTML 中声明的`results` div 中。

我们还将在这里调用我们的`fetchCountries`函数，因为我们想要循环遍历各个国家，以便创建相应的元素:

```
const results = document.getElementById('results');

const showCountries = async () => {
    // getting the data
    await fetchCountries();

    const ul = document.createElement('ul');
    ul.classList.add('countries');

    countries.forEach(country => {
        // creating the structure
        const li = document.createElement('li');
        li.classList.add('country-item');

        const country_flag = document.createElement('img');
        // Setting the image source
        country_flag.src = country.flag;
        country_flag.classList.add('country-flag');

        const country_name = document.createElement('h3');
        country_name.innerText = country.name;
        country_name.classList.add('country-name');

        const country_info = document.createElement('div');
        country_info.classList.add('country-info');

        const country_population = document.createElement('h2');
        country_population.innerText = numberWithCommas(country.population);
        country_population.classList.add('country-population');

        const country_popupation_text = document.createElement('h5');
        country_popupation_text.innerText = 'Population';
        country_popupation_text.classList.add('country-population-text');

        country_info.appendChild(country_population);
        country_info.appendChild(country_popupation_text);

        li.appendChild(country_flag);
        li.appendChild(country_name);
        li.appendChild(country_info);

        ul.appendChild(li);
    });

    results.appendChild(ul);
};

// display initial countries
showCountries();

// From StackOverflow https://stackoverflow.com/questions/2901102/how-to-print-a-number-with-commas-as-thousands-separators-in-javascript
function numberWithCommas(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
} 
```

上面有“一点点”代码，我们来分解一下。？

首先，我们有在`forEach`循环中创建的带有`li`的列表(`ul`)。

所有的人都有三个孩子:

1.  旗帜- `img`
2.  国家名称标题- `h3`
3.  一个`div`，它包含:(a)数字`population`-`h2`-和(b)文本`'Population'`-`h5`

我们以这种方式组织它们，因为在 CSS 中使用 **flexbox** 来对齐一切会更容易。

除此之外，我们为每个元素添加了一个`class`，因为我们想在 CSS 中单独设置它们的样式，然后我们在`forEach`函数的末尾使用`appendChild`将这些元素添加到 DOM 中。

最后，我们有一个来自 StackOverflow 的`numberWithComma`函数，它将添加一个逗号作为数字`population`的千位分隔符。

### 步骤 3 -基于搜索查询更新视图

在这最后一步中，我们将通过在`input`上附加一个`eventListener`来获取搜索查询，并且我们将修改我们的`showCountries`函数，这样它将`filter`掉我们不想显示的值。让我们看看如何做到这一点:

```
const search_input = document.getElementById('search');

let search_term = '';

search_input.addEventListener('input', e => {
    // saving the input value
    search_term = e.target.value;

    // re-displaying countries based on the new search_term
    showCountries();
});

const showCountries = async () => {
    // clear the results
    results.innerHTML = '';

    // see code above at Step 2.

    countries
        .filter(country =>
            country.name.toLowerCase().includes(search_term.toLowerCase())
        )
        .forEach(country => {
            // see code above at Step 2.
        });

    // see code above at Step 2.
}; 
```

如你所见，我们在`showCountries`函数中添加了两个新东西:

1.  我们通过将`innerHTML`设置为空字符串来清除之前的`results`
2.  我们通过检查输入的`search_term`是否是国家`name`中的`included`来过滤`countries`,并且我们也转换这两个值`toLowerCase`,因为用户可能输入大写字母，并且我们仍然希望显示相应的国家

## 整个 JS 代码

如果您想复制完整的 JS 代码，可以在下面找到它:

```
const search_input = document.getElementById('search');
const results = document.getElementById('results');

let search_term = '';
let countries;

const fetchCountries = async () => {
    countries = await fetch(
        'https://restcountries.eu/rest/v2/all?fields=name;population;flag'
    ).then(res => res.json());
};

const showCountries = async () => {
    results.innerHTML = '';

    await fetchCountries();

    const ul = document.createElement('ul');
    ul.classList.add('countries');

    countries
        .filter(country =>
            country.name.toLowerCase().includes(search_term.toLowerCase())
        )
        .forEach(country => {
            const li = document.createElement('li');
            li.classList.add('country-item');

            const country_flag = document.createElement('img');
            country_flag.src = country.flag;
            country_flag.classList.add('country-flag');

            const country_name = document.createElement('h3');
            country_name.innerText = country.name;
            country_name.classList.add('country-name');

            const country_info = document.createElement('div');
            country_info.classList.add('country-info');

            const country_population = document.createElement('h2');
            country_population.innerText = numberWithCommas(country.population);
            country_population.classList.add('country-population');

            const country_popupation_text = document.createElement('h5');
            country_popupation_text.innerText = 'Population';
            country_popupation_text.classList.add('country-population-text');

            country_info.appendChild(country_population);
            country_info.appendChild(country_popupation_text);

            li.appendChild(country_flag);
            li.appendChild(country_name);
            li.appendChild(country_info);

            ul.appendChild(li);
        });

    results.appendChild(ul);
};

showCountries();

search_input.addEventListener('input', e => {
    search_term = e.target.value;
    showCountries();
});

// From StackOverflow https://stackoverflow.com/questions/2901102/how-to-print-a-number-with-commas-as-thousands-separators-in-javascript
function numberWithCommas(x) {
    return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
} 
```

## CSS

最后，让我们给我们的小应用程序添加一些样式:

```
@import url('https://fonts.googleapis.com/css?family=Roboto:300,500&display=swap');

* {
    box-sizing: border-box;
}

body {
    background-image: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    font-family: 'Roboto', sans-serif;

    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;

    min-height: 100vh;
}

.countries {
    padding: 0;
    list-style-type: none;
    width: 480px;
}

.country-item {
    background-color: #fff;
    border-radius: 3px;
    box-shadow: 0 2px 3px rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 10px;
    margin: 10px 0;
}

.country-item:hover {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
}

.country-flag {
    width: 40px;
}

.country-name {
    flex: 2;
    font-weight: normal;
    letter-spacing: 0.5px;
    margin: 0 5px;
    text-align: center;
}

.country-info {
    border-left: 1px solid #aaa;
    color: #777;
    padding: 0 15px;
    flex: 1;
}

.country-population {
    font-weight: 300;
    line-height: 24px;
    margin: 0;
    margin-bottom: 12px;
}

.country-population-text {
    font-weight: 300;
    letter-spacing: 1px;
    margin: 0;
    text-transform: uppercase;
}

input {
    font-family: 'Roboto', sans-serif;
    border-radius: 3px;
    border: 1px solid #ddd;
    padding: 15px;
    width: 480px;
} 
```

因为这不是什么新奇的东西，所以我不打算详细介绍 CSS，但是如果你有任何关于它的问题，请随时联系我，我很乐意回答你的问题！？

## 结论

如上所述，这个小应用程序可能可以用 React、Vue 或 Angular 来完成，如果你想提交，你可以自由地这样做，但我想用 Vanilla JS 来玩，这对我来说是一种有趣的体验！？

像往常一样，确保你分享你将要创造的东西！

编码快乐！？