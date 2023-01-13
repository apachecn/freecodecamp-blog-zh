# 从 AngularJS 到 React & Redux —如何迁移您的 web 应用

> 原文：<https://www.freecodecamp.org/news/angularjs-migration-to-react-redux-2d3bb3a7cc84/>

作者 Panagiotis Vrs

![1*BHCxD-xbmOPDcEMiTXsPxw](img/2befb8163c2c0b4c4da95f7c49997951.png)

# 从 AngularJS 到 React & Redux —如何迁移您的 web 应用

几天前，我想开始实现我在过去 4 个月中一直想做的事情:迁移一个 AngularJS 项目，以应对 Redux 状态管理。

我围绕这个话题做了一些研究。尽管有一些关于角度-反应偏移的文章，我还是迷路了。我也不知道你怎么能仅仅为了反应而重复这个项目。

随着 AngularJS 及其库从社区获得的支持越来越少，未来几年将有数百个项目不得不决定迁移到另一个框架。

我认为这可以更容易，所以我做了一些笔记，这样我就可以分享我的发现。

从 AngularJS 迁移开始做出反应并不是一件容易的事情。如果您也想开始使用 Redux 进行状态管理，会发生什么呢？这不一定是一场噩梦！

我的项目已经用 Webpack 切换到 ES6，所以这是很有帮助的一件事。所以我建议在进行这样的迁移之前，尽快开始使用 Webpack 和模块依赖。

### 我的需求

我想开始把这个项目从 AngularJS 转移到 React，这样我就可以开始使用现代 web 在速度方面提供的所有新功能，把代码分割成更小的组件，当然还有测试。

我不想撒谎——在这个项目中，我没有做足够的测试，或者说根本没有。在这次迁移之前的两个月，我开始围绕这个项目实施一些测试原则，这样我就可以跟踪一些任务。底线——建立测试和覆盖是非常困难的，并且最终需要一些时间来运行任务。

### 开始塑造它

我做的第一件事是安装所有必要的依赖项，如 react、react-dom 和 react-redux。你也可以在下面看到我正在使用的版本。

最酷的插件是 react2angular，我用它把所有的 react 组件翻译成 angular 组件。

然后为了开发，我们需要安装 babel dependencies 来拥有所有很酷的 ES6 特性。请记住，我已经有了巴别塔，我只是粘贴这个版本，以保持一致。

```
{
  "dependencies": {
    "ng-redux": "^3.4.0-beta.1",
    "prop-types": "^15.5.10",
    "react": "^15.5.4",
    "react-dom": "^15.5.4",
    "react-redux": "^5.0.5",
    "react2angular": "^1.1.3",
    "redux": "^3.6.0",
    "redux-devtools": "^3.4.0",
    "redux-thunk": "^2.2.0",
  },
  "devDependencies": {
    "babel-core": "^6.24.1",
    "babel-loader": "^6.4.1",
    "babel-preset-es2015": "^6.24.1",
    "babel-preset-react": "^6.24.1",
    "babel-preset-stage-2": "^6.24.1",
    "webpack": "^2.2.0",
    "webpack-dev-server": "^2.2.0",
  }
}
```

packages-sample.json

完成了吗？不错！

### 项目结构

这就是我想做的，在整个项目中使用这种结构。我在 Github 上的一个完整的 MERN 项目中看到了它，因为我在一个小项目中使用了它，以了解它如何变得非常简单和高效。拆分为每个页面(容器组件)和每个页面都有自己的组件。您也可以在名称组件之外有一个文件夹来保存所有的全局组件。如果代码示例没有意义，请参考此处；)

您还可以看到迁移完成后，MERN 项目是如何处理服务器端渲染的(这是 React 的另一个很棒的特性！)才能做成。

在这个结构中，我没有索引等文件，它只关注 react 组件。

```
├── app - holds all the components
│   ├── common - is not a module of the app. This holds utils for the react app.
│   │   └── utils
│   ├── ratings
│   │   ├── components 
│   │   │   ├── RatingItem 
│   │   │   │   ├── RatingItem.js 
│   │   │   │   └── RatingItem.less 
│   │   │   ├── RatingList 
│   │   │   │   ├── RatingList.js 
│   │   │   │   └── RatingList.less 
│   │   │   ├── RatingsPage.js 
│   │   │   ├── RatingsReducer.js 
│   │   │   └── RatingsActions.js
├── config - all config files for development and production
└── locales - locale files
```

Code Directory Structure

### 创建您的第一个组件

