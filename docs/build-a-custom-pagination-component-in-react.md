# 如何在 React 中构建自定义分页组件

> 原文：<https://www.freecodecamp.org/news/build-a-custom-pagination-component-in-react/>

我们经常使用需要通过 API 从服务器获取大量数据并将其呈现在屏幕上的 web 应用程序。

例如，在**社交媒体应用**中，我们获取并呈现用户的帖子和评论。在**的人力资源仪表盘**中，我们显示了申请工作的候选人的信息。在一个**电子邮件客户端**中，我们展示了用户的电子邮件。由于网页中存在大量 DOM 元素，在屏幕上一次呈现所有数据会导致网页速度大幅下降。

如果我们想优化性能，我们可以采用各种技术以更有效的方式呈现数据。这些方法包括虚拟化的无限滚动和 T2 分页。

如果您事先知道数据的大小，并且不频繁地添加或删除数据集，分页就能很好地工作。

例如，在每隔几毫秒就会发布新帖子的社交媒体网站中，分页并不是理想的解决方案。但是对于显示候选人申请并需要过滤或排序的 HR 仪表板来说，它会工作得很好。

在这篇文章中，我们将重点关注分页，我们将构建一个自定义控制的组件，根据当前页面和总数据量来处理页面按钮。

我们还将编写一个自定义的 React 挂钩，为我们提供一个由分页组件呈现的数字范围。当我们想用不同的样式或不同的设计呈现一个分页组件时，我们也可以独立地使用这个钩子。

下面是我们将在本教程中构建的内容的演示:

![PaginationDemo](img/459accc9d6aaa8078d7816619b56a9cb.png)

## 如何设置项目

如果您熟悉 React 项目的设置，可以跳过这一节。

