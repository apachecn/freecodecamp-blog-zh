# 将 Gatsby 默认启动器转换为使用样式化组件

> 原文：<https://www.freecodecamp.org/news/convert-the-gatsby-default-starter-to-use-styled-components-2018/>

让我们来看看如何使用 Gatsby 默认启动程序的样式化组件。

[https://www.youtube.com/embed/O5sWySCr668?feature=oembed](https://www.youtube.com/embed/O5sWySCr668?feature=oembed)

在这个例子中，我们将使用 Gatsby 默认启动器，并添加样式化组件，首先，打开一个新的[代码沙箱](https://codesandbox.io/s/)，并从服务器模板中选择 Gatsby。

## 1.属国

在 CodeSandbox 编辑器的依赖项部分，您需要添加以下内容:

```
gatsby-plugin-styled-components
styled-components
babel-plugin-styled-components 
```

## 2.配置

现在我们需要修改`gatsby-config.js`来合并我们刚刚添加的新依赖项。它没有任何配置选项，所以它可以作为配置中的额外一行，在这种情况下，我将它添加在`gatsby-plugin-sharp`插件之后:

```
'gatsby-transformer-sharp',
'gatsby-plugin-sharp',
'gatsby-plugin-styled-components', 
```

别忘了结尾的逗号？

## 3.全球风格

既然我们已经准备好用样式组件来摇滚了，我们需要移除默认启动器中当前应用的样式，并应用我们自己的样式。

在`src/components/layout.js`组件中有一个对`layout.css`的导入，这是对 starter 的 CSS 重置，如果我们从这里移除导入，我们将看到边框和字体的样式被重置。我们现在可以删除`layout.css`文件，并使用样式化组件的`createGlobalStyle`函数创建自己的 CSS 重置。

创建一个新文件夹`theme`，在这个例子中它在`src/theme`中，并在那里添加一个`globalStyle.js`文件。

这将导出一个`GlobalStyle`组件供我们在整个应用程序中使用。

让我们添加一个谷歌字体并重置`padding`和`margin`，为了视觉反馈，我们将字体直接添加到 body 元素中。

```
import { createGlobalStyle } from 'styled-components';

export const GlobalStyle = createGlobalStyle`
  @import url('https://fonts.googleapis.com/css?family=Kodchasan:400,700');
  body {
    padding: 0;
    margin: 0;
    font-family: Kodchasan;
  }
  a {
    text-decoration: none;
  }
  ul {
    margin: 0 auto;
    list-style-type: none;
  }
`; 
```

好了，现在我们可以使用这里的导出组件在应用程序中全局应用样式。因此，我们需要将它放在组件树的尽可能高的位置，在这种情况下，它在`layout.js`组件中，因为它包装了这个项目中的所有页面。

在`layout.js`中导入`GlobalStyle`组件。

```
import Header from './header';
import { GlobalStyle } from '../theme/globalStyle'; 
```

然后将它添加到正在渲染的其他组件中。

```
render={data => (
  <>
    <GlobalStyle />
    <Helmet
    ... 
```

好吧！应用全局样式后，我们现在应该可以看到字体和页面主体周围的边距发生了变化。

是时候使用样式化组件了！

## 4.使用样式组件

现在让我们将 starter 中使用的所有内联样式转换为样式化组件。

这是实际的样式部分，它将现有的样式移动到样式化的组件，从文件结构的顶部到底部工作，改变:

```
src/components/header.js
src/components/layout.js
src/pages/index.js 
```

**header.js**

```
import React from 'react';
import { Link } from 'gatsby';
import styled from 'styled-components';

const HeaderWrapper = styled.div`
  background: rebeccapurple;
  margin-bottom: 1.45rem;
`;

const Headline = styled.div`
  margin: 0 auto;
  max-width: 960;
  padding: 1.45rem 1.0875rem;
  h1 {
    margin: 0;
  }
`;

const StyledLink = styled(Link)`
  color: white;
  textdecoration: none;
`;

const Header = ({ siteTitle }) => (
  <HeaderWrapper>
    <Headline>
      <h1>
        <StyledLink to="/">{siteTitle}</StyledLink>
      </h1>
    </Headline>
  </HeaderWrapper>
);

export default Header; 
```

**layout.js**

```
import React from 'react';
import PropTypes from 'prop-types';
import Helmet from 'react-helmet';
import { StaticQuery, graphql } from 'gatsby';

import styled from 'styled-components';

import Header from './header';
import { GlobalStyle } from '../theme/globalStyle';

const ContentWrapper = styled.div`
  margin: 0 auto;
  maxwidth: 960;
  padding: 0px 1.0875rem 1.45rem;
  paddingtop: 0;
`;

const Layout = ({ children }) => (
  <StaticQuery
    query={graphql`
      query SiteTitleQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `}
    render={data => (
      <>
        <GlobalStyle />
        <Helmet title={data.site.siteMetadata.title} meta={[{ name: 'description', content: 'Sample' }, { name: 'keywords', content: 'sample, something' }]}>
          <html lang="en" />
        </Helmet>
        <Header siteTitle={data.site.siteMetadata.title} />
        <ContentWrapper>{children}</ContentWrapper>
      </>
    )}
  />
);

Layout.propTypes = {
  children: PropTypes.node.isRequired,
};

export default Layout; 
```

**index.js**

```
import React from 'react';
import { Link } from 'gatsby';
import styled from 'styled-components';

import Layout from '../components/layout';
import Image from '../components/image';

const ImgWrapper = styled.div`
  max-width: 300px;
  margin-bottom: 1.45rem;
`;

const IndexPage = () => (
  <Layout>
    <h1>Hi people</h1>
    <p>Welcome to your new Gatsby site.</p>
    <p>Now go build something great.</p>
    <ImgWrapper>
      <Image />
    </ImgWrapper>
    <Link to="/page-2/">Go to page 2</Link>
  </Layout>
);

export default IndexPage; 
```

## 5.完成的

**感谢阅读**

如果你需要参考，这是我们编写的[示例代码](https://codesandbox.io/s/yp3z16yw11)。

> 你可以在我的博客上阅读其他类似的文章。