制作第一个组件相对容易。我希望你已经熟悉 react，所以我将直接插入代码。您可以根据自己的需要将代码拆分成任意多个组件。组件的示例如下所示。这就是我如何有规律地将一个 ul 的小(演示)组件与数据或 ul 的每个项目进行切换。

```
import React from "react";
import PropTypes from "prop-types";

function RatingsItem(props) {
    return (
        <div>
            {props.rating.text}
        </div>
    );
}

RatingsItem.propTypes = {
    rating: PropTypes.shape({
        id: PropTypes.string.isRequired,
        text: PropTypes.bool.isRequired,
        reported: PropTypes.bool.isRequired,
    }),
};

export default RatingsItem;
```

RatingsItem.js

这个小组件现在可以导入到我们的项目中了。在 AngularJS 应用程序中定义模块时，可以导入组件，并使用 react2angular dependency 将其更改为 AngularJS 指令/组件。

```
.component("ratingsItem", react2angular(RatingsItem))
```

现在在。html 如果使用 <ratings-item></ratings-item> 组件就会加载。恭喜你，这是你的第一个角度反应组件！

希望你没有迷路。你和我在一起吗？…很好。记住！

> 这是我的第一次写作，所以…请对你的评价温柔一点！

下一步是你想改变整个模块作出反应。这将是我们 react 应用程序中的一个容器组件。我把它们分成几页。在我们继续之前，我们需要开始使用 redux 动作和 reducers。这将有助于我们下一步将 redux 与 reducers 结合使用！

