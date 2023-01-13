# 开闭原则——用简单的英语解释的软件开发概念

> 原文：<https://www.freecodecamp.org/news/open-closed-principle/>

有很多关于开闭原则的文章，但是我从来没有找到一篇以一种真正适合我的方式来解释它。

所以，希望这是一个好的例子——有一个真实的例子，支持什么样的改变，以及权衡的描述。

开闭原则规定代码应该“开放以供扩展”和“封闭以供修改”。

关于术语有相当多的混淆，但本质上它意味着如果你想实现一个*支持的*变更，你应该能够在不改变大量代码的情况下实现。理想情况下，您可以只通过添加新代码来实现新功能，并且只需修改很少或不修改旧代码，这使得代码更易于开发和维护。

Open 是[固体设计原则](https://www.freecodecamp.org/news/solid-principles-explained-in-plain-english/)中的‘O’，这可能是编写高质量代码最著名的指南。

没有一个有用的代码能够对所有可能的变化完全开放，所以我们必须决定哪些变化我们将*支持*。当我们写代码时，我们可以考虑潜在的变化可能是什么，决定哪些*支持*，然后让代码对这些“开放”。

我们可以通过以下方式创建这些潜在变化的列表:

*   分析代码
*   查看以前对代码的更改
*   利用我们通常要求变更的经验
*   使用即将到来的特征请求的任何知识

花一点时间看看下面的代码([和 GitHub 上的](https://github.com/ceddlyburge/open-closed-principle/blob/master/OpenClosedPrinciple/Original/GrossToNetCalculator.cs))，想想我们可能会期待什么样的变化。您无法访问提交历史，也不了解即将到来的特性请求，但是您仍然可以提出一些可能的候选对象。

```
public class GrossToNetCalculator
{
    public GrossToNetCalculator(
        IGrossEnergyYield grossYield,
        double grossEnergy,
        double hysteresisLoss,
        double curtailmentLossGrid,
        double turbineLossTurbulence,
        double electricalLoss,
        double turbineLossShear,
        double turbinePerformanceExperience,
        double operationalExperienceLoss)
    {
        double dependentLoss = 
            CombinePercentages(
                grossYield.TurbineAvailability,
                grossYield.BalanceAvailability,
                grossYield.AccessibilityAvailability,
                hysteresisLoss,
                electricalLoss,
                grossYield.EnvironmentalShutdownWeather,
                grossYield.EnvironmentalSiteAccess,
                grossYield.EnvironmentTreeGrowth);

        double independentLoss = 
            CombinePercentages(
                grossYield.GridAvailability,
                turbinePerformanceExperience,
                turbineLossTurbulence,
                grossYield.EnvironmentalPerformanceDegradationIcing,
                grossYield.CurtailmentPowerPurchase,
                grossYield.SubOptimalPerformance,
                turbineLossShear,
                operationalExperienceLoss);

        GrossToNet = 
        	1 - 
            (1 - (dependentLoss + curtailmentLossGrid))
            * (1 - independentLoss);
    }

    double CombinePercentages(params double[] percentages)
    {
        double combination = 1;
        foreach (var percentage in percentages)
            combination *= 1 - percentage;
        return 1 - combination;
    }

    public double GrossToNet { get; private set; }
}
```

这段代码相对简单，我看到的潜在变化如下:

*   可以从`dependentLoss`和`independentLoss`计算中添加或删除项目。物品也可以在`dependentLoss`和`independentLoss`之间移动，但本质上是一样的
*   `GrossToNet`的计算可能会改变
*   `CombinePercentages`计算可能会改变

正如计算机编程中的大多数事情一样，在应用开闭原则时会有一种紧张感。

一方面，使代码更容易扩展是好的。另一方面，这样做通常会破坏封装，增加复杂性，并增加不必要的抽象层次。

因此，我们再次需要决定我们想要支持这些变化中的哪一个，并使代码“对”*开放。这样我们就可以避免因不合适的变更而给代码增加不必要的复杂性。*

值得记住的是，工作总是可以在以后完成，那时会更容易，因为我们会确切地知道需要什么。

为了决定我们应该支持什么样的变化并使代码“开放”，我们需要估计变化发生的可能性，考虑设计解决方案，然后考虑权衡。

## 我们可以在相关损失和独立损失计算中添加或删除项目

### 很可能会改变

`dependentLoss`和`independentLoss`(例如`double dependentLoss = CombinePercentages(...)`)的计算各使用 8 个参数(`electricalLoss`、`TurbineAvailability`等)。

这 16 个构成了整个计算的 17 个总输入的大部分。因此，从纯统计的角度来看，其中一个的改变有 16/17 (94%)的机会影响这些计算。

也很容易想象，我们可能希望在未来添加另一个“损失”或“可用性”或类似的东西，或者当前的一个不再相关，或者在不同的情况下需要不同的组合。

### 可能的解决方案

在构造函数中列出从属损失和独立损失，而不是单独列出每个损失。所以现有的构造函数:

```
public GrossToNetCalculator(
	...
    double hysteresisLoss,
    double curtailmentLossGrid,
    ...)
```

替换为以下内容:

```
public GrossToNetCalculator(
    IReadOnlyList<double> dependentLosses
    IReadOnlyList<double> independentLosses)
```

这意味着，如果请求了更改，我们可以在不更改类的情况下实现它(而只是更改我们传递给构造函数的参数)。

例如，如果请求另一个“dependentLoss ”,我们可以将它添加到`dependentLosses`列表中。

(你可以在这里看到 GitHub 上的[代码](https://github.com/ceddlyburge/open-closed-principle/tree/master/OpenClosedPrinciple/ListParameters))

### 权衡取舍

少量封装丢失，调用代码现在将负责决定传递哪些丢失。

代码更好地遵循了开闭原则，变得更容易扩展和重用。如果您需要做出改变，您不需要修改测试，这是有用的，因为它们很复杂。

调用代码的测试必须修改，但只是为了验证它们传递了正确的参数，这要简单得多。

可能构造函数参数在代码库中传递，现在只有两个参数，而以前有九个。

### 决定

我们应该实现这个解决方案来支持这种变化，并使代码对它“开放”。

## GrossToNet 的计算可能会改变

### 不太可能改变

GrossToNet 计算是`GrossToNet = 1 - (1 - (dependentLoss + curtailmentLossGrid)) * (1 - independentLoss);`

除了前面提到的`dependentLoss`和`independentLoss`之外，只使用了`curtailmentLossGrid`参数。

这 1 个参数是整个计算的 17 个总输入中的一小部分。因此，从纯统计的角度来看，其中一个的改变有 1/17 (6%)的机会影响这个计算。

### 可能的解决方案

1.  在构造函数中取一个 lambda 参数计算`GrossToNet`并传递给`dependentLoss`和`independentLoss`，这样计算就变成了`GrossToNet = grossToNetCalculatorLambda(dependentLoss, independentLoss)`(GitHub 上的[代码](https://github.com/ceddlyburge/open-closed-principle/tree/master/OpenClosedPrinciple/GrossToNetLambda))
2.  将`curtailmentLossGrid`从计算中移除，这样它就变得完全通用，并且可以被重命名为`PercentageCombiner`。要求调用代码应用这种调整(对于有用的示例代码来说，这种调整太复杂了)
3.  如上所述从计算中移除`curtailmentLossGrid`，然后重新创建原始的`GrossToNetCalculator`，使用`PercentageCombiner`并将`curtailmentLossGrid`添加到计算
    (GitHub 上的[代码)](https://github.com/ceddlyburge/open-closed-principle/tree/master/OpenClosedPrinciple/PercentageCombiner)

### 权衡取舍

选项 1 和 2 丢失了大量封装。选项 3 是合理的工作量，并增加了一个抽象层。

### 决定

这种变化不太可能发生，所以可能不值得花力气去支持它，让代码对它“开放”。但是如果我们对新的`PercentageCombiner`有另一种用途，那肯定是值得的。

## 合并百分比的计算可能会改变

### 不太可能改变

```
CombinePercentages(params double[] percentages)
{
    double combination = 1;
    foreach (var percentage in percentages)
    combination *= 1 - percentage;
    return 1 - combination;
}
```

CombinePercentages 计算实现了一些标准的数学/统计定律，这些定律不会改变。

### 可能的解决方案

1.  在构造函数中采用一个 lambda 参数来组合百分比，并使用它来代替 CombinePercentages 函数。所以不是有`double dependentLoss = CombinePercentages(...)`，而是有`double dependentLoss = combinePercentagesLambda(...)`。
    ([GitHub 上的代码](https://github.com/ceddlyburge/open-closed-principle/tree/master/OpenClosedPrinciple/CombinePercentagesLambda))
2.  创建一个`PercentageCombiner`抽象，在构造函数中使用它来组合百分比，并使用它来代替 CombinePercentages 函数。因此，你将拥有`double dependentLoss = percentageCombiner.CombinePercentages(...)`，而不是`double dependentLoss = CombinePercentages(...)`。
    (GitHub 上的[代码](https://github.com/ceddlyburge/open-closed-principle/tree/master/OpenClosedPrinciple/PercentageCombinerAbstraction))

### 权衡取舍

组合百分比是这段代码的核心，因此删除这种逻辑会使代码变得毫无用处。

选项 1 将这方面的所有责任传递给调用者，而选项 2 至少允许预定义的抽象实现。

### 决定

这种改变是不太可能的，唯一合理的解决方案(选项 2)需要大量的工作，并且增加了复杂性和抽象性。

这意味着只有在实际需要改变的时候才这么做，而且只有在需要多种算法的时候才有意义。请注意，如果需要对算法进行更改，简单地更改 CombinePercentages 函数的实现会更有意义。

## 结论

决定代码是否遵循开闭原则几乎总是一个判断的问题，通常需要在封装、复杂性和抽象之间进行权衡。

值得考虑可能的变化和扩展，并使用这些来指导您的决策。