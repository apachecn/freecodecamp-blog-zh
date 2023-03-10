# 如何使用 React、TypeScript 和 React 测试库创建出色的用户体验

> 原文：<https://www.freecodecamp.org/news/ux-studies-with-react-typescript-and-testing-library/>

不管我知道多少，我总是愿意学习。作为一名软件工程师，我的求知欲增加了很多。我知道我每天都有很多东西要学。

但是在我能学习更多之前，我想掌握基本原理。为了让自己成为一名更好的开发人员，我想更多地了解如何创造伟大的产品体验。

这篇文章是我试图说明我建立的概念证明(PoC)来尝试一些想法。

我对这个项目有一些想法。它需要:

*   使用高质量的软件
*   提供出色的用户体验

当我说高质量软件时，这可能意味着许多不同的事情。但是我想集中讨论三个部分:

*   干净的代码:努力编写易于阅读和维护的可读代码。将功能和组件的责任分开。
*   良好的测试覆盖率:这实际上与覆盖率无关。它是关于在不知道太多实现细节的情况下覆盖组件行为的重要部分的测试。
*   一致的状态管理:我想构建一个软件，让应用程序拥有一致的数据。可预测性很重要。

用户体验是本次概念验证的主要焦点。软件和技术是为用户提供良好体验的基础。

为了使状态一致，我需要一个类型系统。所以我选择了 TypeScript。这是我第一次使用带有 React 的 Typescript。这个项目还允许我构建定制的钩子，并正确地测试它。

## 设置项目

