# 如何通过这些新的重构使计算测试更简单、更有表现力

> 原文：<https://www.freecodecamp.org/news/how-to-test-complicated-calculations-new-refactoring/>

让测试尽可能具有描述性是一个好主意——将[测试实现为文档](http://xunitpatterns.com/Goals%20of%20Test%20Automation.html#Tests%20as%20Documentation)。在测试中包括计算是很大的一部分，并且避免了[硬编码测试数据](http://xunitpatterns.com/Obscure%20Test.html#Hard-Coded%20Test%20Data)的味道。

例如，如果一个测试看起来像这样，当它失败时，很难知道问题是什么，因为测试数据都是硬编码的。该测试很少告诉你被测系统(SUT)应该如何运行，因此不能作为文档。

```
assert sut.wake_erosion_rate(0.03) == 0.8 
```

然而，如果您去除了这种代码味道，并且在测试中包含了计算，那么当出现故障时，问题是什么就变得很明显了，并且测试现在确实作为文档了。

您可能还想用它们所代表的常量的名称来替换`2.5`和`0.05`。

```
ambient_turbuluence = 0.03

assert 
    sut.wake_erosion_rate(ambient_turbuluence) 
    == 2.5 * ambient_turbuluence + 0.05 
```

本文引用了优秀的 [XUnit 测试模式书](https://www.goodreads.com/review/show/2179089513)，它定义了最广泛接受的单元测试模式和实践词典。

## 示例计算

本文的其余部分将讨论[示例 ConstructionMarginCalculator 类](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/cash_flow_calculator/construction_margin_calculator.py#L31)，并将描述简化测试并允许它们可行地包含计算的重构。

该计算大约有 30 行长，如果您有时间的话，值得快速浏览一下。但是如果没有，不要担心，这篇文章的其余部分仍然有意义。

有几个 if 语句和一个循环，总的来说，这些导致了代码中的 2^9 或 512 条路径！Eek！显然，测试所有这些是不可行的(或没有用的),因此需要找到使其更容易的方法。

这个示例代码有一个[初始测试](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/tests/test_construction_margin_calculator.py#L17)，大约 30 行长。它不包括计算，引入计算会使测试更长更复杂，所以很难证明。

以下每一节都描述了一个重构，并包含指向重构后的示例代码的链接。这些构建在来自 [XUnit 测试模式](https://www.goodreads.com/review/show/2179089513)的现有[测试重构](http://xunitpatterns.com/Test%20Refactorings.html)之上。

重构减少了代码中的路径数量，简化了测试。这意味着需要更少的测试，并且测试变得足够小和简单以包括计算。

## 从循环中提取计算

代码最初为 `steps`的*列表计算值，这意味着对它的任何测试都必须考虑这种复杂性。*

测试的设置更加困难，因为我们必须创建一个列表，而不是单个值。断言更难，因为我们必须断言一个列表，而不是单个值。最后，我们应该有多个测试(可能针对列表中的 0、1 和多个项目)。

简单的解决方法就是重构代码，使它只计算一个`step`。这将迭代代码转移到其他可能需要测试的地方。然而，这可以使用 Mock 来测试，所以只需要测试迭代(相对于迭代*和*计算)，这很简单，并且可能简单到你不需要测试它。

这种重构意味着不再需要 3 次测试(针对列表中的 0、1 和多个项目)，现在只需要一次。这就减少了从 2^9 到 2^7 的编码路径，即 128 条。这已经好了很多，但仍然太多，无法测试！

*   [重构计算](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/cash_flow_calculator/construction_margin_calculator_without_loop.py#L31)
*   [重构测试](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/tests/test_construction_margin_calculator_remove_loop.py#L11)

## 引入可模仿的抽象

示例中没有显示通货膨胀计算的细节，但是它们相当复杂。

为了避免这种情况，我们可以修改代码来接受一个`InflationCalculator`。膨胀计算总是使用相同的`date_of_financial_close`、`inflation_rate`和`inflation_mode`，这意味着它可以在构造函数中采用这些。这意味着主计算不再需要`inflation_rate`和`inflation_mode`。

然后在测试中，我们可以创建一个模拟`InflationCalculator`。举例来说，这可以总是返回一个值`2`，这使得整个计算简单了很多。

也很容易想象通货膨胀计算将发生在代码的其他部分，因此抽象将广泛有用。

这一步意味着在通货膨胀计算中，现在只有一个分支，而不是 4 个条件分支。这就减少了从 2^7 到 2^3 的编码路径，也就是 8 条。还需要测试`InflationCalculator`，它有 4 个分支，所以需要 4 个测试，但是总共仍然只有 12 个测试。松耦合代码万岁！

我们现在有一些可行的测试要写，但是在测试中包含计算仍然会非常麻烦。幸运的是，我们还有一些重构的内容。

*   [重构计算](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/cash_flow_calculator/construction_margin_calculator_mockable_abstraction.py#L29)
*   [重构测试](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/tests/test_construction_margin_calculator_mockable_abstraction.py#L11)

## 隔离测试条件分支

代码基于某些条件分支。我们可以简单地创建一些条件`False`，然后单独测试代码的每个分支。这样，每个测试只需要包括它所关注的分支的计算。

例如，我们可以将`in_selling_mode`设置为`False`，将`step.start_of_step`设置为不同于`date_of_financial_close`。这使得测试足够简单，它可以执行与代码相同的计算。

这反过来意味着测试清楚地传达了`turbine_cost_including_margin`应该是`turbine_costs * fraction_of_spend * inflation`。这有助于读者理解计算器，并达到将[测试作为文档](http://xunitpatterns.com/Goals%20of%20Test%20Automation.html#Tests%20as%20Documentation)的目的

目前测试还很长。然而，因为我们只测试了计算的一小部分，所以很多信息现在是不相关的(变量`any_double`)。这意味着我们现在可以创建一个[测试数据构建器](http://natpryce.com/articles/000714.html)，或者使用助手函数使事情更加简洁。我们将在下一个重构中看到这样的例子。

*   [计算无变化](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/cash_flow_calculator/construction_margin_calculator_mockable_abstraction.py#L29)
*   [重构测试](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/tests/test_construction_margin_calculator_isolate_branches.py#L16)

## 孤立测试值

“隔离测试条件分支”是一种有用的技术，但是如果在多个分支中计算/更新一个值，它仍然会给我们留下一些麻烦。

一个很好的例子是`balance_of_plant_cost_including_margin`，它最初被设置，然后在`if (in_selling_mode)`分支中被更新。

隔离测试`balance_of_plant_cost_including_margin`允许测试只关注这一个值/计算，这意味着需要的设置要少得多。[测试数据构建器](http://natpryce.com/articles/000714.html)模式隐藏了[无关信息](http://xunitpatterns.com/Obscure%20Test.html#Irrelevant%20Information)，测试变得更加简洁明了。

包括计算继续使测试清楚地传达其意图并作为文档。有趣的是，测试计算代码不再是 SUT 计算代码的精确副本，因为它已经知道它是`in_selling_mode`，所以不需要条件语句。这意味着测试比代码简单，这有助于避免[晦涩的测试](http://xunitpatterns.com/Obscure%20Test.html)味道。

*   [计算无变化](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/cash_flow_calculator/construction_margin_calculator_mockable_abstraction.py#L29)
*   [重构测试](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/tests/test_construction_margin_calculator_isolate_values.py#L18)

## 孤立地测试部分值

有时候单个值的计算非常复杂，不能用条件分支拆分。这使得在测试中包括计算具有挑战性。`construction_profit`是一个合理的例子，计算如下:

```
step.turbine_cost_including_margin = 
 turbine_costs * inflation * fraction_of_spend;

step.balance_of_plant_cost_including_margin = 
 balance_of_plant_costs_at_financial_close * inflation * fraction_of_spend;

step.construction_profit = 
 -1 * 
 (step.turbine_cost_including_margin + step.balance_of_plant_cost_including_margin) *
 epc_margin 
```

`inflation`、`fraction_of_spend`和`epc_margin`具有乘法效应，所以如果我们将它们设置为`1`，它们将不会有任何影响，我们可以轻松地为其余的逻辑编写一个测试。

`step.turbine_cost_including_margin`和`step.balance_of_plant_cost_including_margin`有叠加效应，所以如果我们将它们设置为`0`，它们也不会有任何影响，我们可以轻松地为其余的逻辑编写一个测试。

单独测试`construction_profit`的一部分允许测试集中在计算的这一部分，这和以前一样，使测试更短更简单，并且避免了[模糊测试](https://hackmd.io/4xuiUbrAQimsWCknJiqXNw?both)的味道。

*   [计算无变化](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/cash_flow_calculator/construction_margin_calculator_mockable_abstraction.py#L29)
*   [重构测试](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/tests/test_construction_margin_calculator_isolate_partial_values.py#L14)

## 介绍黑板模式

黑板模式是一种更复杂的技术，通常很难理解。但本质上，它涉及到使用“黑板”来打破复杂计算中的依赖性。

在这个例子中，`construction_profit`取决于`turbine_cost_including_margin`和`balance_of_plant_cost_including_margin`，它们本身是根据输入计算的。这使得测试更加困难。

为了测试`construction_profit`，你基本上也必须测试`turbine_cost_including_margin`和`balance_of_plant_cost_including_margin`。

当我们引入黑板模式时，一个计算器将`turbine_cost_including_margin`和`balance_of_plant_cost_including_margin`写到黑板上，当计算`construction_profit`时，我们从黑板上读取这些值。

这打破了两者之间的联系，所以在测试时，我们可以将`turbine_cost_including_margin`和`balance_of_plant_cost_including_margin`的值添加到黑板上。

*   [重构计算](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/cash_flow_calculator/construction_margin_calculator_blackboard_pattern.py#L15)
*   [重构测试](https://github.com/ceddlyburge/unit-testing-calculations/blob/main/tests/test_construction_margin_calculator_blackboard_pattern.py#L12)

## 结论

测试计算时，将计算包含在测试中是很重要的。这避免了[硬编码的测试数据](http://xunitpatterns.com/Obscure%20Test.html#Hard-Coded%20Test%20Data)的味道，允许测试清楚地表达他们的意图，并实现作为文档的[测试。](http://xunitpatterns.com/Goals%20of%20Test%20Automation.html#Tests%20as%20Documentation)

本文中描述的重构允许您这样做，同时还确保测试简明易懂。