为了设置我们的 React 项目，我们将使用 [`create-react-app`](https://github.com/facebook/create-react-app) 命令行包。您可以使用`npm install -g create-react-app`或`yarn add global create-react-app`全局安装软件包。

从命令行运行`create-react-app`创建一个新项目，如下所示:

```
npx create-react-app react-pagination
```

接下来，我们需要安装我们的依赖项。我们将只使用一个简单的叫做`classnames` 的附加依赖，它在有条件地处理多个类名时提供了灵活性。

要安装它，运行`npm install classnames`或`yarn add classnames`。

现在，我们可以使用下面的命令运行我们的项目:

```
yarn start
```

## 如何定义接口

既然我们的项目已经运行，我们将直接进入我们的`Pagination`组件。

让我们先来看看我们需要什么样的价值观来支撑我们的`Pagination`组件:

*   **totalCount** :表示可从数据源获得的数据总数。
*   **当前页面** : 代表当前活动页面。对于我们的`currentPage`值，我们将使用基于 **1 的索引**，而不是传统的基于 0 的索引。
*   **pageSize** : 表示单页中可见的最大数据量。
*   **onPageChange** : 当页面发生变化时，用更新后的页面值调用回调函数。
*   **siblingCount** (可选):表示当前页面按钮每侧显示的最小页面按钮数。默认为 1。

![image-85](img/55510346f105802bee7b3747104ce726.png)

Illustration of different values siblingCount

*   **className** (可选):要添加到顶层容器中的类名。

从分页组件中，我们将调用`usePagination`钩子，它将接受以下参数来计算页面范围:`totalCount`、`currentPage`、`pageSize`、`siblingCount`。

## 如何实现 usePagination 挂钩

下面是我们在实现`usePagination`钩子时需要记住的几件事:

*   我们的分页钩子必须返回要在分页组件中以数组形式显示的数字范围。
*   当`currentPage`、`pageSize`、`siblingCount`或`totalCount`发生变化时，计算逻辑需要重新运行。
*   钩子返回的项目总数应该保持不变。如果用户与组件交互时范围数组的长度发生变化，这将避免调整分页组件的大小。

记住上面的事情，让我们在我们的项目`src`文件夹中创建一个名为`usePagination.js`的文件，并开始实现。

我们的代码框架如下:

```
export const usePagination = ({
  totalCount,
  pageSize,
  siblingCount = 1,
  currentPage
}) => {
  const paginationRange = useMemo(() => {
     // Our implementation logic will go here 

  }, [totalCount, pageSize, siblingCount, currentPage]);

  return paginationRange;
};
```

如果我们看看上面的代码，我们正在使用`useMemo`钩子来计算我们的核心逻辑。当依赖数组中的任何值改变时，`useMemo`回调将运行。

此外，我们将`siblingCount`道具的`defaultValue`设置为`1`，因为它是一个可选道具。

在我们继续实现代码逻辑之前，让我们理解一下`Pagination`组件的不同行为。下图包含分页组件的可能状态:

![image-80](img/3e7a8c6188338a7587b1f61780d97e83.png)

Different states of a Pagination Component

请注意，分页组件有四种可能的状态。我们会一个一个检查。

*   总页数少于我们要显示的页数。在这种情况下，我们只返回从`1 to totalPageCount`开始的范围。
*   总页数大于页数，但只有正确的点可见。
*   总页数大于页码，但只有左边的点可见。
*   总页数大于页数，左右点都可见。

作为第一步，我们将着手计算从`totalCount`到`pageSize`的总页数，如下所示:

```
const totalPageCount = Math.ceil(totalCount / pageSize);
```

注意，我们使用`Math.ceil`将数字四舍五入到下一个更高的整数值。这确保了我们为剩余的数据保留了额外的页面。

接下来，我们将实现一个定制的`range`函数，它接受一个`start`和`end`值，并返回一个从头到尾都包含元素的数组:

```
const range = (start, end) => {
  let length = end - start + 1;
  /*
  	Create an array of certain length and set the elements within it from
    start value to end value.
  */
  return Array.from({ length }, (_, idx) => idx + start);
};
```

最后，我们将通过记住上述案例来实现核心逻辑。

```
export const usePagination = ({
  totalCount,
  pageSize,
  siblingCount = 1,
  currentPage
}) => {
  const paginationRange = useMemo(() => {
    const totalPageCount = Math.ceil(totalCount / pageSize);

    // Pages count is determined as siblingCount + firstPage + lastPage + currentPage + 2*DOTS
    const totalPageNumbers = siblingCount + 5;

    /*
      Case 1:
      If the number of pages is less than the page numbers we want to show in our
      paginationComponent, we return the range [1..totalPageCount]
    */
    if (totalPageNumbers >= totalPageCount) {
      return range(1, totalPageCount);
    }

    /*
    	Calculate left and right sibling index and make sure they are within range 1 and totalPageCount
    */
    const leftSiblingIndex = Math.max(currentPage - siblingCount, 1);
    const rightSiblingIndex = Math.min(
      currentPage + siblingCount,
      totalPageCount
    );

    /*
      We do not show dots just when there is just one page number to be inserted between the extremes of sibling and the page limits i.e 1 and totalPageCount. Hence we are using leftSiblingIndex > 2 and rightSiblingIndex < totalPageCount - 2
    */
    const shouldShowLeftDots = leftSiblingIndex > 2;
    const shouldShowRightDots = rightSiblingIndex < totalPageCount - 2;

    const firstPageIndex = 1;
    const lastPageIndex = totalPageCount;

    /*
    	Case 2: No left dots to show, but rights dots to be shown
    */
    if (!shouldShowLeftDots && shouldShowRightDots) {
      let leftItemCount = 3 + 2 * siblingCount;
      let leftRange = range(1, leftItemCount);

      return [...leftRange, DOTS, totalPageCount];
    }

    /*
    	Case 3: No right dots to show, but left dots to be shown
    */
    if (shouldShowLeftDots && !shouldShowRightDots) {

      let rightItemCount = 3 + 2 * siblingCount;
      let rightRange = range(
        totalPageCount - rightItemCount + 1,
        totalPageCount
      );
      return [firstPageIndex, DOTS, ...rightRange];
    }

    /*
    	Case 4: Both left and right dots to be shown
    */
    if (shouldShowLeftDots && shouldShowRightDots) {
      let middleRange = range(leftSiblingIndex, rightSiblingIndex);
      return [firstPageIndex, DOTS, ...middleRange, DOTS, lastPageIndex];
    }
  }, [totalCount, pageSize, siblingCount, currentPage]);

  return paginationRange;
}; 
```

usePagination Custom hook implementation 

实现的思想是，我们确定想要在分页组件中显示的数字范围，然后在返回最终范围时用分隔符或点将它们连接在一起。

对于第一个场景，我们的`totalPageCount`小于我们根据其他参数计算的药丸总数，我们只返回一个数字范围`1..totalPageCount`。

对于其他场景，我们在将兄弟药丸包含到`currentPage`之后，通过计算左侧和右侧的索引来识别我们是否需要在`currentPage`的左侧或右侧的点，然后做出我们的决定。

一旦我们知道要在哪里显示点，剩下的计算就很简单了。

## 如何实现分页组件

正如我前面提到的，我们将在分页组件中使用`usePagination`钩子，我们将映射返回的范围来呈现它们。

我们在`src`文件夹中创建一个`Pagination.js`文件，并实现如下代码逻辑:

```
import React from 'react';
import classnames from 'classnames';
import { usePagination, DOTS } from './usePagination';
import './pagination.scss';
const Pagination = props => {
  const {
    onPageChange,
    totalCount,
    siblingCount = 1,
    currentPage,
    pageSize,
    className
  } = props;

  const paginationRange = usePagination({
    currentPage,
    totalCount,
    siblingCount,
    pageSize
  });

  // If there are less than 2 times in pagination range we shall not render the component
  if (currentPage === 0 || paginationRange.length < 2) {
    return null;
  }

  const onNext = () => {
    onPageChange(currentPage + 1);
  };

  const onPrevious = () => {
    onPageChange(currentPage - 1);
  };

  let lastPage = paginationRange[paginationRange.length - 1];
  return (
    <ul
      className={classnames('pagination-container', { [className]: className })}
    >
       {/* Left navigation arrow */}
      <li
        className={classnames('pagination-item', {
          disabled: currentPage === 1
        })}
        onClick={onPrevious}
      >
        <div className="arrow left" />
      </li>
      {paginationRange.map(pageNumber => {

        // If the pageItem is a DOT, render the DOTS unicode character
        if (pageNumber === DOTS) {
          return <li className="pagination-item dots">&#8230;</li>;
        }

        // Render our Page Pills
        return (
          <li
            className={classnames('pagination-item', {
              selected: pageNumber === currentPage
            })}
            onClick={() => onPageChange(pageNumber)}
          >
            {pageNumber}
          </li>
        );
      })}
      {/*  Right Navigation arrow */}
      <li
        className={classnames('pagination-item', {
          disabled: currentPage === lastPage
        })}
        onClick={onNext}
      >
        <div className="arrow right" />
      </li>
    </ul>
  );
};

export default Pagination; 
```

Pagination Implementation

如果页面少于两页，我们不会呈现一个`Pagination`组件(然后我们返回`null`)。

我们将`Pagination`组件呈现为一个带有左右箭头的列表，用于处理用户的上一个和下一个动作。在箭头之间，我们映射到`paginationRange`上，并将页码呈现为`pagination-items`。如果页面项是一个点，我们为它呈现一个 unicode 字符。

作为一个特殊的处理，如果`currentPage`分别是第一页或最后一页，我们将为左/右箭头添加一个`disabled`类。我们禁用`pointer-events`,如果图标需要禁用，通过 CSS 更新箭头图标的样式。

我们还向页面药丸添加了 click 事件处理程序，它将使用更新后的值`currentPage`调用`onPageChanged`回调函数。

我们的 CSS 文件将包含以下样式:

```
.pagination-container {
  display: flex;
  list-style-type: none;

  .pagination-item {
    padding: 0 12px;
    height: 32px;
    text-align: center;
    margin: auto 4px;
    color: rgba(0, 0, 0, 0.87);
    display: flex;
    box-sizing: border-box;
    align-items: center;
    letter-spacing: 0.01071em;
    border-radius: 16px;
    line-height: 1.43;
    font-size: 13px;
    min-width: 32px;

    &.dots:hover {
      background-color: transparent;
      cursor: default;
    }
    &:hover {
      background-color: rgba(0, 0, 0, 0.04);
      cursor: pointer;
    }

    &.selected {
      background-color: rgba(0, 0, 0, 0.08);
    }

    .arrow {
      &::before {
        position: relative;
        /* top: 3pt; Uncomment this to lower the icons as requested in comments*/
        content: '';
        /* By using an em scale, the arrows will size with the font */
        display: inline-block;
        width: 0.4em;
        height: 0.4em;
        border-right: 0.12em solid rgba(0, 0, 0, 0.87);
        border-top: 0.12em solid rgba(0, 0, 0, 0.87);
      }

      &.left {
        transform: rotate(-135deg) translate(-50%);
      }

      &.right {
        transform: rotate(45deg);
      }
    }

    &.disabled {
      pointer-events: none;

      .arrow::before {
        border-right: 0.12em solid rgba(0, 0, 0, 0.43);
        border-top: 0.12em solid rgba(0, 0, 0, 0.43);
      }

      &:hover {
        background-color: transparent;
        cursor: default;
      }
    }
  }
} 
```

Pagination component styles

**就这样！**

我们的通用分页实现已经准备好了，我们可以在代码库中的任何地方使用它。

## 如何使用自定义分页组件

最后一步，让我们将这个组件合并到一个小例子中。

对于本文的范围，我们将以表格的形式呈现静态数据。所以，让我们先这样做:

```
import React from 'react';
import data from './data/mock-data.json';

export default function App() {
  return (
    <>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>FIRST NAME</th>
            <th>LAST NAME</th>
            <th>EMAIL</th>
            <th>PHONE</th>
          </tr>
        </thead>
        <tbody>
          {data.map(item => {
            return (
              <tr>
                <td>{item.id}</td>
                <td>{item.first_name}</td>
                <td>{item.last_name}</td>
                <td>{item.email}</td>
                <td>{item.phone}</td>
              </tr>
            );
          })}
        </tbody>
      </table>
    </>
  );
}
```

此时，我们的用户界面如下所示:

![InfiniteTable-1](img/e0303f1e05132bb9b6b2cac4cd7cbe96.png)

现在要合并`Pagination`组件，我们需要两件东西。

*   首先，我们保持一个`currentPage`状态。
*   第二，我们计算要为给定页面呈现的数据，并对其进行映射和呈现。

出于演示的目的，我们将保持`PageSize`不变，并将其值设置为`10`。我们还可以为用户提供一个选择器来选择所需的`pageSize`。

一旦我们做了修改，我们就可以继续用合适的道具渲染我们的`Pagination`组件。

考虑到这些变化，我们的最终代码将如下所示:

```
import React, { useState, useMemo } from 'react';
import Pagination from '../Pagination';
import data from './data/mock-data.json';
import './style.scss';

let PageSize = 10;

export default function App() {
  const [currentPage, setCurrentPage] = useState(1);

  const currentTableData = useMemo(() => {
    const firstPageIndex = (currentPage - 1) * PageSize;
    const lastPageIndex = firstPageIndex + PageSize;
    return data.slice(firstPageIndex, lastPageIndex);
  }, [currentPage]);

  return (
    <>
      <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>FIRST NAME</th>
            <th>LAST NAME</th>
            <th>EMAIL</th>
            <th>PHONE</th>
          </tr>
        </thead>
        <tbody>
          {currentTableData.map(item => {
            return (
              <tr>
                <td>{item.id}</td>
                <td>{item.first_name}</td>
                <td>{item.last_name}</td>
                <td>{item.email}</td>
                <td>{item.phone}</td>
              </tr>
            );
          })}
        </tbody>
      </table>
      <Pagination
        className="pagination-bar"
        currentPage={currentPage}
        totalCount={data.length}
        pageSize={PageSize}
        onPageChange={page => setCurrentPage(page)}
      />
    </>
  );
} 
```

Final Demo code

下面是本教程的现场演示:

[https://stackblitz.com/edit/react-1zaeqk?embed=1&file=src%2FPagination.js](https://stackblitz.com/edit/react-1zaeqk?embed=1&file=src%2FPagination.js)

## 结论

在本文中，我们创建了一个定制的 React 挂钩`usePagination`，并在我们的`Pagination`组件中使用它。我们还实现了一个使用该组件的简短演示。

你可以在 GitHub 库中查看本教程的完整源代码。

如果您对本文有任何问题或建议，请随时在 Twitter 上联系我。

感谢阅读。