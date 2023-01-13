# 长期、敏捷的需求文档

> 原文：<https://www.freecodecamp.org/news/long-term-agile-documentation-of-requirements/>

在我的培训课程中，我们讨论许多话题。包括:在敏捷环境中，你如何记录长期的需求？

文档是存储的知识。随着事情被遗忘，它的价值随着时间而增加。这就是为什么我认为长期文档的问题很有趣。

我想从在敏捷环境中没有意义的长期文档的两个选项开始。那么我想指出明智的选择。各有利弊。

#### 不是一个有用的选项:详细的预先规格

提前详细指定所有需求是没有意义的。在复杂的环境中，变化频繁。需求被重新划分优先级。这是敏捷开发的优势之一。考虑了一些需求，但从未实施。或者没有按计划进行，因为你在发展中获得了新的知识。

讨论和记录需求需要时间。如果需求没有按照文档实现，那就是浪费时间。发展中急需的时间。

#### 也不是一个有用的选择:积压

假设你开始以敏捷的方式工作。也许你认为:没有任何详细的规范。而是积压。让我们用它来记录长期的需求。

但是积压服务于未来，而不是过去。这更像是一份待办事项清单。接下来我们实施什么？积压对于长期文档来说不是一个明智的选择。它没有记录已经实现的内容。

#### 选项 1:实现后归档用户故事

在一次培训课程中，一位参与者告诉我，他的公司在 JIRA 管理用户故事。开发人员在实现后将它们归档。当然，你可以搜索这个档案。该参与者报告说，这对他们很有效。

一个敏捷的实用主义者很难不同意。有用的，有用的。至少在一定的背景下。我认为这种方法有两个风险:

*   **太多细节:**为了能够长期使用这些故事，你当然必须记录许多细节。细节不能按计划实施怎么办？用户故事之后会调整吗？这些故事可能不再正确地记录实现。
*   **增量文档而不是系统文档:**用户故事描述需要做什么。从一个状态到另一个状态的“增量”。为了找出当前状态，可能需要分析几个过去的用户案例。故事缺乏上下文信息。它们不是系统文档，而只是小片段。

#### 选项 2:系统文件的增量调整

该文档可以持续维护。在 Scrum 冲刺阶段，你记录当前的状态。刚刚实现的要求。文档会随着时间的推移而增长。它是递增补充的。

如果你坚持遵循这种方法，它有很大的优势。系统文档总是最新的。它记录了哪些需求实际上已经被实现。

这种选择的一个挑战是纪律。只有一致地记录，文档才会保持最新。这需要时间。

此外，并不是每个开发人员都是天生的文档作者。但是，如果开发人员不记录他们自己，而是委托给其他员工，那么就有信息丢失的风险。

在团队中推广这种纪律的一个方法是将它定义为完成。类似于“系统文档已更新”。在 Sprint Review 中检查。

#### 选项 3:代码中的需求

一种完全被低估的长期文档是软件的代码。如果您适当地构造代码并使用命名约定，您可以从代码中生成文档。

为了实现这一点，我开发了一个库。有了它，你可以在代码中指定可执行的用例模型。他们的行为[类似于一个国家机器](https://www.freecodecamp.org/news/kissing-the-state-machine-goodbye/)。下面是一个信用卡用例的代码示例:

```
 Model model = Model.builder()
	  .useCase("Use credit card")
	    .basicFlow()
	    	.step(ASSIGN).user(requestsToAssignLimit).system(assignsLimit)
	    	.step(WITHDRAW).user(requestsWithdrawal).system(withdraws).reactWhile(accountOpen)
	    	.step(REPAY).user(requestsRepay).system(repays).reactWhile(accountOpen)

	    .flow("Withdraw again").after(REPAY)
	    	.step(WITHDRAW_AGAIN).user(requestsWithdrawal).system(withdraws)
	    	.step(REPEAT).continuesAt(WITHDRAW)

	    .flow("Cycle is over").anytime()
	    	.step(CLOSE).on(requestToCloseCycle).system(closesCycle)

	    .flow("Assign limit twice").condition(limitAlreadyAssigned)
	    	.step(ASSIGN_TWICE).user(requestsToAssignLimit).system(throwsAssignLimitException)

	    .flow("Too many withdrawals").condition(tooManyWithdrawalsInCycle) 
	    	.step(WITHDRAW_TOO_OFTEN).user(requestsWithdrawal).system(throwsTooManyWithdrawalsException)
	.build();
```

从该代码生成的文档[如下。](https://github.com/bertilmuth/requirementsascode/tree/master/requirementsascodeextract)

生成的文档-开始

### **使用信用卡**

#### **基本流程**

**分配额度**:用户请求分配额度。系统分配限制。
**取款**:只要开户:用户请求取款。系统退出。
**还款**:只要开户:用户请求还款。系统偿还。

#### 再次撤回

还款后:
**再次取款**:用户请求取款。系统退出。
**重复**:系统继续退出。

#### 周期结束了

随时:
**关闭周期**:处理请求关闭周期:系统关闭周期。

#### 分配两次限制

任何时候，当限额已经分配:
**分配限额两次**:用户请求分配限额。系统抛出分配限制异常。

#### 太多取款

随时，周期内多次取款:
**取款太频繁**:系统抛出多次取款异常。

生成的文档-结束

相同的代码控制应用程序的行为，并且是文档的来源。优势是显而易见的:您可以轻松地生成文档。它反映了软件的实际行为。

当然，这种方法也需要纪律。尤其是开发商这边。在使用任何方法之前，您都应该尝试一下。是否适合正在开发的软件类型？

此外，你不能用这样的方法达到完整性。例如，您不能从代码中生成像健壮性这样的质量需求。设计权衡也不是代码的一部分。

我期待你的反馈。您使用哪些长期文档选项？

*这篇文章最初以德文发表在* [*胡德博客*](https://blog.hood-group.com/blog/2017/05/10/anforderungen-langfristig-dokumentieren-im-agilen-umfeld/) *上。如果你想了解我正在做的事情或给我留言，请在 [dev.to](https://dev.to/bertilmuth) 、 [LinkedIn](https://www.linkedin.com/in/bertilmuth/) 或 [twitter](https://twitter.com/BertilMuth) 上关注我。或者访问我的 [GitHub 项目](https://github.com/bertilmuth/requirementsascode)。*