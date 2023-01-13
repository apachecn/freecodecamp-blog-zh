# 测试驱动的开发、功能和反应组件

> 原文：<https://www.freecodecamp.org/news/tdd-functions-and-react-components/>

这篇文章是我关于如何构建可持续和一致的软件的研究的一部分。在本帖中，我们将讨论测试驱动开发背后的思想，以及如何将这些知识应用于简单的功能、web 可访问性和 React 组件，主要是 Jest 和 React 测试库。

自动化测试是软件开发的一大部分。它给了我们，开发人员，发布代码的信心，但是我们增加了软件正常运行和工作的信心。

从我学习语言的第一天起，我就开始了我在 Ruby 社区写测试的软件生涯。Ruby(和 Rails)社区在测试自动化领域一直很强大。它帮助我形成了如何写好软件的心态。

所以使用 Ruby 和 Rails，我做了很多后端工作，比如后台工作、数据结构建模、API 构建等等。在这个范围内，用户始终是一个:开发人员用户。如果构建一个 API，用户就是使用该 API 的开发者。如果构建模型，用户将是使用该模型的开发人员。

现在我也在做大量的前端工作，在主要使用 React 和 Redux 构建 PWAs 的紧张的一年之后，首先我想到了一些想法:

*   在构建 UI 的时候，TDD 是不可能的。我如何知道它是 div 还是 span？
*   测试可能会很“复杂”。我应该浅尝辄止还是继续努力？测试一切？确保每个 div 都在正确的位置？

因此，我开始重新思考这些测试实践，以及如何使其富有成效。

TDD 是可能的。如果我想知道我是否应该期待一个 div 或一个 span，我可能测试错了。记住:测试应该给我们信心，不一定要涵盖每一点或实现细节。稍后我们将深入探讨这个话题！

我想建立这样的测试:

*   确保软件正常工作
*   自信地将代码交付到生产环境中
*   让我们思考软件设计

以及制作软件的测试:

*   易于维护
*   易于重构

## 测试驱动开发

TDD 不应该很复杂。这只是一个包含 3 个步骤的过程:

*   做个测试
*   让它跑起来
*   做正确的事

我们开始编写一个简单的测试来涵盖我们期望软件如何工作。然后我们进行代码的第一次实现(类、函数、脚本等)。现在，软件正在运行。它像预期的那样工作。是时候弥补了。是时候做得更好了。

目标是一个干净的工作代码。我们首先解决“有效”的问题，然后使代码变得干净。

这很简单。也应该如此。我没说这很容易。但这很简单，直截了当，只需 3 个步骤。每次你练习这个先写测试，后写代码，然后重构的过程，你都会感到更加自信。

首先编写测试时，一个好的技巧是考虑用例，并模拟它应该如何被使用(作为一个功能、组件或被一个真实的用户使用)。

## 功能

让我们将 TDD 应用到简单的函数中。

不久前，我实现了一个不动产登记流程的草案特性。该功能的一部分是显示一个模式，如果用户有一个未完成的房地产。我们将实现的功能是回答用户是否至少有一个房地产草案的功能。

所以第一步:编写测试！让我们想想这个函数的用例。它总是返回一个布尔值:真或假。

*   没有未保存的房产草稿:`false`
*   至少有一个未保存的房地产草稿:`true`

让我们编写代表这种行为的测试:

```
describe('hasRealEstateDraft', () => {
  describe('with real estate drafts', () => {
    it('returns true', () => {
      const realEstateDrafts = [
        {
          address: 'São Paulo',
          status: 'UNSAVED'
        }
      ];

      expect(hasRealEstateDraft(realEstateDrafts)).toBeTruthy();
    });
  });

  describe('with not drafts', () => {
    it('returns false', () => {
      expect(hasRealEstateDraft([])).toBeFalsy();
    });
  });
}); 
```

我们写了测试。但是当运行它时，它显示红色:2 个失败的测试，因为我们还没有实现这个函数。

第二步:让它跑起来！在这种情况下，非常简单。我们需要接收这个数组对象，并返回它是否至少有一个房地产草图。

```
const hasRealEstateDraft = (realEstateDrafts) => realEstateDrafts.length > 0; 
```

太好了！功能简单。简单的测试。我们可以进行第三步:纠正错误！但是在这种情况下，我们的函数非常简单，而且我们已经做对了。

但是现在我们需要该函数来获取房地产草案并将其传递给`hasRealEstateDraft`。