因此，为了对我们的容器组件实现一些 redux，首先您需要为这个 Rating 组件定义您的 reducer 和 action。你可以看到上面的 [app 结构](https://gist.github.com/panvourtsis/05a6b1118b348c792f64fb18c2e4a534)来查看我的项目中那些的命名和位置。

在这个主题中，我们将不解释动作和归约器，因为我认为你已经熟悉了 Redux 动作和归约器

我的减速器长这样。

```
import { ADD_RATINGS } from "./RatingsActions";

const initialState = { list: [] };

const RatingsReducer = (state = initialState, action) => {
    switch (action.type) {

    case ADD_RATINGS :
        return {
            ...state,
            list: action.ratings,
        };

    default:
        return state;
    }
};

/* Selectors */

// Get all ratings
export const getRatings = state => state.Ratings.list;

// Export Reducer
export default RatingsReducer;
```

RatingsReducer.js

动作看起来像这样。

```
import callApi from "../common/utils/apiCaller";

export const ADD_RATINGS = "ADD_RATINGS";

export function addRatings(ratings) {
    return {
        type: ADD_RATINGS,
        ratings,
    };
}

export function fetchRatings() {
    return (dispatch) => {
        return callApi("ratings").then(ratings => {
            dispatch(addRatings(ratings));
        });
    };
}
```

RatingsActions.js

简单！做两件事仅仅是为了测试是否有效。你可以用你自己的。

所以这将是我的评分页面。这就是我的页面的外观。您还可以在 div 内部传递一个“hi there ”,这样您就可以确保没有数据。同样只是为了测试。

```
import { connect } from "react-redux";
import React, { Component } from "react";
import PropTypes from "prop-types";
// Import Components
import RatingList from "./components/RatingList/RatingList";
// Import Actions
import { fetchRatings } from "./RatingsActions";
// import Reducer
import { getRatings } from "./RatingsReducer";
class RatingsPage extends Component {
    debugger;
    render() {
        return (
            <div>
                <RatingList ratings={this.props.ratings} />
            </div>
        );
    }
}
// Retrieve data from store as props
const mapStateToProps = (state) => {
    return {
        ratings: getRatings(state)
    }
};
const mapDispatchToProps = (dispatch) => {
    return {
        dispatch,
        fetchRatings: dispatch(fetchRatings()),
    };
}
RatingsPage.propTypes = {
    ratings: PropTypes.arrayOf(PropTypes.object),
    dispatch: PropTypes.func.isRequired,
};
export default connect(mapStateToProps, mapDispatchToProps)(RatingsPage);
```

RatingsPage.js

现在我们需要使用 ng-redux 来创建一个存储，并将这个存储传递给我们的组件。我们要做的是像在 react 中一样组合我们所有的应用程序缩减器，所以我为它创建了一个单独的文件，以便在将来删除 AngularJS 时使用。我称之为“减压器”,它看起来像这样。

它实际上是在这里定义所有的应用程序缩减器，以便将它们组合起来，在下一步中，我们将这些缩减器“馈送”到状态的 redux。你会看到我现在只有评级，但你可以(也必须)在这里填写任何新的组成部分。

```
import {combineReducers} from "redux";
import Ratings from "./ratings/RatingsReducer";

// Combine all reducers into one root reducer
export const RootReducer = combineReducers({
    Ratings
});
```

Reducers.js

现在我们需要导入 redux。在我们的主文件中，当我们定义整个应用程序时，我们组合导入 reduces、ngRedux、redux thunk 并将其作为模块依赖传递。

```
import ngRedux from "ng-redux";
import thunk from "redux-thunk";
import {RootReducer} from "./Reducers";
angular
    .module(
        "funkmartini",
        [
            ...,
            ngRedux,
            .....
        ]
     )
     .config(configApp)
     .run(runApp);
```

在您的应用程序配置中，我正在传递$ngReduxProvider 并创建我的商店。如果你想把 redux devtools 放到你的项目中，我也会传递它，如果不只是从 createStoreWith 函数中移除的话。

```
$ngReduxProvider.createStoreWith(RootReducer, [thunk], [$window.__REDUX_DEVTOOLS_EXTENSION__()]);
```

你现在都准备好了。使用控制器中的$ngRedux，您现在可以获得应用程序的存储！

### 总结到现在

所以我们做了一个小组件。然后我们重构了 Angular 的整个模块，创建了一个容器组件来代替它。现在我们想传递一些数据并使用 redux，这就是为什么我们创建 redux 存储来拥有一个初始状态，并使用文件中定义的 or reducers 来更新它。

### 将该存储传递给组件！！

因为我在 Angular 中使用了$stateProvider，我得到了类似下面的东西。我所做的是停止使用模板 html 页面的导入，直接使用我之前在 AngularJS 中定义为组件的 react 页面，就像我们之前所做的那样。所以我拿了这个。

```
function ratingsConfig($stateProvider) {
    $stateProvider
        .state("ratings", {
            url: "/ratings",
            views: {
                "main@settings": {
                    templateUrl: ratingsTemplate,
                    controller: "RatingsController"
                }
            }
        });
}
```

换成这样。

```
function ratingsConfig($stateProvider) {
    $stateProvider
        .state("ratings", {
            url: "/ratings",
            views: {
                "main@settings": {
                    template: `
                        <Ratings-Page store="store"></Ratings-Page>
                    `,
                    controller: "RatingsController"
                }
            }
        });
}
```

如你所见，我在 RatingsPage 属性中传递了一个存储变量。你想得没错，我只是使用 RatingsController 来获得并通过那个商店。所以控制器应该是这样的

```
function ratingsController($scope, $ngRedux) {
    $scope.store = $ngRedux;
}
```

这样，redux 可以识别出 props 中的存储实际上是 connect()函数中需要的存储。

### 结论

将 AngularJS 迁移到 React/redux 是困难的，可能需要时间，但不一定是一场噩梦。一旦你完成了项目的一些基本配置，它可以很容易地扩展，慢慢地成为一个 React 项目。不得不尽可能接近普通的 Javascript 真的很棒。虽然迁移需要一些时间，但我认为从长远来看是值得的。我在本文中跳过的建议是，你也可以开始使用一些林挺(jslint，eslint-Airbnb ),并且不要忘记测试新的组件！

玩得开心，让我知道进展如何！

### 资源

[**我们从 AngularJS 迁移 100k 行代码到 React 的旅程(第一章)**](https://tech.small-improvements.com/2017/01/25/how-to-migrate-an-angularjs-1-app-to-react/)
[*这篇帖子总结了我们从 AngularJS 迁移到 React/ Redux 的策略、模式和经验教训。*](https://tech.small-improvements.com/2017/01/25/how-to-migrate-an-angularjs-1-app-to-react/)
[tech.small-improvements.com](https://tech.small-improvements.com/2017/01/25/how-to-migrate-an-angularjs-1-app-to-react/)

[**迁移到 Redux Redux**](http://redux.js.org/docs/recipes/MigratingToRedux.html)
[*创建一个名为 createflusstore(reducer)的函数，该函数从一个…*](http://redux.js.org/docs/recipes/MigratingToRedux.html)
[redux.js.org](http://redux.js.org/docs/recipes/MigratingToRedux.html)

[**hash node/mern-starter**](https://github.com/Hashnode/mern-starter)
[*mern-starter-stack*](https://github.com/Hashnode/mern-starter)
[github.com](https://github.com/Hashnode/mern-starter)