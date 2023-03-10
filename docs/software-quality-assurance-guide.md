# 软件质量保证指南

> 原文：<https://www.freecodecamp.org/news/software-quality-assurance-guide/>

## **质量保证**

质量保证(通常称为 QA)是对开发中的产品进行检查的方法，以确保它按预期工作。QA 过程中使用的实际方法根据产品的大小和性质有很大的不同。

对于个人项目，你可能只是边做边测试，在适当的阶段要求他人提供反馈。相比之下，银行应用程序必须对每个功能的每个方面进行彻底的检查和记录，以确保其功能性和安全性。

不管 QA 过程有多正式或详细，它的目的是识别 bug，以便在产品发布前解决它们。

## 方法学

### 敏捷

在敏捷开发方法中，目标是每一个工作周期(“冲刺”)都产生可以迭代添加和改进的工作软件。这使得 QA 过程成为开发周期的固有部分。

通过在产品的每个阶段测试软件组件，您可以降低发布时出现错误的风险。

## 术语

### 自动化测试

使用预先编写的脚本完成测试，这些脚本用于控制测试的执行。

### 黑匣子

这些测试并不检查被测系统的内部，而是将其视为“封闭的”,就像最终用户将体验到的一样。

### 缺点

与应用规范的任何偏差；通常被称为“bug”。

### 探索性测试

一种无脚本的测试方法，它依赖于测试人员独特的创造力，努力寻找未知的错误并识别回归。

### 集成测试

一起测试单个组件/模块，以确保它们彼此连接和交互良好。

### 负路径测试

一种测试场景，旨在产生功能/应用程序中的错误状态，并验证错误是否得到妥善处理。例如，在用户注册表单的电子邮件字段中输入一系列数字，并检查以确保在提供实际电子邮件地址之前不接受注册。

### 回归测试

对新版本进行测试，以确保新功能不会无意中破坏之前测试过的功能。

### 烟雾测试

一种极简的测试方法，旨在确保在更深入的测试发生之前，基本的功能是有效的。通常发生在测试过程的开始。

### 判例案件

QA 测试人员/工程师参考指定的前提条件、步骤和预期结果，以确定某项功能是否按预期执行其任务。

### 白箱

指的是在代码库中的结构级别上执行的测试。程序员检查特定功能或组件的输入和输出是白盒测试。

也称为“玻璃盒”、“透明盒”、“透明盒”，因为测试人员可以“看到”被测系统的内部。

主要类别有

*   ****单元测试**** (代码的单个单元做它们应该做的事情)
*   ****集成测试**** (单元/组件适当交互)
*   ****回归测试**** (在开发的后期重新应用测试以确保它们仍然工作)

有三种主要技术:

*   ****等价划分**** (测试的输入值代表较大的输入数据集)
*   ****边界值分析**** (在行为和输出应改变的情况下，用选定的输入测试系统)
*   **(测试是从输入输出关系的可视化来设计的)**

### ****其他资源****

*   **[如何编写实际有效的 QA 文档](https://www.freecodecamp.org/news/how-to-write-qa-documentation-that-will-work/)**
*   **[测试驱动开发](https://guide.freecodecamp.org/agile/test-driven-development)**
*   **[单元测试](https://guide.freecodecamp.org/software-engineering/unit-tests/)**