我遇到了这个名为 [tsdx](https://github.com/jaredpalmer/tsdx) 的库，它为您设置了所有的 Typescript 配置。它主要用于构建包。由于这是一个简单的兼职项目，我不介意尝试一下。

安装后，我选择了 React 模板，并准备编码。但是在有趣的部分之前，我也想设置测试配置。我使用了 [React 测试库](https://github.com/testing-library/react-testing-library)作为主库，同时使用了 [jest-dom](https://github.com/testing-library/jest-dom) 来提供一些令人敬畏的定制方法(我非常喜欢`toBeInTheDocument`匹配器)。

安装好所有这些之后，我通过添加一个新的`jest.config.js`重写了 jest 配置:

```
module.exports = {
  verbose: true,
  setupFilesAfterEnv: ["./setupTests.ts"],
}; 
```

以及进口我所需要的一切。

```
import "@testing-library/jest-dom"; 
```

在这种情况下，我只需要导入`jest-dom`库。这样，我不需要在我的测试文件中导入这个包。现在它开箱即用了。

为了测试这个安装和配置，我构建了一个简单的组件:

```
export const Thing = () => <h1>I'm TK</h1>; 
```

在我的测试中，我想渲染它，看看它是否在 DOM 中。

```
import React from 'react';
import { render } from '@testing-library/react';
import { Thing } from '../index';

describe('Thing', () => {
  it('renders the correct text in the document', () => {
    const { getByText } = render(<Thing />);

    expect(getByText("I'm TK")).toBeInTheDocument();
  });
}); 
```

现在我们为下一步做好了准备。

## 配置路线

在这里，我想现在只有两条路线。主页和搜索页面——尽管我不会对主页做任何事情。

对于这个项目，我使用`react-router-dom`库来处理所有与路由器相关的事情。它简单、容易、有趣。

安装完成后，我在`app.typescript`中添加了路由器组件。

```
import { BrowserRouter as Router, Switch, Route } from 'react-router-dom';

export const App = () => (
  <Router>
    <Switch>
      <Route path="/search">
        <h1>It's the search!</h1>
      </Route>
      <Route path="/">
        <h1>It's Home</h1>
      </Route>
    </Switch>
  </Router>
); 
```

现在如果我们输入`localhost:1234`，我们会看到标题`It's Home`。去`localhost:1234/search`，我们会看到正文`It's the search!`。

在我们继续开始实现我们的搜索页面之前，我想构建一个简单的菜单来在主页和搜索页面之间切换，而不操纵 URL。对于这个项目，我正在使用 [Material UI](https://material-ui.com/) 来构建 UI 基础。

目前，我们只是在安装`@material-ui/core`。

为了建立菜单，我们有打开菜单选项的按钮。在这种情况下，它们是“主页”和“搜索”选项。

但是为了构建一个更好的组件抽象，我更喜欢隐藏菜单项的内容(链接和标签),让`Menu`组件接收这些数据作为道具。这样，菜单就不知道这些项目了，它只会遍历项目列表并呈现它们。

看起来是这样的:

```
import React, { Fragment, useState, MouseEvent } from 'react';
import { Link } from 'react-router-dom';
import Button from '@material-ui/core/Button';
import MuiMenu from '@material-ui/core/Menu';
import MuiMenuItem from '@material-ui/core/MenuItem';

import { MenuItem } from '../../types/MenuItem';

type MenuPropsType = { menuItems: MenuItem[] };

export const Menu = ({ menuItems }: MenuPropsType) => {
  const [anchorEl, setAnchorEl] = useState<null | HTMLElement>(null);

  const handleClick = (event: MouseEvent<HTMLButtonElement>): void => {
    setAnchorEl(event.currentTarget);
  };

  const handleClose = (): void => {
    setAnchorEl(null);
  };

  return (
    <Fragment>
      <Button aria-controls="menu" aria-haspopup="true" onClick={handleClick}>
        Open Menu
      </Button>
      <MuiMenu
        id="simple-menu"
        anchorEl={anchorEl}
        keepMounted
        open={Boolean(anchorEl)}
        onClose={handleClose}
      >
        {menuItems.map((item: MenuItem) => (
          <Link to={item.linkTo} onClick={handleClose} key={item.key}>
            <MuiMenuItem>{item.label}</MuiMenuItem>
          </Link>
        ))}
      </MuiMenu>
    </Fragment>
  );
};

export default Menu; 
```

不要慌！我知道这是一个巨大的代码块，但它非常简单。`Fragment`包裹`Button`和`MuiMenu` ( `Mui`代表材质 UI。我需要重命名组件，因为我构建的组件也叫做 menu)。

它接收`menuItems`作为道具，并通过它映射来构建由`Link`组件包装的菜单项。Link 是从 react-router 到给定 URL 的链接的组件。

菜单行为也很简单:我们将`handleClick`函数绑定到按钮的`onClick`。这样，我们就可以在按钮被触发时(或者如果你愿意的话，点击一下)改变`anchorEl`。`anchorEl`只是一个组件状态，代表 Mui 菜单元素打开菜单开关。所以它会打开菜单项，让用户从中选择一个。

现在，我们如何使用这个组件？

```
import { Menu } from './components/Menu';
import { MenuItem } from './types/MenuItem';

const menuItems: MenuItem[] = [
  {
    linkTo: '/',
    label: 'Home',
    key: 'link-to-home',
  },
  {
    linkTo: '/search',
    label: 'Search',
    key: 'link-to-search',
  },
];

<Menu menuItems={menuItems} /> 
```

`menuItems`是一个对象列表。该对象具有`Menu`组件所期望的正确契约。类型`MenuItem`确保契约是正确的。它只是一份打字稿`type`:

```
export type MenuItem = {
  linkTo: string;
  label: string;
  key: string;
}; 
```

## 搜索

现在，我们已经准备好构建包含所有产品和良好体验的搜索页面。但是在构建产品列表之前，我想创建一个 fetch 函数来处理对产品的请求。因为我还没有产品的 API，所以我可以模拟 fetch 请求。

起初，我只是用`Search`组件中的`useEffect`构建抓取。这个想法应该是这样的:

```
import React, { useState, useEffect } from 'react';
import { getProducts } from 'api';

export const Search = () => {
  const [products, setProducts] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [hasError, setHasError] = useState(false);

  useEffect(() => {
    const fetchProducts = async () => {
      try {
        setIsLoading(true);

        const fetchedProducts = await getProducts();

        setIsLoading(false);
        setProducts(fetchedProducts);
      } catch (error) {
        setIsLoading(false);
        setHasError(true);
      }
    };

    fetchProducts();
  }, []);
}; 
```

我有:

*   `products`初始化为空数组
*   `isLoading`初始化为假
*   `hasError`初始化为假
*   `fetchProducts`是一个异步函数，从`api`模块调用`getProducts`。由于我们还没有合适的产品 API，这个`getProducts`将返回一个模拟数据。
*   当`fetchProducts`被执行时，我们将`isLoading`设置为真，取出产品，然后将`isLoading`设置为假，因为取出完成了，并且将取出的产品放入`products`以在组件中使用。
*   如果它在获取过程中出现任何错误，我们会捕获它们，将`isLoading`设置为 false，将`hasError`设置为 true。在这种情况下，组件将知道我们在获取时出现了错误，并可以处理这种情况。
*   一切都被封装在一个`useEffect`中，因为我们在这里做了一个副作用。

为了处理所有的状态逻辑(何时为特定上下文更新每个部分)，我们可以将其提取到一个简单的缩减器中。

```
import { State, FetchActionType, FetchAction } from './types';

export const fetchReducer = (state: State, action: FetchAction): State => {
  switch (action.type) {
    case FetchActionType.FETCH_INIT:
      return {
        ...state,
        isLoading: true,
        hasError: false,
      };
    case FetchActionType.FETCH_SUCCESS:
      return {
        ...state,
        hasError: false,
        isLoading: false,
        data: action.payload,
      };
    case FetchActionType.FETCH_ERROR:
      return {
        ...state,
        hasError: true,
        isLoading: false,
      };
    default:
      return state;
  }
}; 
```

这里的想法是分离每个动作类型并处理每个状态更新。所以`fetchReducer`将接收状态和动作，并返回一个新的状态。这一部分很有趣，因为它获取当前状态，然后返回一个新状态，但是我们通过使用`State`类型来保持状态契约。

对于每一种动作类型，我们都将以正确的方式更新状态。

*   `FETCH_INIT` : `isLoading`为真，`hasError`为假。
*   `FETCH_SUCCESS` : `hasError`为假，`isLoading`为假，更新数据(产品)。
*   `FETCH_ERROR` : `hasError`为真，`isLoading`为假。

万一不匹配任何动作类型，就返回当前状态。

`FetchActionType`是一个简单的类型脚本枚举:

```
export enum FetchActionType {
  FETCH_INIT = 'FETCH_INIT',
  FETCH_SUCCESS = 'FETCH_SUCCESS',
  FETCH_ERROR = 'FETCH_ERROR',
} 
```

而`State`只是一个简单的类型:

```
export type ProductType = {
  name: string;
  price: number;
  imageUrl: string;
  description: string;
  isShippingFree: boolean;
  discount: number;
};

export type Data = ProductType[];

export type State = {
  isLoading: boolean;
  hasError: boolean;
  data: Data;
}; 
```

有了这个新的减速器，现在我们可以取东西了。我们将新的缩减器和初始状态传递给它:

```
const initialState: State = {
  isLoading: false,
  hasError: false,
  data: fakeData,
};

const [state, dispatch] = useReducer(fetchReducer, initialState);

useEffect(() => {
  const fetchAPI = async () => {
    dispatch({ type: FetchActionType.FETCH_INIT });

    try {
      const payload = await fetchProducts();

      dispatch({
        type: FetchActionType.FETCH_SUCCESS,
        payload,
      });
    } catch (error) {
      dispatch({ type: FetchActionType.FETCH_ERROR });
    }
  };

  fetchAPI();
}, []); 
```

`initialState`具有相同的合同类型。我们把它和我们刚刚构建的`fetchReducer`一起传递给`useReducer`。`useReducer`提供状态和一个名为`dispatch`的函数来调用动作以更新我们的状态。

*   状态提取:分派`FETCH_INIT`
*   完成提取:发送`FETCH_SUCCESS`和产品有效载荷
*   获取时出错:调度`FETCH_ERROR`

这种抽象变得非常庞大，在我们的组件中可能非常冗长。我们可以将它提取为一个名为`useProductFetchAPI`的独立钩子。

```
export const useProductFetchAPI = (): State => {
  const initialState: State = {
    isLoading: false,
    hasError: false,
    data: fakeData,
  };

  const [state, dispatch] = useReducer(fetchReducer, initialState);

  useEffect(() => {
    const fetchAPI = async () => {
      dispatch({ type: FetchActionType.FETCH_INIT });

      try {
        const payload = await fetchProducts();

        dispatch({
          type: FetchActionType.FETCH_SUCCESS,
          payload,
        });
      } catch (error) {
        dispatch({ type: FetchActionType.FETCH_ERROR });
      }
    };

    fetchAPI();
  }, []);

  return state;
}; 
```

它只是一个包装我们的获取操作的函数。现在，在`Search`组件中，我们可以导入并调用它。

```
export const Search = () => {
  const { isLoading, hasError, data }: State = useProductFetchAPI();
}; 
```

我们在组件中使用了所有的 API: `isLoading`、`hasError`和`data`。有了这个 API，我们可以基于`isLoading`数据渲染一个加载微调器或骨架。我们可以基于`hasError`值呈现一条错误消息。或者只使用`data`渲染产品列表。

在开始实现我们的产品列表之前，我想停下来为我们的定制钩子添加测试。这里我们要测试两个部分:减速器和定制钩子。

缩减器更简单，因为它只是一个纯函数。它接收值、处理并返回新值。没有副作用。一切都是确定的。

为了涵盖这个缩减器的所有可能性，我创建了三个上下文:`FETCH_INIT`、`FETCH_SUCCESS`和`FETCH_ERROR`动作。

在实现任何东西之前，我设置了要使用的初始数据。

```
const initialData: Data = [];
const initialState: State = {
  isLoading: false,
  hasError: false,
  data: initialData,
}; 
```

现在，我可以为 reducer 传递这个初始状态以及我想要覆盖的特定操作。对于第一个测试，我想涵盖`FETCH_INIT`动作:

```
describe('when dispatch FETCH_INIT action', () => {
  it('returns the isLoading as true without any error', () => {
    const action: FetchAction = {
      type: FetchActionType.FETCH_INIT,
    };

    expect(fetchReducer(initialState, action)).toEqual({
      isLoading: true,
      hasError: false,
      data: initialData,
    });
  });
}); 
```

这很简单。它接收初始状态和动作，我们期待正确的返回值:新的状态，其中`isLoading`为`true`。

`FETCH_ERROR`非常相似:

```
describe('when dispatch FETCH_ERROR action', () => {
  it('returns the isLoading as true without any error', () => {
    const action: FetchAction = {
      type: FetchActionType.FETCH_ERROR,
    };

    expect(fetchReducer(initialState, action)).toEqual({
      isLoading: false,
      hasError: true,
      data: [],
    });
  });
}); 
```

但是我们传递了一个不同的动作，并期望`hasError`是`true`。

`FETCH_SUCCESS`有点复杂，因为我们只需要构建一个新的状态，并将其添加到动作的 payload 属性中。

```
describe('when dispatch FETCH_SUCCESS action', () => {
  it('returns the the API data', () => {
    const product: ProductType = {
      name: 'iPhone',
      price: 3500,
      imageUrl: 'image-url.png',
      description: 'Apple mobile phone',
      isShippingFree: true,
      discount: 0,
    };

    const action: FetchAction = {
      type: FetchActionType.FETCH_SUCCESS,
      payload: [product],
    };

    expect(fetchReducer(initialState, action)).toEqual({
      isLoading: false,
      hasError: false,
      data: [product],
    });
  });
}); 
```

但是这里没有什么太复杂的。新数据就在那里。产品清单。在这种情况下，就一个，iPhone 产品。

第二个测试将覆盖我们构建的定制钩子。在这些测试中，我编写了三个上下文:一个超时请求、一个失败的网络请求和一个成功的请求。

在这里，当我使用`axios`来获取数据时(当我有一个 API 来获取数据时，我会正确地使用它)，我使用`axios-mock-adapter`来模拟我们测试的每个上下文。

首先是设置:初始化我们的数据并设置一个 axios 模拟。

```
const mock: MockAdapter = new MockAdapter(axios);
const url: string = '/search';
const initialData: Data = []; 
```

我们开始对超时请求进行测试:

```
it('handles error on timed-out api request', async () => {
  mock.onGet(url).timeout();

  const { result, waitForNextUpdate } = renderHook(() =>
    useProductFetchAPI(url, initialData)
  );

  await waitForNextUpdate();

  const { isLoading, hasError, data }: State = result.current;

  expect(isLoading).toEqual(false);
  expect(hasError).toEqual(true);
  expect(data).toEqual(initialData);
}); 
```

我们设置模拟来返回超时。测试调用`useProductFetchAPI`，等待更新，然后我们可以得到状态。`isLoading`是假的，`data`仍然是相同的(一个空列表)，并且`hasError`现在如预期的那样是真的。

网络请求几乎是相同的行为。唯一的区别是，mock 将有一个网络错误，而不是超时。

```
it('handles error on failed network api request', async () => {
  mock.onGet(url).networkError();

  const { result, waitForNextUpdate } = renderHook(() =>
    useFetchAPI(url, initialData)
  );

  await waitForNextUpdate();

  const { isLoading, hasError, data }: State = result.current;

  expect(isLoading).toEqual(false);
  expect(hasError).toEqual(true);
  expect(data).toEqual(initialData);
}); 
```

对于成功的案例，我们需要创建一个产品对象，将其用作请求-响应数据。我们还期望`data`是这个产品对象的列表。在这种情况下,`hasError`和`isLoading`为假。

```
it('gets and updates data from the api request', async () => {
  const product: ProductType = {
    name: 'iPhone',
    price: 3500,
    imageUrl: 'image-url.png',
    description: 'Apple mobile phone',
    isShippingFree: true,
    discount: 0,
  };

  const mockedResponseData: Data = [product];

  mock.onGet(url).reply(200, mockedResponseData);

  const { result, waitForNextUpdate } = renderHook(() =>
    useFetchAPI(url, initialData)
  );

  await waitForNextUpdate();

  const { isLoading, hasError, data }: State = result.current;

  expect(isLoading).toEqual(false);
  expect(hasError).toEqual(false);
  expect(data).toEqual([product]);
}); 
```

太好了。我们涵盖了我们创建的这个定制钩子和缩减器所需的一切。现在，我们可以专注于构建产品列表。

## 产品列表

产品列表的想法是列出包含一些信息的产品:标题、描述、价格、折扣，以及是否有免费送货。最终的产品卡将如下所示:

![Screen-Shot-2020-06-06-at-15.52.17](img/8e3136f3e6072a2e36542bdfe0160a1b.png)

为了构建这张卡，我创建了产品组件的基础:

```
const Product = () => (
  <Box>
    <Image />
    <TitleDescription/>
    <Price />
    <Tag />
  </Box>
); 
```

为了构建产品，我们需要构建产品内部的每个组件。

但是在开始构建产品组件之前，我想展示一下假 API 将为我们返回的`JSON`数据。

```
{
  imageUrl: 'a-url-for-tokyo-tower.png',
  name: 'Tokyo Tower',
  description: 'Some description here',
  price: 45,
  discount: 20,
  isShippingFree: true,
} 
```

该数据从`Search`组件传递到`ProductList`组件:

```
export const Search = () => {
  const { isLoading, hasError, data }: State = useProductFetchAPI();

  if (hasError) {
    return <h2>Error</h2>;
  }

  return <ProductList products={data} isLoading={isLoading} />;
}; 
```

由于我使用的是 Typescript，所以我可以强制组件属性的静态类型。在这种情况下，我有道具`products`和`isLoading`。

我构建了一个`ProductListPropsType`类型来处理产品列表道具。

```
type ProductListPropsType = {
  products: ProductType[];
  isLoading: boolean;
}; 
```

而`ProductType`是代表产品的简单类型:

```
export type ProductType = {
  name: string;
  price: number;
  imageUrl: string;
  description: string;
  isShippingFree: boolean;
  discount: number;
}; 
```

为了构建产品列表，我将使用 Material UI 中的`Grid`组件。首先，我们有一个网格容器，然后，对于每个产品，我们将呈现一个网格项目。

```
export const ProductList = ({ products, isLoading }: ProductListPropsType) => (
  <Grid container spacing={3}>
    {products.map(product => (
      <Grid
        item
        xs={6}
        md={3}
        key={`grid-${product.name}-${product.description}-${product.price}`}
      >
        <Product
          key={`product-${product.name}-${product.description}-${product.price}`}
          imageUrl={product.imageUrl}
          name={product.name}
          description={product.description}
          price={product.price}
          discount={product.discount}
          isShippingFree={product.isShippingFree}
          isLoading={isLoading}
        />
      </Grid>
    ))}
  </Grid>
); 
```

当我们为每列使用值`6`时，`Grid`项将为移动设备每行显示 2 个项目。对于桌面版本，每行将呈现 4 个项目。

我们遍历`products`列表并呈现`Product`组件，传递它需要的所有数据。

现在我们可以专注于构建`Product`组件了。

让我们从最简单的开始:`Tag`。我们将向该组件传递三个数据。`label`、`isVisible`和`isLoading`。当它不可见时，我们只需返回`null`来不渲染它。如果正在加载，我们将从材质 UI 渲染一个`Skeleton`组件。但是在加载之后，我们用`Free Shipping`标签呈现标签信息。

```
export const Tag = ({ label, isVisible, isLoading }: TagProps) => {
  if (!isVisible) return null;
  if (isLoading) {
    return (
      <Skeleton width="110px" height="40px" data-testid="tag-skeleton-loader" />
    );
  }

  return (
    <Box mt={1} data-testid="tag-label-wrapper">
      <span style={tabStyle}>{label}</span>
    </Box>
  );
}; 
```

`TagProps`是一个简单的类型:

```
type TagProps = {
  label: string;
  isVisible: boolean;
  isLoading: boolean;
}; 
```

我还使用一个对象来设计`span`的样式:

```
const tabStyle = {
  padding: '4px 8px',
  backgroundColor: '#f2f3fe',
  color: '#87a7ff',
  borderRadius: '4px',
}; 
```

我还想为这个组件构建测试，尝试思考它的行为:

*   当它不可见时:标签将不在文档中。

```
describe('when is not visible', () => {
  it('does not render anything', () => {
    const { queryByTestId } = render(
      <Tag label="a label" isVisible={false} isLoading={false} />
    );

    expect(queryByTestId('tag-label-wrapper')).not.toBeInTheDocument();
  });
}); 
```

*   加载时:框架将在文档中。

```
describe('when is loading', () => {
  it('renders the tag label', () => {
    const { queryByTestId } = render(
      <Tag label="a label" isVisible isLoading />
    );

    expect(queryByTestId('tag-skeleton-loader')).toBeInTheDocument();
  });
}); 
```

*   当准备好呈现时:标签将在文档中。

```
describe('when is visible and not loading', () => {
  it('renders the tag label', () => {
    render(<Tag label="a label" isVisible isLoading={false} />);

    expect(screen.getByText('a label')).toBeInTheDocument();
  });
}); 
```

*   加分点:可及性。我还使用`jest-axe`构建了一个自动化测试来覆盖可访问性违规。

```
it('has no accessibility violations', async () => {
  const { container } = render(
    <Tag label="a label" isVisible isLoading={false} />
  );

  const results = await axe(container);

  expect(results).toHaveNoViolations();
}); 
```

我们准备实现另一个组件:`TitleDescription`。它的工作方式几乎与`Tag`组件相似。它收到一些道具:`name`、`description`、`isLoading`。

因为我们有了带有类型定义的`name`和`description`的`Product`类型，所以我想重用它。我尝试了不同的东西-你可以[在这里查看更多细节](https://leandrotk.github.io/tk/2020/05/typescript-learnings-interesting-types/index.html) -我找到了`Pick`类型。这样，我就可以从`ProductType`得到`name`和`description`:

```
type TitleDescriptionType = Pick<ProductType, 'name' | 'description'>; 
```

有了这个新类型，我可以为组件创建`TitleDescriptionPropsType`:

```
type TitleDescriptionPropsType = TitleDescriptionType & {
  isLoading: boolean;
}; 
```

现在在组件内部工作，如果`isLoading`为真，则组件在呈现实际的标题和描述文本之前呈现适当的框架组件。

```
if (isLoading) {
  return (
    <Fragment>
      <Skeleton
        width="60%"
        height="24px"
        data-testid="name-skeleton-loader"
      />
      <Skeleton
        style={descriptionSkeletonStyle}
        height="20px"
        data-testid="description-skeleton-loader"
      />
    </Fragment>
  );
} 
```

如果组件不再被加载，我们渲染标题和描述文本。这里我们使用了`Typography`组件。

```
return (
  <Fragment>
    <Typography data-testid="product-name">{name}</Typography>
    <Typography
      data-testid="product-description"
      color="textSecondary"
      variant="body2"
      style={descriptionStyle}
    >
      {description}
    </Typography>
  </Fragment>
); 
```

对于测试，我们需要三样东西:

*   加载时，组件会渲染骨骼
*   当组件不再加载时，它会呈现文本
*   确保组件不违反可访问性

我们将使用与`Tag`测试相同的想法:根据状态查看它是否在文档中。

当它被加载时，我们想看看框架是否在文档中，但是标题和描述文本不在。

```
describe('when is loading', () => {
  it('does not render anything', () => {
    const { queryByTestId } = render(
      <TitleDescription
        name={product.name}
        description={product.description}
        isLoading
      />
    );

    expect(queryByTestId('name-skeleton-loader')).toBeInTheDocument();
    expect(queryByTestId('description-skeleton-loader')).toBeInTheDocument();
    expect(queryByTestId('product-name')).not.toBeInTheDocument();
    expect(queryByTestId('product-description')).not.toBeInTheDocument();
  });
}); 
```

当它不再加载时，它呈现 DOM 中的文本:

```
describe('when finished loading', () => {
  it('renders the product name and description', () => {
    render(
      <TitleDescription
        name={product.name}
        description={product.description}
        isLoading={false}
      />
    );

    expect(screen.getByText(product.name)).toBeInTheDocument();
    expect(screen.getByText(product.description)).toBeInTheDocument();
  });
}); 
```

以及一个简单的测试来涵盖可访问性问题:

```
it('has no accessibility violations', async () => {
  const { container } = render(
    <TitleDescription
      name={product.name}
      description={product.description}
      isLoading={false}
    />
  );

  const results = await axe(container);

  expect(results).toHaveNoViolations();
}); 
```

下一个组件是`Price`。在这个组件中，我们将提供一个仍然加载的骨架，就像我们在另一个组件中所做的那样，并在这里添加三个不同的组件:

*   我们将折扣应用到原始价格中，并将其呈现出来
*   `OriginalPrice`:只是渲染产品价格
*   `Discount`:当产品有折扣时，呈现折扣百分比

但是在我开始实现这些组件之前，我想对要使用的数据进行结构化。`price`和`discount`值是数字。因此，让我们构建一个名为`getPriceInfo`的函数，它接收`price`和`discount`，并返回这些数据:

```
{
  priceWithDiscount,
  originalPrice,
  discountOff,
  hasDiscount,
}; 
```

对于这种类型的合同:

```
type PriceInfoType = {
  priceWithDiscount: string;
  originalPrice: string;
  discountOff: string;
  hasDiscount: boolean;
}; 
```

在这个函数中，它将获取`discount`并将其转换为`boolean`，然后应用`discount`构建`priceWithDiscount`，使用`hasDiscount`构建折扣百分比，并构建带有美元符号的`originalPrice`:

```
export const applyDiscount = (price: number, discount: number): number =>
  price - (price * discount) / 100;

export const getPriceInfo = (
  price: number,
  discount: number
): PriceInfoType => {
  const hasDiscount: boolean = Boolean(discount);
  const priceWithDiscount: string = hasDiscount
    ? `${applyDiscount(price, discount)}`
    : `${price}`;

  const originalPrice: string = `${price}`;
  const discountOff: string = hasDiscount ? `${discount}% OFF` : '';

  return {
    priceWithDiscount,
    originalPrice,
    discountOff,
    hasDiscount,
  };
}; 
```

这里我还构建了一个`applytDiscount`函数来提取折扣计算。

我添加了一些测试来涵盖这些功能。因为它们是纯函数，我们只需要传递一些值并期待新的数据。

对`applyDiscount`的测试:

```
describe('applyDiscount', () => {
  it('applies 20% discount in the price', () => {
    expect(applyDiscount(100, 20)).toEqual(80);
  });

  it('applies 95% discount in the price', () => {
    expect(applyDiscount(100, 95)).toEqual(5);
  });
}); 
```

对`getPriceInfo`的测试:

```
describe('getPriceInfo', () => {
  describe('with discount', () => {
    it('returns the correct price info', () => {
      expect(getPriceInfo(100, 20)).toMatchObject({
        priceWithDiscount: '$80',
        originalPrice: '$100',
        discountOff: '20% OFF',
        hasDiscount: true,
      });
    });
  });

  describe('without discount', () => {
    it('returns the correct price info', () => {
      expect(getPriceInfo(100, 0)).toMatchObject({
        priceWithDiscount: '$100',
        originalPrice: '$100',
        discountOff: '',
        hasDiscount: false,
      });
    });
  });
}); 
```

现在我们可以使用`Price`组件中的`getPriceInfo`来获取这个结构数据，并像这样传递给其他组件:

```
export const Price = ({ price, discount, isLoading }: PricePropsType) => {
  if (isLoading) {
    return (
      <Skeleton width="80%" height="18px" data-testid="price-skeleton-loader" />
    );
  }

  const {
    priceWithDiscount,
    originalPrice,
    discountOff,
    hasDiscount,
  }: PriceInfoType = getPriceInfo(price, discount);

  return (
    <Fragment>
      <PriceWithDiscount price={priceWithDiscount} />
      <OriginalPrice hasDiscount={hasDiscount} price={originalPrice} />
      <Discount hasDiscount={hasDiscount} discountOff={discountOff} />
    </Fragment>
  );
}; 
```

正如我们之前所说的，当它被加载时，我们只是渲染`Skeleton`组件。当它完成加载时，它将构建结构化数据并呈现价格信息。现在让我们来构建每个组件吧！

让我们从`OriginalPrice`开始。我们只需要将`price`作为道具传递，它将使用`Typography`组件进行渲染。

```
type OriginalPricePropsType = {
  price: string;
};

export const OriginalPrice = ({ price }: OriginalPricePropsType) => (
  <Typography display="inline" style={originalPriceStyle} color="textSecondary">
    {price}
  </Typography>
); 
```

很简单！现在让我们添加一个测试。

只需传递一个价格，并查看它是否在 DOM 中呈现:

```
it('shows the price', () => {
  const price = '$200';
  render(<OriginalPrice price={price} />);
  expect(screen.getByText(price)).toBeInTheDocument();
}); 
```

我还添加了一个测试来涵盖可访问性问题:

```
it('has no accessibility violations', async () => {
  const { container } = render(<OriginalPrice price="$200" />);
  const results = await axe(container);

  expect(results).toHaveNoViolations();
}); 
```

组件`PriceWithDiscount`有一个非常相似的实现，但是我们通过`hasDiscount`布尔值来显示价格。如果它有折扣，则显示有折扣的价格。否则不会渲染任何东西。

```
type PricePropsType = {
  hasDiscount: boolean;
  price: string;
}; 
```

道具类型有`hasDiscount`和`price`。该组件只是基于`hasDiscount`值来呈现事物。

```
export const PriceWithDiscount = ({ price, hasDiscount }: PricePropsType) => {
  if (!hasDiscount) {
    return null;
  }

  return (
    <Typography display="inline" style={priceWithDiscountStyle}>
      {price}
    </Typography>
  );
}; 
```

测试将覆盖这个逻辑，无论它有没有折扣。如果没有折扣，价格就不会显示出来。

```
describe('when the product has no discount', () => {
  it('shows nothing', () => {
    const { queryByTestId } = render(
      <PriceWithDiscount hasDiscount={false} price="" />
    );

    expect(queryByTestId('discount-off-label')).not.toBeInTheDocument();
  });
}); 
```

如果它有折扣，它将在 DOM 中呈现:

```
describe('when the product has a discount', () => {
  it('shows the price', () => {
    const price = '$200';
    render(<PriceWithDiscount hasDiscount price={price} />);
    expect(screen.getByText(price)).toBeInTheDocument();
  });
}); 
```

和往常一样，测试覆盖可访问性违规:

```
it('has no accessibility violations', async () => {
  const { container } = render(
    <PriceWithDiscount hasDiscount price="$200" />
  );

  const results = await axe(container);

  expect(results).toHaveNoViolations();
}); 
```

`Discount`组件与`PriceWithDiscount`非常相似。如果产品有折扣，则呈现折扣标签:

```
type DiscountPropsType = {
  hasDiscount: boolean;
  discountOff: string;
};

export const Discount = ({ hasDiscount, discountOff }: DiscountPropsType) => {
  if (!hasDiscount) {
    return null;
  }

  return (
    <Typography
      display="inline"
      color="secondary"
      data-testid="discount-off-label"
    >
      {discountOff}
    </Typography>
  );
}; 
```

我们对其他组件所做的所有测试，对`Discount`组件也做了同样的事情:

```
describe('Discount', () => {
  describe('when the product has a discount', () => {
    it('shows the discount label', () => {
      const discountOff = '20% OFF';
      render(<Discount hasDiscount discountOff={discountOff} />);
      expect(screen.getByText(discountOff)).toBeInTheDocument();
    });
  });

  describe('when the product has no discount', () => {
    it('shows nothing', () => {
      const { queryByTestId } = render(
        <Discount hasDiscount={false} discountOff="" />
      );

      expect(queryByTestId('discount-off-label')).not.toBeInTheDocument();
    });
  });

  it('has no accessibility violations', async () => {
    const { container } = render(
      <Discount hasDiscount discountOff="20% OFF" />
    );

    const results = await axe(container);

    expect(results).toHaveNoViolations();
  });
}); 
```

现在我们将构建一个`Image`组件。这个组件具有我们构建的任何其他组件的基本框架。如果正在加载，请等待渲染图像源，然后渲染骨架。当它完成加载时，我们将渲染图像，但前提是组件位于浏览器窗口的交叉点。

这是什么意思？当你在移动设备上浏览网站时，你可能会看到前 4 种产品。他们将渲染骨骼，然后是图像。但是在这 4 个产品下面，你看不到任何一个，所以我们是否渲染它们并不重要。我们可以选择不渲染它们。暂时没有。而是按需提供。当你滚动时，如果产品的图像在浏览器窗口的交叉点，我们开始渲染图像源。

这样，我们可以通过加快页面加载时间来提高性能，并通过按需请求图像来降低成本。

我们将使用交叉点观察器 API 按需下载图像。但是在编写关于这项技术的任何代码之前，让我们开始用图像和骨架视图构建我们的组件。

形象道具会有这个对象:

```
{
  imageUrl,
  imageAlt,
  width,
  isLoading,
  imageWrapperStyle,
  imageStyle,
} 
```

`imageUrl`、`imageAlt`、`isLoading`道具由产品组件传递。`width`是骨骼和图像标签的属性。`imageWrapperStyle`和`imageStyle`是在图像组件中具有默认值的道具。我们以后再谈这个。

让我们为这个道具添加一个类型:

```
type ImageUrlType = Pick<ProductType, 'imageUrl'>;
type ImageAttrType = { imageAlt: string; width: string };
type ImageStateType = { isLoading: boolean };
type ImageStyleType = {
  imageWrapperStyle: CSSProperties;
  imageStyle: CSSProperties;
};

export type ImagePropsType = ImageUrlType &
  ImageAttrType &
  ImageStateType &
  ImageStyleType; 
```

这里的想法是给类型赋予意义，然后组合一切。我们可以从`ProductType`中得到`imageUrl`。属性类型将有`imageAlt`和`width`。图像状态有`isLoading`状态。而且形象风格有一些`CSSProperties`。

起初，组件会这样:

```
export const Image = ({
  imageUrl,
  imageAlt,
  width,
  isLoading,
  imageWrapperStyle,
  imageStyle,
}: ImagePropsType) => {
  if (isLoading) {
    <Skeleton
      variant="rect"
      width={width}
      data-testid="image-skeleton-loader"
    />
  }

  return (
    <img
      src={imageUrl}
      alt={imageAlt}
      width={width}
      style={imageStyle}
    />
  );
}; 
```

让我们构建代码来使交叉点观察器工作。

交叉点观察器的思想是接收一个被观察的目标和一个回调函数，每当被观察的目标进入或退出视口时，该回调函数就被执行。所以实现非常简单:

```
const observer: IntersectionObserver = new IntersectionObserver(
  onIntersect,
  options
);

observer.observe(target); 
```

通过传递一个选项对象和回调函数来实例化`IntersectionObserver`类。`observer`将观察到`target`元素。

由于它是 DOM 中的一个效果，我们可以将它包装到一个`useEffect`中。

```
useEffect(() => {
  const observer: IntersectionObserver = new IntersectionObserver(
    onIntersect,
    options
  );

  observer.observe(target);

  return () => {
    observer.unobserve(target);
  };
}, [target]); 
```

使用`useEffect`，我们在这里有两个不同的东西:依赖数组和返回函数。我们将`target`作为依赖函数传递，以确保如果`target`改变，我们将重新运行效果。返回函数是一个清理函数。React 在组件卸载时执行清理，因此它会在每次渲染运行另一个效果之前清理效果。

在这个清理函数中，我们只是停止观察`target`元素。

当组件开始渲染时，`target`引用还没有设置，所以我们需要有一个防护来防止观察到`undefined`目标。

```
useEffect(() => {
  if (!target) {
    return;
  }

  const observer: IntersectionObserver = new IntersectionObserver(
    onIntersect,
    options
  );

  observer.observe(target);

  return () => {
    observer.unobserve(target);
  };
}, [target]); 
```

我们可以不在组件中使用这种效果，而是构建一个定制的钩子来接收目标，一些选项来定制配置，它将提供一个布尔值来判断目标是否在视口的交叉点上。

```
export type TargetType = Element | HTMLDivElement | undefined;
export type IntersectionStatus = {
  isIntersecting: boolean;
};

const defaultOptions: IntersectionObserverInit = {
  rootMargin: '0px',
  threshold: 0.1,
};

export const useIntersectionObserver = (
  target: TargetType,
  options: IntersectionObserverInit = defaultOptions
): IntersectionStatus => {
  const [isIntersecting, setIsIntersecting] = useState(false);

  useEffect(() => {
    if (!target) {
      return;
    }

    const onIntersect = ([entry]: IntersectionObserverEntry[]) => {
      setIsIntersecting(entry.isIntersecting);

			if (entry.isIntersecting) {
        observer.unobserve(target);
      }
    };

    const observer: IntersectionObserver = new IntersectionObserver(
      onIntersect,
      options
    );

    observer.observe(target);

    return () => {
      observer.unobserve(target);
    };
  }, [target]);

  return { isIntersecting };
}; 
```

在我们的回调函数中，我们只是设置入口目标是否与视口相交。`setIsIntersecting`是来自我们在自定义钩子顶部定义的`useState`钩子的 setter。

它被初始化为`false`，但是如果它与视口相交，它将更新为`true`。

有了组件中的新信息，我们就可以渲染或不渲染图像。如果是相交的，我们可以渲染图像。如果没有，只需渲染一个骨架，直到用户到达产品图像的视口交叉点。

实际情况如何？

首先，我们使用`useState`定义包装器引用:

```
const [wrapperRef, setWrapperRef] = useState<HTMLDivElement>(); 
```

它从`undefined`开始。然后构建一个包装回调来设置元素节点:

```
const wrapperCallback = useCallback(node => {
  setWrapperRef(node);
}, []); 
```

这样，我们就可以通过在我们的`div`中使用一个`ref`属性来获取包装器引用。

```
<div ref={wrapperCallback}> 
```

在设置了`wrapperRef`之后，我们可以将它作为`useIntersectionObserver`的`target`进行传递，并期望得到一个`isIntersecting`状态:

```
const { isIntersecting }: IntersectionStatus = useIntersectionObserver(wrapperRef); 
```

有了这个新值，我们可以构建一个布尔值来知道我们是渲染了骨架还是产品图像。

```
const showImageSkeleton: boolean = isLoading || !isIntersecting; 
```

所以现在我们可以将适当的节点呈现给 DOM。

```
<div ref={wrapperCallback} style={imageWrapperStyle}>
  {showImageSkeleton ? (
    <Skeleton
      variant="rect"
      width={width}
      height={imageWrapperStyle.height}
      style={skeletonStyle}
      data-testid="image-skeleton-loader"
    />
  ) : (
    <img
      src={imageUrl}
      alt={imageAlt}
      width={width}
    />
  )}
</div> 
```

完整的组件如下所示:

```
export const Image = ({
  imageUrl,
  imageAlt,
  width,
  isLoading,
  imageWrapperStyle,
}: ImagePropsType) => {
  const [wrapperRef, setWrapperRef] = useState<HTMLDivElement>();
  const wrapperCallback = useCallback(node => {
    setWrapperRef(node);
  }, []);

  const { isIntersecting }: IntersectionStatus = useIntersectionObserver(wrapperRef);
  const showImageSkeleton: boolean = isLoading || !isIntersecting;

  return (
    <div ref={wrapperCallback} style={imageWrapperStyle}>
      {showImageSkeleton ? (
        <Skeleton
          variant="rect"
          width={width}
          height={imageWrapperStyle.height}
          style={skeletonStyle}
          data-testid="image-skeleton-loader"
        />
      ) : (
        <img
          src={imageUrl}
          alt={imageAlt}
          width={width}
        />
      )}
    </div>
  );
}; 
```

太好了，现在按需加载运行良好。但是我想建立一个稍微好一点的体验。这里的想法是有两个不同大小的相同图像。要求低质量的图像，我们使其可见，但在背景中要求高质量的图像时，图像变模糊。当高质量图像最终完成加载时，我们使用渐入/渐出过渡从低质量图像过渡到高质量图像，以使其体验流畅。

让我们建立这个逻辑。我们可以将它构建到组件中，但是我们也可以将这个逻辑提取到一个定制的钩子中。

```
export const useImageOnLoad = (): ImageOnLoadType => {
  const [isLoaded, setIsLoaded] = useState(false);
  const handleImageOnLoad = () => setIsLoaded(true);

  const imageVisibility: CSSProperties = {
    visibility: isLoaded ? 'hidden' : 'visible',
    filter: 'blur(10px)',
    transition: 'visibility 0ms ease-out 500ms',
  };

  const imageOpactity: CSSProperties = {
    opacity: isLoaded ? 1 : 0,
    transition: 'opacity 500ms ease-in 0ms',
  };

  return { handleImageOnLoad, imageVisibility, imageOpactity };
}; 
```

这个钩子只是为组件提供一些数据和行为。我们之前谈过的`handleImageOnLoad`、使低质量图像可见或不可见的`imageVisibility`以及从透明到不透明的`imageOpactity`，这样我们在加载图像后使其可见。

`isLoaded`是一个简单的布尔值，用来处理图像的可见性。另一个小细节是`filter: 'blur(10px)'`使低质量图像模糊，然后在从低质量图像过渡到高质量图像时缓慢对焦。

有了这个新钩子，我们只需导入它，并在组件内部调用:

```
const {
  handleImageOnLoad,
  imageVisibility,
  imageOpactity,
}: ImageOnLoadType = useImageOnLoad(); 
```

并开始使用我们建立的数据和行为。

```
<Fragment>
  <img
    src={thumbUrl}
    alt={imageAlt}
    width={width}
    style={{ ...imageStyle, ...imageVisibility }}
  />
  <img
    onLoad={handleImageOnLoad}
    src={imageUrl}
    alt={imageAlt}
    width={width}
    style={{ ...imageStyle, ...imageOpactity }}
  />
</Fragment> 
```

第一个是低质量图像`thumbUrl`。第二个具有原始的高质量图像，即`imageUrl`。当加载高质量图像时，它调用`handleImageOnLoad`函数。该功能将在一幅图像和另一幅图像之间进行转换。

 <https://raw.githubusercontent.com/leandrotk/tk/master/2020/06/ux-studies-with-react-typescript-and-testing-library/assets/loading-japan.mp4> 

## 包扎

这是这个项目的第一部分，学习更多关于用户体验、本地 API、类型化前端和测试的知识。

对于本系列的下一部分，我们将更多地从架构的角度来考虑使用过滤器来构建搜索，但要保持带来技术解决方案的心态，以使用户体验尽可能顺畅。

你可以在 [TK 的博客](https://leandrotk.github.io/tk/2020/06/ux-studies-with-react-typescript-and-testing-library/index.html)上找到其他类似的文章。

## 资源

*   [懒惰加载图像和视频](https://developers.google.com/web/fundamentals/performance/lazy-loading-guidance/images-and-video)
*   [交叉点观察器的功能用途](https://css-tricks.com/a-few-functional-uses-for-intersection-observer-to-know-when-an-element-is-in-view/)
*   [滚动自己懒加载的小技巧](https://css-tricks.com/tips-for-rolling-your-own-lazy-loading/)
*   [交叉口观察器 API - MDN](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
*   [反应打字稿备忘单](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet)