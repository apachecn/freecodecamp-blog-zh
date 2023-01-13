# 简化事件源应用程序

> 原文：<https://www.freecodecamp.org/news/simplifying-an-event-sourced-application/>

每次对应用程序状态进行更改时，都会将更改记录为一个事件。
您可以回放从录像开始到特定时间的事件。那么您已经重新创建了当时应用程序的状态。

这就是[事件采购](https://martinfowler.com/eaaDev/EventSourcing.html)的目的。就好像你可以穿越时空回到过去。我觉得这很有趣。

当您需要满足法规要求时，事件源可提供审计跟踪。它有助于调试。你甚至可以探索不同的现实:如果...

我最近看了[雅各布·皮里蒙](https://twitter.com/JakubPilimon)和[肯尼·巴斯塔尼](https://twitter.com/kennybastani)关于活动采购的[精彩演讲](https://youtu.be/r7AGQsM7ncA)。

这是一个 1 小时的生命编码讲座。两位演讲者从一个简单的应用程序开始，这个应用程序是由*而不是*事件发起的。然后他们重构它来使用事件。

他们最终用 Apache Kafka 连接了应用程序。在本文中，我将跳过这一部分，转而关注事件源的概念部分。

## 讲话的摘要

作为信用卡管理应用程序的用户，您可以:

*   给信用卡设定限额
*   取钱
*   还钱

对于这些命令中的每一个，在`CreditCard`类中都有一个方法。
下面是`assignLimit`方法的原始代码:

```
public void assignLimit(BigDecimal amount) { 
  if(limitAlreadyAssigned()) {  
    throw new IllegalStateException(); 
  }
  this.initialLimit = amount; 
} 
```

下面是`withdraw`方法:

```
 public void withdraw(BigDecimal amount) {
        if(notEnoughMoneyToWithdraw(amount)) {
            throw new IllegalStateException();
        }
        if(tooManyWithdrawalsInCycle()) {
            throw new IllegalStateException();
        }
        this.usedLimit = usedLimit.add(amount);
        withdrawals++;
    } 
```

`repay`方法类似。

请记住，对于事件源，您需要在应用程序改变其状态时记录一个事件
？
所以说话者将每个状态变化提取到[信用卡](https://gitlab.com/pilloPl/eventsourced-credit-cards/blob/4329a0aac283067f1376b3802e13f5a561f18753/src/main/java/io/pillopl/eventsourcing/model/CreditCard.java)类中它自己的方法中。

下面是重构后的`withdraw`方法:

```
 public void withdraw(BigDecimal amount) {
        if(notEnoughMoneyToWithdraw(amount)) {
            throw new IllegalStateException();
        }
        if(tooManyWithdrawalsInCycle()) {
            throw new IllegalStateException();
        }
        cardWithdrawn(new CardWithdrawn(uuid, amount, Instant.now()));
    }

    private CreditCard cardWithdrawn(CardWithdrawn event) {
        this.usedLimit = usedLimit.add(event.getAmount());
        withdrawals++;
        pendingEvents.add(event);
        return this;
    } 
```

`CardWithdrawn`的一个实例表示用户成功取款的事件。*在*状态改变后，事件被添加到未决事件列表中。

您调用 [CreditCardRepository](https://gitlab.com/pilloPl/eventsourced-credit-cards/blob/4329a0aac283067f1376b3802e13f5a561f18753/src/main/java/io/pillopl/eventsourcing/persistence/CreditCardRepository.java) 类的`save`方法将未决事件刷新到事件流中。事件监听器然后可以处理事件。

除了有效负载之外，每个事件都有自己唯一的标识符和时间戳。以便您可以在以后对事件进行排序和重放。
为了重放特定信用卡的事件，存储库调用[信用卡](https://gitlab.com/pilloPl/eventsourced-credit-cards/blob/4329a0aac283067f1376b3802e13f5a561f18753/src/main/java/io/pillopl/eventsourcing/model/CreditCard.java)类的`recreateFrom`方法，传入信用卡的 id 和为其存储的事件:

```
 public static CreditCard recreateFrom(UUID uuid, List<DomainEvent> events) {
        return ofAll(events).foldLeft(new CreditCard(uuid), CreditCard::handle);
    }

    private CreditCard handle(DomainEvent event) {
        return Match(event).of(
                Case($(Predicates.instanceOf(LimitAssigned.class)), this::limitAssigned),
                Case($(Predicates.instanceOf(CardWithdrawn.class)), this::cardWithdrawn),
                Case($(Predicates.instanceOf(CardRepaid.class)), this::cardRepaid),
                Case($(Predicates.instanceOf(CycleClosed.class)), this::cycleWasClosed)
        );
    }
```

这段代码使用 [vavr.io](http://www.vavr.io/) 库为每个事件调用`handle`方法。`handle`方法将事件对象分派给适当的方法。
例如:对于每个`LimitAssigned`事件，`handle`方法调用`limitAssigned`方法，将事件作为参数。

## 简化应用程序

为了简化代码，我使用了[需求作为代码](https://github.com/bertilmuth/requirementsascode)库。首先，我将所有的事件类和处理方法放在一个模型中。像这样:

```
this.eventHandlingModel = 
        Model.builder()
           .on(LimitAssigned.class).system(this::limitAssigned)
           .on(CardWithdrawn.class).system(this::cardWithdrawn)
           .on(CardRepaid.class).system(this::cardRepaid)
           .on(CycleClosed.class).system(this::cycleWasClosed)
       .build(); 
```

我不得不将处理方法的返回类型(例如`limitAssigned`)改为`void`。除此之外，从 [vavr.io](http://www.vavr.io/) 的转换非常简单。

然后，我创建了一个流道，并开始为模型:

```
this.modelRunner = new ModelRunner();
modelRunner.run(eventHandlingModel); 
```

之后，我将`recreateFrom`和`handle`方法改为:

```
public static CreditCard recreateFrom(UUID uuid, List<DomainEvent> events) {
    CreditCard creditCard = new CreditCard(uuid);
    events.forEach(ev -> creditCard.handle(ev));
    return creditCard;
}

private void handle(DomainEvent event) {
    modelRunner.reactTo(event);
} 
```

此时，我可以摆脱对 [vavr.io](http://www.vavr.io/) 的依赖。
跃迁完成。现在我可以做更多的简化工作了。

我重新审视了`withdraw`方法:

```
public void withdraw(BigDecimal amount) {
    if(notEnoughMoneyToWithdraw(amount)) {
        throw new IllegalStateException();
    }
    if(tooManyWithdrawalsInCycle()) {
        throw new IllegalStateException();
    }
    cardWithdrawn(new CardWithdrawn(uuid, amount, Instant.now()));
} 
```

检查`tooManyWithdrawalsInCycle()`不依赖于事件的数据。这只取决于`CreditCard`的状态。
像这样的状态检查可以在模型中表示为`conditions`。

在我将所有方法的所有状态检查转移到模型中之后，它看起来像这样:

```
this.eventHandlingModel = 
  Model.builder()
    .condition(this::limitNotAssigned)
        .on(LimitAssigned.class).system(this::limitAssigned)
    .condition(this::limitAlreadyAssigned)
        .on(LimitAssigned.class).system(this::throwsException)
    .condition(this::notTooManyWithdrawalsInCycle)
        .on(CardWithdrawn.class).system(this::cardWithdrawn)
    .condition(this::tooManyWithdrawalsInCycle)
        .on(CardWithdrawn.class).system(this::throwsException)
    .on(CardRepaid.class).system(this::cardRepaid)
    .on(CycleClosed.class).system(this::cycleWasClosed)
.build(); 
```

为此，我需要用`handle`方法替换对改变状态的方法的直接调用。之后，`assignLimit`和`withdraw`方法看起来像这样:

```
public void assignLimit(BigDecimal amount) { 
    handle(new LimitAssigned(uuid, amount, Instant.now()));
}

private void limitAssigned(LimitAssigned event) {
    this.initialLimit = event.getAmount(); 
    pendingEvents.add(event);
}

public void withdraw(BigDecimal amount) {
    if(notEnoughMoneyToWithdraw(amount)) {
        throw new IllegalStateException();
    }
    handle(new CardWithdrawn(uuid, amount, Instant.now()));
}

private void cardWithdrawn(CardWithdrawn event) {
    this.usedLimit = usedLimit.add(event.getAmount());
    withdrawals++;
    pendingEvents.add(event);
} 
```

正如您所看到的，大多数条件逻辑已经从方法中转移到了模型中。这使得这些方法更容易理解。

困扰我的一件事是，你一定不能忘记将事件添加到未决事件中。每次都是。否则你的代码就不会工作。

[需求代码](https://github.com/bertilmuth/requirementsascode)允许您控制系统如何处理事件。所以我也从方法中提取了`pendingEvents.add(event)`:

```
modelRunner.handleWith(this::addingPendingEvents);
...

public void addingPendingEvents(StepToBeRun stepToBeRun) {
    stepToBeRun.run();
    DomainEvent domainEvent = (DomainEvent) stepToBeRun.getEvent().get();
    pendingEvents.add(domainEvent);
} 
```

我本可以更进一步，提取验证逻辑。亲爱的读者，我把这个留给你做思考练习。

## 有什么意义？

我试图实现的是关注点的清晰分离:

*   方法的状态依赖执行在模型中定义
*   数据验证和状态更改在方法的实现中
*   事件会自动添加到未决事件中。总的来说:基础设施代码与业务逻辑是明确分离的。

简化一个已经很简单的例子有利于解释。但这不是我想说的重点。

关键是:在实践中，如此清晰的关注点分离是有好处的。特别是，如果你和多个团队一起工作。复杂问题上。

关注点的分离有助于以不同的速度改变代码的不同部分。在哪里找东西有简单的规则。代码更容易理解。并且更容易隔离单元用于测试目的。

## 结论

我希望你喜欢我的文章。请给我反馈。

你一直在开发活动采购应用程序吗？你的经历是怎样的？你能理解我在这篇文章中写的内容吗？

我还想邀请你看看我在整篇文章中使用的库。如果你在实践中尝试一下，并告诉我你的想法，我会很激动。

*本文最初发表于[dev . to](https://dev.to/bertilmuth/simplifying-an-event-sourced-application-1klp)*