我们能想到哪个用例？

*   一份空的房地产清单
*   仅保存不动产
*   只有未保存的不动产
*   混合:保存和未保存的房地产

让我们编写测试来表示它:

```
describe('getRealEstateDrafts', () => {
  describe('with an empty list', () => {
    it('returns an empty list', () => {
      const realEstates = [];

      expect(getRealEstateDrafts(realEstates)).toMatchObject([]);
    });
  });

  describe('with only unsaved real estates', () => {
    it('returns the drafts', () => {
      const realEstates = [
        {
          address: 'São Paulo',
          status: 'UNSAVED'
        },
        {
          address: 'Tokyo',
          status: 'UNSAVED'
        }
      ];

      expect(getRealEstateDrafts(realEstates)).toMatchObject(realEstates);
    });
  });

  describe('with only saved real estates', () => {
    it('returns an empty list', () => {
      const realEstates = [
        {
          address: 'São Paulo',
          status: 'SAVED'
        },
        {
          address: 'Tokyo',
          status: 'SAVED'
        }
      ];

      expect(getRealEstateDrafts(realEstates)).toMatchObject([]);
    });
  });

  describe('with saved and unsaved real estates', () => {
    it('returns the drafts', () => {
      const realEstates = [
        {
          address: 'São Paulo',
          status: 'SAVED'
        },
        {
          address: 'Tokyo',
          status: 'UNSAVED'
        }
      ];

      expect(getRealEstateDrafts(realEstates)).toMatchObject([{
        address: 'Tokyo',
        status: 'UNSAVED'
      }]);
    });
  });
}); 
```

太好了！我们进行测试。它不起作用..还没！现在实现函数。

```
const getRealEstatesDrafts = (realEstates) => {
  const unsavedRealEstates = realEstates.filter((realEstate) => realEstate.status === 'UNSAVED');
  return unsavedRealEstates;
}; 
```

我们只需根据房地产状况进行筛选，然后返回。太好了，测试通过了，绿灯亮了！软件运行良好，但我们可以做得更好:第三步！

如何提取`filter`函数中的匿名函数，并用枚举来表示`'UNSAVED'`?

```
const STATUS = {
  UNSAVED: 'UNSAVED',
  SAVED: 'SAVED',
};

const byUnsaved = (realEstate) => realEstate.status === STATUS.UNSAVED;

const getRealEstatesDrafts = (realEstates) => realEstates.filter(byUnsaved); 
```

测试仍然通过，我们有一个更好的解决方案。

这里需要记住一件事:我将数据源与逻辑隔离开来。这是什么意思？我们从本地存储(数据源)获取数据，但是我们只测试负责逻辑的函数来获取草稿，并查看它是否至少有一个草稿。函数与逻辑，我们确保它的工作，它是干净的代码。

如果我们把`localStorage`放在函数内部，测试就变得很困难。因此，我们将责任分开，并使测试易于编写。纯函数更容易维护，编写测试也更简单。

## 反应组分

现在我们来谈谈 React 组件。回到引言，我们谈到了编写测试实现细节的测试。现在，我们将看到如何让它变得更好，更可持续，更有信心。

几天前，我计划为房地产所有者建立新的入职信息。它基本上是一堆具有相同设计的页面，但是它改变了页面的图标、标题和描述。

![ui](img/4ec4e94997c5f629cff7b569ec329d79.png)

我只想构建一个组件:`Content`并传递呈现正确图标、标题和描述所需的信息。我会将`businessContext`和`step`作为道具传递，它会将正确的内容呈现给入职页面。

我们不想知道我们是否会呈现 div 或 paragraph 标签。我们的测试需要确保对于给定的业务上下文和步骤，正确的内容将会存在。所以我带来了这些用例:

*   租赁业务环境的第一步
*   租赁业务环境的最后一步
*   销售业务环境的第一步
*   销售业务上下文的最后一步

让我们看看测试:

```
describe('Content', () => {
  describe('in the rental context', () => {
    const defaultProps = {
      businessContext: BUSINESS_CONTEXT.RENTAL
    };

    it('renders the title and description for the first step', () => {
      const step = 0;
      const { getByText } = render(<Content {...defaultProps} step={step} />);

      expect(getByText('the first step title')).toBeInTheDocument();
      expect(getByText('the first step description')).toBeInTheDocument();
    });

    it('renders the title and description for the forth step', () => {
      const step = 3;
      const { getByText } = render(<Content {...defaultProps} step={step} />);

      expect(getByText('the last step title')).toBeInTheDocument();
      expect(getByText('the last step description')).toBeInTheDocument();
    });
  });

  describe('in the sales context', () => {
    const defaultProps = {
      businessContext: BUSINESS_CONTEXT.SALE
    };

    it('renders the title and description for the first step', () => {
      const step = 0;
      const { getByText } = render(<Content {...defaultProps} step={step} />);

      expect(getByText('the first step title')).toBeInTheDocument();
      expect(getByText('the first step description')).toBeInTheDocument();
    });

    it('renders the title and description for the last step', () => {
      const step = 6;
      const { getByText } = render(<Content {...defaultProps} step={step} />);

      expect(getByText('the last step title')).toBeInTheDocument();
      expect(getByText('the last step description')).toBeInTheDocument();
    });
  });
}); 
```

我们为每个业务上下文准备了一个`describe`块，为每个步骤准备了一个`it`块。我还创建了一个可访问性测试来确保我们正在构建的组件是可访问的。

```
it('has not accessibility violations', async () => {
  const props = {
    businessContext: BUSINESS_CONTEXT.SALE,
    step: 0,
  };

  const { container } = render(<Content {...props} />);
  const results = await axe(container);

  expect(results).toHaveNoViolations();
}); 
```

现在我们需要让它运行起来！基本上，这个组件的 UI 部分只是图标、标题和描述。类似于:

```
<Fragment>
  <Icon />
  <h1>{title}</h1>
  <p>{description}</p>
</Fragment> 
```

我们只需要构建逻辑来获得所有这些正确的数据。因为我在这个组件中有了`businessContext`和`step`,所以我想做类似这样的事情

```
content[businessContext][step] 
```

它得到正确的内容。所以我建立了一个数据结构来这样工作。

```
const onboardingStepsContent = {
  alugar: {
    0: {
      Icon: Home,
      title: 'first step title',
      description: 'first step description',
    },
    // ...
  },
  vender: {
    0: {
      Icon: Home,
      title: 'first step title',
      description: 'first step description',
    },
    // ...
  },
}; 
```

它只是一个对象，第一个键作为业务上下文数据，对于每个业务上下文，它都有代表入职每一步的键。我们的组成部分是:

```
const Content = ({ businessContext, step }) => {
  const onboardingStepsContent = {
    alugar: {
      0: {
        Icon: Home,
        title: 'first step title',
        description: 'first step description',
      },
      // ...
    },
    vender: {
      0: {
        Icon: Home,
        title: 'first step title',
        description: 'first step description',
      },
      // ...
    },
  };

  const { Icon, title, description } = onboardingStepsContent[businessContext][step];

  return (
    <Fragment>
      <Icon />
      <h1>{title}</h1>
      <p>{description}</p>
    </Fragment>
  );
}; 
```

有用！现在让我们做得更好。我想让 get 内容更有弹性。例如，如果它接收到一个不存在的步骤呢？这些是使用案例:

*   租赁业务环境的第一步
*   租赁业务环境的最后一步
*   销售业务环境的第一步
*   销售业务上下文的最后一步
*   租赁业务上下文中不存在的步骤
*   销售业务上下文中不存在的步骤

让我们看看测试:

```
describe('getOnboardingStepContent', () => {
  describe('when it receives existent businessContext and step', () => {
    it('returns the correct content for the step in "alugar" businessContext', () => {
      const businessContext = 'alugar';
      const step = 0;

      expect(getOnboardingStepContent({ businessContext, step })).toMatchObject({
        Icon: Home,
        title: 'first step title',
        description: 'first step description',
      });
    });

    it('returns the correct content for the step in "vender" businessContext', () => {
      const businessContext = 'vender';
      const step = 5;

      expect(getOnboardingStepContent({ businessContext, step })).toMatchObject({
        Icon: ContractSign,
        title: 'last step title',
        description: 'last step description',
      });
    });
  });

  describe('when it receives inexistent step for a given businessContext', () => {
    it('returns the first step of "alugar" businessContext', () => {
      const businessContext = 'alugar';
      const step = 7;

      expect(getOnboardingStepContent({ businessContext, step })).toMatchObject({
        Icon: Home,
        title: 'first step title',
        description: 'first step description',
      });
    });

    it('returns the first step of "vender" businessContext', () => {
      const businessContext = 'vender';
      const step = 10;

      expect(getOnboardingStepContent({ businessContext, step })).toMatchObject({
        Icon: Home,
        title: 'first step title',
        description: 'first step description',
      });
    });
  });
}); 
```

