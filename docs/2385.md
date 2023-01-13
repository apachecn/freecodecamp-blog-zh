# 我是如何为 DefinitelyTyped 做第一份贡献的，一个开源的 TypeScript Repo

> 原文：<https://www.freecodecamp.org/news/how-i-made-my-first-contribution-to-definitelytyped/>

TypeScript 太棒了。它立即使我们的代码更安全，更容易调试，并更好地记录。

一段时间以来，我一直在我的所有项目中专门使用 TypeScript。事情是这样的——我从未遇到过缺少类型定义的包。迄今...

我承认在这方面有点被宠坏了。但我惊讶地发现，我们使用的一个小而有用的[盖茨比](https://www.gatsbyjs.com/)插件——[盖茨比插件断点](https://www.gatsbyjs.com/plugins/gatsby-plugin-breakpoints/)——还没有类型定义。这让我开始思考。

# 我是如何解决这个问题的

我最初的反应是将插件从项目中移除。它提供了一个简单的功能，我可以很容易地复制自己。一般来说，我们对外部的依赖越少越好，对吗？

但是这个插件已经在整个项目中广泛使用了。所以如果我想避免重构，我必须完全复制它的 API。这意味着我必须重写已经存在的(和工作的)代码，仅仅是为了移除一个依赖。这似乎不是一个好方法。

相反，我决定只在我的项目中添加必要的类型。这很简单。在我知道之前，一切都完成了，可以走了。

但后来我意识到一件事。这是我第一次为我每天在 React 项目中使用的几十个库和插件中的任何一个做这件事。这为我提供了一个独一无二的机会，让我可以投身其中，为这个一开始就宠坏了我的社区做出一点小小的贡献。

我照做了。

# 如何为确定的类型做出贡献

[DefinitelyTyped](https://definitelytyped.org/) 是一个社区主导的开源库，用于 TypeScript 类型定义。这是我想要做出贡献的地方。

虽然我下面描述的有点以反应为中心，但我希望它仍然可以作为一个更通用的指南。

## 让设置有所贡献

入门很简单。第一步是派生出[明确类型化的](https://github.com/DefinitelyTyped/DefinitelyTyped) repo，并使用以下代码生成脚手架:

```
npx dts-gen --dt --name gatsby-plugin-breakpoints --template module 
```

这将在 DefinitelyTyped 项目中创建一个包含 4 个文件的文件夹:

```
gatsby-plugin-breakpoints-test.ts
index.d.ts
tsconfig.json
tslint.json
```

我们所有的类型定义都将存在于`index.d.ts`中，而`gatsby-plugin-breakpoints-test.ts`将帮助我们确保一切按预期运行。

在我们开始之前，因为我们正在使用 React，所以需要对`tsconfig.json`做一个小的调整。我们应该将`"jsx": "react"`添加到编译器选项中，并将测试文件名改为`gatsby-plugin-breakpoints-tests.tsx`。我们也不要忘记重命名实际的测试文件。

最终的`tsconfig.json`应该是这样的:

```
{
    "compilerOptions": {
        "module": "commonjs",
        "lib": [
            "es6",
            "DOM"
        ],
        "noImplicitAny": true,
        "noImplicitThis": true,
        "strictFunctionTypes": true,
        "strictNullChecks": true,
        "baseUrl": "../",
        "typeRoots": [
            "../"
        ],
        "types": [],
        "noEmit": true,
        "forceConsistentCasingInFileNames": true,
        "jsx": "react",
        "esModuleInterop": true
    },
    "files": [
        "index.d.ts",
        "gatsby-plugin-breakpoints-tests.tsx"
    ]
} 
```

到目前为止一切顺利！

## 如何创建类型并测试它们

通过查看`gatsby-plugin-breakpoints` [文档](https://www.gatsbyjs.com/plugins/gatsby-plugin-breakpoints/)和源代码，我们可以确定我们需要创建的类型。让我们列一个清单:

*   Plugin config
*   使用断点反应挂钩
*   带断点反应特设(高阶组件)
*   断点提供者
*   断点上下文

### Plugin config

让我们先解决配置问题。

```
// index.d.ts

// Type definitions for gatsby-plugin-breakpoints 1.3
// Project: https://github.com/JimmyBeldone/gatsby-plugin-breakpoints
// Definitions by: Iva Kop <https://github.com/IvaKop>
// Definitions: https://github.com/DefinitelyTyped/DefinitelyTyped

/**
 * @see https://www.gatsbyjs.com/plugins/gatsby-plugin-breakpoints/
 */

export type QueriesObject = Record<string, string>;

export interface BreakpointOptions {
    queries?: QueriesObject;
}

export interface BreakpointConfig {
    resolve: 'gatsby-plugin-breakpoints';
    options?: BreakpointOptions;
}
```

酷！现在让我们转到测试文件，确保我们已经正确地做了所有的事情:

```
// gatsby-plugin-breakpoints-tests.tsx

import React from 'react';
import {
    BreakpointConfig,
} from 'gatsby-plugin-breakpoints';

const defaultQueries = {
    xs: '(max-width: 320px)',
    sm: '(max-width: 720px)',
    md: '(max-width: 1024px)',
    l: '(max-width: 1536px)',
};

const plugins: BreakpointConfig = {
    resolve: `gatsby-plugin-breakpoints`,
    options: {
        queries: defaultQueries,
    },
}; 
```

### 使用断点

接下来让我们键入 useBreakpoints 钩子。

```
// index.d.ts
// ...

export type BreakpointsObject = Record<string, boolean>;

export function useBreakpoint(): BreakpointsObject;
```

并进行测试。

```
// gatsby-plugin-breakpoints-tests.tsx

import React from 'react';
import {
    // ...
    useBreakpoint
} from 'gatsby-plugin-breakpoints';

function HookComponent() {
    const breakpoints = useBreakpoint();
    return <div>{breakpoints.xs ? <div /> : null}</div>;
} 
```

### 带断点

该插件还提供了一个我们可以使用的 HOC(高阶组件)。让我们键入它:

```
// index.d.ts
// ...

export interface BreakpointProps {
    breakpoints: BreakpointsObject;
}

export function withBreakpoints<P extends BreakpointProps>(Component: React.ComponentType<P>): React.ComponentType<P>; 
```

...并进行测试:

```
// gatsby-plugin-breakpoints-tests.tsx

import React from 'react';
import {
    // ...
    withBreakpoints,
    BreakpointProps,
} from 'gatsby-plugin-breakpoints';

const EnhancedFunctionalComponent = withBreakpoints(function Component({ breakpoints }) {
    return breakpoints.xs ? <div /> : <div>Content hidden only on mobile</div>;
});

class Component extends React.Component<BreakpointProps> {
    render() {
        const { breakpoints } = this.props;
        return breakpoints.xs ? <div /> : <div>Content hidden only on mobile</div>;
    }
}

const EnhancedClassComponent = withBreakpoints(Component); 
```

### BreakpointContext 和 BreakpointProvider

最后，还有两件事要处理:`BreakpointContext`和`BreakpointProvider`:

```
// index.d.ts
// ...

export type QueriesObject = Record<string, string>;

export const BreakpointContext: React.Context<QueriesObject>;

export const BreakpointProvider: React.Provider<QueriesObject>;
```

上次测试:

```
// gatsby-plugin-breakpoints-tests.tsx

import React from 'react';
import {
    // ...
    BreakpointProvider,
    BreakpointContext,
} from 'gatsby-plugin-breakpoints';

// BreakpointContext
function useContext() {
    const context = React.useContext(BreakpointContext);
    return context;
}

// BreakpointProvider
const ProviderComponent: React.FC = ({ children }) => {
    return <BreakpointProvider value={defaultQueries}>{children}</BreakpointProvider>;
}; 
```

## 最后的步骤

最困难的部分已经过去了。剩下的工作就是在创建 pull 请求之前运行明确类型的 lint 和测试脚本。

```
npm run lint gatsby-plugin-breakpoints
```

一切似乎都很好！

```
npm run test gatsby-plugin-breakpoints
```

哦，不！似乎有一个错误:

```
Cannot find module 'csstype' or its corresponding type declarations.
```

谢天谢地，我们不是第一个遇到它的人。在确定类型的回购协议中已经有一个[未解决的问题](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/24788),修复看起来相当简单——我们需要在`types/react`中安装节点模块。让我们去吧！

```
cd types/react && npm install && cd ../../ && npm run test gatsby-plugin-breakpoints
```

唷，成功了！

在此阶段，我们准备好打开一个 PR，确保所有 CI 检查通过并等待批准。从我有限的经验来看，这个过程似乎相当快。这份特殊的公关在一天多一点的时间里就被批准、合并并出版了。

这里是[链接](https://github.com/DefinitelyTyped/DefinitelyTyped/pull/50868)如果你想去看看的话。

# 结论

我在这里描述的是一个微小的贡献。它可能还可以改进(我们编写的所有代码都是如此)。但是当与社区分享时，微小的贡献就会累积起来。

事实上，我已经专门使用 TypeScript 一年多了，这是我第一次遇到可以自己打字的项目，这证明了开源的强大。我很高兴让下一个人在他们的项目中使用 TypeScript 变得稍微容易了一点。

所以下一次你发现一个项目还在寻找它的类型(或者其他东西)，如果可以的话，一定要参与进来并做出贡献。这很重要！

*如果你觉得这个有用，[我们来连线](https://twitter.com/iva_kop)。更多关于 React 的深入文章，请查看我的博客。*