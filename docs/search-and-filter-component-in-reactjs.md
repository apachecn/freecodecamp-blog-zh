# 如何在 React 中搜索和过滤组件

> 原文：<https://www.freecodecamp.org/news/search-and-filter-component-in-reactjs/>

如果你正在构建一个 React 应用，你希望你的用户能够搜索并得到准确的结果。如果你从一个 API 中获得大量的条目，那么你需要为用户创建一种方法，使他们能够容易地找到各种各样的条目。

对于本教程，我们将使用一个[前端导师的免费高级 API 项目](https://www.frontendmentor.io/challenges/rest-countries-api-with-color-theme-switcher-5cacc469fec04111f7b848ca)作为例子。

## 目录

1.  入门指南
2.  如何设置 React
3.  如何获取数据
4.  如何在 API 中搜索项目
5.  如何根据地区筛选项目

# 入门指南

在本教程中，我们将使用由 [Apilayer](https://apilayer.com/) 提供的免费 [REST 国家 API](https://restcountries.eu/) 。

基本上，我们将从 API 端点`https://restcountries.eu/rest/v2/all`获取数据，并以用户可读的形式显示数据。

然后，我们将为用户提供一种方法，通过他们的名字和首都轻松搜索特定的国家。‌‌是一个特定国家回应的例子:

```
 "name": "Colombia",
        "topLevelDomain": [".co"],
        "alpha2Code": "CO",
        "alpha3Code": "COL",
        "callingCodes": ["57"],
        "capital": "Bogotá",
        "altSpellings": ["CO", "Republic of Colombia", "República de Colombia"],
        "region": "Americas",
        "subregion": "South America",
        "population": 48759958,
        "latlng": [4.0, -72.0],
        "demonym": "Colombian",
        "area": 1141748.0,
        "gini": 55.9,
        "timezones": ["UTC-05:00"],
        "borders": ["BRA", "ECU", "PAN", "PER", "VEN"],
        "nativeName": "Colombia",
        "numericCode": "170",
        "currencies": [{
            "code": "COP",
            "name": "Colombian peso",
            "symbol": "$"
        }],
        "languages": [{
            "iso639_1": "es",
            "iso639_2": "spa",
            "name": "Spanish",
            "nativeName": "Español"
        }],
        "translations": {
            "de": "Kolumbien",
            "es": "Colombia",
            "fr": "Colombie",
            "ja": "コロンビア",
            "it": "Colombia",
            "br": "Colômbia",
            "pt": "Colômbia"
        },
        "flag": "https://restcountries.eu/data/col.svg",
        "regionalBlocs": [{
            "acronym": "PA",
            "name": "Pacific Alliance",
            "otherAcronyms": [],
            "otherNames": ["Alianza del Pacífico"]
        }, {
            "acronym": "USAN",
            "name": "Union of South American Nations",
            "otherAcronyms": ["UNASUR", "UNASUL", "UZAN"],
            "otherNames": ["Unión de Naciones Suramericanas", "União de Nações Sul-Americanas", "Unie van Zuid-Amerikaanse Naties", "South American Union"]
        }],
        "cioc": "COL"
    }] 
```

‌‌By 本教程结束时，希望你将学习如何通过一个 API 搜索，并只返回查询结果与反应。

# 如何设置 React

我们将使用`create-react-app`来设置我们的项目，因为它提供了一个不需要任何配置的现代构建设置。‌‌

要设置 React，请启动您的终端(可以是您的操作系统提供的终端，也可以使用像 VS Code 这样的编辑器)并运行以下命令:

```
npx create-react-app my-app 
cd my-app 
npm start
```

如果你不确定如何正确设置一个`create-react-app`项目，你可以在 [create-react-app-dev](https://create-react-app.dev/docs/getting-started/) 查阅官方指南。‌‌

对于我们的例子，为了在本教程中显示实时结果，我们将使用 [Codepen](https://codepen.io/) 来设置我们的项目。你可以通过使用由 [Lathryx](https://codepen.io/MrMaster) 开发的 Codepen 模板来完成这项工作:

[https://codepen.io/MrMaster/embed/preview/oNYWNjr?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=oNYWNjr](https://codepen.io/MrMaster/embed/preview/oNYWNjr?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=oNYWNjr)

‌‌and:好了，我们已经在 Codepen 中设置了 React。

## 如何从我们的 API 端点获取数据

现在我们已经成功地设置了 React 项目，是时候从我们的 API 获取数据了。React 中获取数据的方法有很多，但最流行的两种是 **Axios** (一种基于 promise 的 HTTP 客户端)和 **Fetch API** (一种浏览器内置的 web API)。‌‌

我们将使用浏览器和 Ajax 提供的`Fetch API`从 API 端点获取数据。‌‌这里是一个使用来自 Ajax 的钩子和 React 的 API 的例子:

```
function MyComponent() {
      const [error, setError] = useState(null);
      const [isLoaded, setIsLoaded] = useState(false);
      const [items, setItems] = useState([]);

      // Note: the empty deps array [] means
      // this useEffect will run once
      // similar to componentDidMount()
      useEffect(() => {
        fetch("https://api.example.com/items")
          .then(res => res.json())
          .then(
            (result) => {
              setIsLoaded(true);
              setItems(result);
            },
            // Note: it's important to handle errors here
            // instead of a catch() block so that we don't swallow
            // exceptions from actual bugs in components.
            (error) => {
              setIsLoaded(true);
              setError(error);
            }
          )
      }, [])

      if (error) {
        return <div>Error: {error.message}</div>;
      } else if (!isLoaded) {
        return <div>Loading...</div>;
      } else {
        return (
          <ul>
            {items.map(item => (
              <li key={item.id}>
                {item.name} {item.price}
              </li>
            ))}
          </ul>
        );
      }
    }
```

这将从我们的端点`line 10`中获取数据，然后在获取数据时使用`setState`来更新我们的组件。

从`line 27`开始，如果从我们的 API 获取数据失败，我们会显示一条错误消息。如果没有失败，我们将数据显示为列表。

如果你对 React 列表不熟悉，我建议你看一下这个指南 [React 列表和键](https://reactjs.org/docs/lists-and-keys.html)。

现在让我们使用这段代码从 REST COUNTRIES API 中获取并显示数据。

从上面的示例代码中，我们希望将`import useState from React`改为`line 10`:

`fetch("https://restcountries.eu/rest/v2/all")` ‌

当我们将所有这些放在一起时，我们有:

```
import { useState, useEffect } from "https://cdn.skypack.dev/react";

    // Note: the empty deps array [] means
    // this useEffect will run once
    function App() {
        const [error, setError] = useState(null);
        const [isLoaded, setIsLoaded] = useState(false);
        const [items, setItems] = useState([]);

        useEffect(() => {
            fetch("https://restcountries.eu/rest/v2/all")
                .then((res) => res.json())
                .then(
                    (result) => {
                        setIsLoaded(true);
                        setItems(result);
                    },
                    // Note: it's important to handle errors here
                    // instead of a catch() block so that we don't swallow
                    // exceptions from actual bugs in components.
                    (error) => {
                        setIsLoaded(true);
                        setError(error);
                    }
                );
        }, []);
```

**注:**我们正在从`"https://cdn.skypack.dev/react";`进口`useState`和`useEffect`。这是因为我们使用 CDN 在 Codepen 中导入 React。如果您在本地设置 React，那么您应该使用`import { useState, useEffect } from "react";`。

然后，我们希望将收到的数据显示为国家列表。完成这项工作的最终代码如下所示:

```
 // Note: the empty deps array [] means
    // this useEffect will run once
    function App() {
        const [error, setError] = useState(null);
        const [isLoaded, setIsLoaded] = useState(false);
        const [items, setItems] = useState([]);

        useEffect(() => {
            fetch("https://restcountries.eu/rest/v2/all")
                .then((res) => res.json())
                .then(
                    (result) => {
                        setIsLoaded(true);
                        setItems(result);
                    },
                    // Note: it's important to handle errors here
                    // instead of a catch() block so that we don't swallow
                    // exceptions from actual bugs in components.
                    (error) => {
                        setIsLoaded(true);
                        setError(error);
                    }
                );
        }, []);

        if (error) {
            return <>{error.message}</>;
        } else if (!isLoaded) {
            return <>loading...</>;
        } else {
            return (
                /* here we map over the element and display each item as a card  */
                <div className="wrapper">
                    <ul className="card-grid">
                        {items.map((item) => (
                            <li>
                                <article className="card" key={item.callingCodes}>
                                    <div className="card-image">
                                        <img src={item.flag} alt={item.name} />
                                    </div>
                                    <div className="card-content">
                                        <h2 className="card-name">{item.name}</h2>
                                        <ol className="card-list">
                                            <li>
                                                population:{" "}
                                                <span>{item.population}</span>
                                            </li>
                                            <li>
                                                Region: <span>{item.region}</span>
                                            </li>
                                            <li>
                                                Capital: <span>{item.capital}</span>
                                            </li>
                                        </ol>
                                    </div>
                                </article>
                            </li>
                        ))}
                    </ul>
                </div>
            );
        }
    }

    ReactDOM.render(<App />, document.getElementById("root"));
```

这是 Codepen 的现场预览:

[https://codepen.io/Spruce_khalifa/embed/preview/ZELeWoN?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=ZELeWoN](https://codepen.io/Spruce_khalifa/embed/preview/ZELeWoN?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=ZELeWoN)

既然我们已经成功地获取并显示了来自 REST COUNTRIES API 的数据，现在我们可以集中精力搜索正在显示的国家。

但是在我们这样做之前，让我们用 CSS 样式化上面的例子(因为当像这样显示时它看起来很丑)。

当我们将 CSS 添加到上面的例子中时，我们得到了类似下面的例子:

[https://codepen.io/Spruce_khalifa/embed/preview/zYNZBBB?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=zYNZBBB](https://codepen.io/Spruce_khalifa/embed/preview/zYNZBBB?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=zYNZBBB)

虽然我们添加的 CSS 并不完美，但它显示国家的方式比前者更整洁，我希望你同意？

## 如何构建搜索组件

在我们的 APP 函数中，我们使用`useState()`钩子将查询`q`设置为空字符串。我们还有`setQ`，我们将使用它来绑定搜索表单中的值。‌‌

在`line 13`上，我们使用`useState`来定义我们希望能够从 API 中搜索的默认值数组。这意味着我们希望能够只通过`Capital`和`name`来搜索任何国家。您可以根据自己的喜好让这个数组变长。

```
 const [error, setError] = useState(null);
        const [isLoaded, setIsLoaded] = useState(false);
        const [items, setItems] = useState([]);

        //     set search query to empty string
        const [q, setQ] = useState("");
        //     set search parameters
        //     we only what to search countries by capital and name
        //     this list can be longer if you want
        //     you can search countries even by their population
        // just add it to this array
        const [searchParam] = useState(["capital", "name"]);

        useEffect(() => {
            // our fetch codes
        }, []);

     }
```

在我们的返回函数中，我们将创建搜索表单，我们的代码现在看起来像这样:

```
 return <>{error.message}</>;
        } else if (!isLoaded) {
            return <>loading...</>;
        } else {
            return (
                <div className="wrapper">
                    <div className="search-wrapper">
                        <label htmlFor="search-form">
                            <input
                                type="search"
                                name="search-form"
                                id="search-form"
                                className="search-input"
                                placeholder="Search for..."
                                value={q}
                                /*
                                // set the value of our useState q
                                //  anytime the user types in the search box
                                */
                                onChange={(e) => setQ(e.target.value)}
                            />
                            <span className="sr-only">Search countries here</span>
                        </label>
                    </div>
                    <ul className="card-grid">
                        {items.map((item) => (
                            <li>
                                <article className="card" key={item.callingCodes}>
                                    <div className="card-image">
                                        <img src={item.flag} alt={item.name} />
                                    </div>
                                    <div className="card-content">
                                        <h2 className="card-name">{item.name}</h2>
                                        <ol className="card-list">
                                            <li>
                                                population:{" "}
                                                <span>{item.population}</span>
                                            </li>
                                            <li>
                                                Region: <span>{item.region}</span>
                                            </li>
                                            <li>
                                                Capital: <span>{item.capital}</span>
                                            </li>
                                        </ol>
                                    </div>
                                </article>
                            </li>
                        ))}
                    </ul>
                </div>
            );
        }
    }

    ReactDOM.render(<App />, document.getElementById("root")); 
```

现在我们将创建一个函数来处理我们的搜索，并将它放在我们的返回之上(上面的代码)。

```
 return items.filter((item) => {
                return searchParam.some((newItem) => {
                    return (
                        item[newItem]
                            .toString()
                            .toLowerCase()
                            .indexOf(q.toLowerCase()) > -1
                    );
                });
            });
        }
```

如果`indexOF()`是`> -1`，这个函数接收我们获取的条目，并返回与我们的`searchParam`数组中的任何内容匹配的所有条目。

既然函数已经设置好了，为了使用它，我们将使用搜索函数包装返回的数据。

`{serach(items).map((item) => ( <li> // card goes here </li> ))}` ‌

现在，存储在我们的`useState()`中的数据将在传递给列表项之前在我们的搜索函数中进行过滤，从而只返回与我们的查询匹配的项。

这里是代码放在一起，并在 Codepen 上的现场预览。尝试使用下面的搜索表单，按名称或首都搜索任何国家。

[https://codepen.io/Spruce_khalifa/embed/preview/wvgJWdO?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=wvgJWdO](https://codepen.io/Spruce_khalifa/embed/preview/wvgJWdO?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=wvgJWdO)

## 如何按地区筛选国家

现在我们可以更进一步，按地区过滤国家。假设我们不想显示所有国家，我们只想显示并能够搜索仅在`Africa`或`Asia`中的国家。你可以通过使用 React 中的`useState()`钩子来实现。

### 区域:

1.  非洲
2.  美国
3.  亚洲
4.  欧洲
5.  大洋洲

现在我们知道了我们的区域，让我们创建过滤器组件。首先，我们像这样设置滤波器的`useState`:

`const [filterParam, setFilterParam] = useState(["All"]);` ‌

请注意，我们故意将 useState 默认设置为`ALL`，因为如果没有指定区域，我们希望能够显示和搜索所有国家。

```
 <select
    /*
    // here we create a basic select input
    // we set the value to the selected value
    // and update the setFilterParam() state every time onChange is called
    */
      onChange={(e) => {
      setFilterParam(e.target.value);
       }}
       className="custom-select"
       aria-label="Filter Countries By Region">
        <option value="All">Filter By Region</option>
        <option value="Africa">Africa</option>
        <option value="Americas">America</option>
        <option value="Asia">Asia</option>
        <option value="Europe">Europe</option>
        <option value="Oceania">Oceania</option>
        </select>
        <span className="focus"></span>
        </div> 
```

现在我们已经创建了过滤器，剩下的就是修改`search function`。基本上，我们检查输入的地区，只返回拥有该地区的国家:

```
function search(items) {
       return items.filter((item) => {
    /*
    // in here we check if our region is equal to our c state
    // if it's equal to then only return the items that match
    // if not return All the countries
    */
       if (item.region == filterParam) {
           return searchParam.some((newItem) => {
             return (
               item[newItem]
                   .toString()
                   .toLowerCase()
                   .indexOf(q.toLowerCase()) > -1
                        );
                    });
                } else if (filterParam == "All") {
                    return searchParam.some((newItem) => {
                        return (
                            item[newItem]
                                .toString()
                                .toLowerCase()
                                .indexOf(q.toLowerCase()) > -1
                        );
                    });
                }
            });
        }
```

你可以在 Codepen 上找到完整的代码和实时预览。尝试过滤国家并观察结果。

[https://codepen.io/Spruce_khalifa/embed/preview/BapWzPe?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BapWzPe](https://codepen.io/Spruce_khalifa/embed/preview/BapWzPe?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=BapWzPe)

一旦我们添加了一些 CSS，我们现在可以查看 React 应用程序的最终预览。

[https://codepen.io/Spruce_khalifa/embed/preview/GRrWjmR?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=GRrWjmR](https://codepen.io/Spruce_khalifa/embed/preview/GRrWjmR?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=GRrWjmR)

# 包扎

当您处理需要向用户显示的大量数据时，搜索和过滤功能可以帮助用户快速导航和找到重要信息。

有问题可以联系我，我很乐意聊天。

你可以在 [earthly vercel app](https://earthly.vercel.app) 找到这个项目的完整预览。你可以在 [Twitter @sprucekhalifa](https://twitter.com/sprucekhalifa) 上关注我。