太好了！现在让我们构建我们的`getOnboardingStepContent`函数来处理这个逻辑。

```
const getOnboardingStepContent = ({ businessContext, step }) => {
  const content = onboardingStepsContent[businessContext][step];

  return content
    ? content
    : onboardingStepsContent[businessContext][0];
}; 
```

我们试图得到满足。如果我们有，就退回去。如果我们没有，返回入职的第一步。

整洁！但是我们可以改进它。使用`||`操作符怎么样？不需要给变量赋值，不需要用三元。

```
const getOnboardingStepContent = ({ businessContext, step }) =>
  onboardingStepsContent[businessContext][step] ||
  onboardingStepsContent[businessContext][0]; 
```

如果它找到了内容，就返回它。如果没有找到，返回给定业务上下文的第一步。

现在我们的组件只有 UI。

```
const Content = ({ businessContext, step }) => {
  const {
    Icon,
    title,
    description,
  } = getOnboardingStepContent({ businessContext, step });

  return (
    <Fragment>
      <Icon />
      <h1>{title}</h1>
      <p>{description}</p>
    </Fragment>
  );
}; 
```

* * *

## 最后的想法

我喜欢深入思考我正在编写的测试。我认为所有的开发者也应该这样做。它确实需要给我们信心来发布更多的代码，并对我们正在努力的市场产生更大的影响。

像所有的代码一样，当我们写了又臭又差的测试时，它会影响其他开发人员遵循“模式”。越大的公司情况越糟。它伸缩性很差。但是我们总是能够停下来，反思现状，并采取行动让它变得更好。

我分享了一些我觉得有趣的阅读和学习资源。如果你想对 TDD 有一个很好的介绍，我真的推荐《TDD 举例》, Kent Beck 的一本书。

我将写更多关于测试、TDD 和 React 的内容。以及我们如何使我们的软件更加一致，并在将代码交付到产品时感到安全。

## 属国

*   jest-axe : jest 匹配器，用于测试可访问性
*   [testing-library/react-testing-library](https://github.com/testing-library/react-testing-library):帮助测试 react 的测试工具
*   testing-library/jest-dom :测试 dom 状态的 jest 匹配器

## 资源

*   [初级 JavaScript 课程](https://BeginnerJavaScript.com/friend/LEANDRO)
*   [React for 初学者课程](https://ReactForBeginners.com/friend/LEANDRO)
*   [高级反应课程](https://AdvancedReact.com/friend/LEANDRO)
*   [ES6 课程](https://ES6.io/friend/LEANDRO)
*   [学习之路有什么反应](https://www.educative.io/courses/the-road-to-learn-react?aff=x8bV)
*   [学习 React 前的 JavaScript 基础知识](https://www.educative.io/courses/javascript-fundamentals-before-learning-react?aff=x8bV)
*   [重新引入 React: V16 及更高版本](https://www.educative.io/courses/reintroducing-react-v16-beyond?aff=x8bV)
*   [带挂钩的高级反应模式](https://www.educative.io/courses/advanced-react-patterns-with-hooks?aff=x8bV)
*   [实用冗余](https://www.educative.io/courses/practical-redux)
*   [为期一个月的 JavaScript 课程](https://mbsy.co/lFtbC)
*   Kent Beck 所著的《测试驱动开发示例》一书
*   马克·伊森·特罗斯特勒的《可测试的 Javascript》一书
*   [博文源代码](https://github.com/tk-notes/blog/tree/master/tdd-functions-and-react-components)
*   [用 jest、jest-axe 和 react-testing-library 测试 React 应用](https://medium.com/hackernoon/testing-react-with-jest-axe-and-react-testing-library-accessibility-34b952240f53)
*   现代反应测试，第 3 部分:Jest 和反应测试库
*   [我们在世界上最难访问的网页上测试工具时发现了什么](https://accessibility.blog.gov.uk/2017/02/24/what-we-found-when-we-tested-tools-on-the-worlds-least-accessible-webpage/)
*   [测试实施细节](https://kentcdodds.com/blog/testing-implementation-details)
*   [通过构建 App 学习 React](https://alterclass.io/?ref=5ec57f513c1321001703dcd2)

你可以在我的博客上发表类似这样的文章。