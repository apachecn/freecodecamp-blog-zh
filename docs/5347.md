# 如何优雅地处理错误:无声地失败不是一个选项

> 原文：<https://www.freecodecamp.org/news/how-to-handle-errors-with-grace-failing-silently-is-not-an-option-de6ce8f897d7/>

作者:Rina Artstain

我对错误处理从来没有什么看法。这可能会让那些知道我非常固执己见的人感到震惊(从好的方面来说！)，但是是啊。如果我进入一个现有的代码库，我只是做他们以前做的事情，如果我从头开始写，我只是做我当时觉得对的事情。

当我最近阅读鲍勃叔叔的《干净代码》中的错误处理部分时，那是我第一次思考这个问题。过了一段时间，我遇到了一个由一些代码静默失败引起的 bug，我意识到可能是时候多考虑一下这个问题了。我可能无法改变我正在处理的整个代码库中错误的处理方式，但至少我会被告知存在哪些方法，权衡是什么，并且，你知道，对这个问题有自己的看法。

### **期待西班牙宗教裁判所**

处理错误的第一步是识别什么时候“错误”不是“错误！”。这当然取决于您的应用程序的业务逻辑，但一般来说，有些错误是显而易见的，很容易修复。

*   有一个“到”在“从”之前的从到日期范围吗？调换顺序。
*   有一个以+开头或包含破折号的电话号码，但其中没有特殊字符？移除它们。
*   空集合有问题吗？确保在访问之前初始化它(使用[惰性初始化](https://en.wikipedia.org/wiki/Lazy_initialization)或在构造函数中)。

不要因为你可以修复的错误而中断你的代码流，当然也不要中断你的用户。如果你能理解这个问题并自己解决它，那就去做吧。

![gMPEtUMvrcUzejIk2Qc1m2yjc-vPWBeLCqAU](img/81ee46ca9f17e74ffa83b25f1811f309.png)

Photo by [Lance Anderson](https://unsplash.com/photos/7DITOdv_Uxo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/expect?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### **返回空值或其他幻值**

空值，-1(预期为正数)和其他“神奇的”返回值——所有这些都是将错误检查的责任推给函数调用方的弊端。这不仅是一个问题，因为它会导致错误检查激增和繁殖，它也是一个问题，因为它依赖于约定，并要求您的用户了解任意的实现细节。

您的代码将充满这样的代码块，它们模糊了应用程序的逻辑:

```
return_value = possibly_return_a_magic_value()if return_value < 0:   handle_error()else:    do_something()
```

```
other_return_value = possibly_nullable_value()if other_return_value is None:   handle_null_value()else:   do_some_other_thing()
```

即使你的语言有一个内置的可空值传播系统——那也只是对脆弱的代码应用一个不可读的补丁:

```
var item = returnSomethingWhichCouldBeNull();var result = item?.Property?.MaybeExists;if (result.HasValue){    DoSomething();}
```

> 向方法传递空值也同样有问题，您经常会看到方法以几行检查输入是否有效开始，但这确实是不必要的。大多数现代语言提供了几个工具，允许你明确你所期望的，并跳过那些代码混乱的检查，例如，定义参数为不可空的或用适当的修饰。

### 错误代码

错误代码与空值和其他幻值有相同的问题，额外的复杂性是必须处理错误代码。

您可能决定通过“out”参数返回错误代码:

```
int errorCode;var result = getSomething(out errorCode);if (errorCode != 0){    doSomethingWithResult(result);}
```

您可以选择将所有结果包装在一个“Result”结构中，就像这样(我对此深感内疚，尽管它在当时对 ajax 调用非常有用):

```
public class Result<T>{   public T Item { get; set; }   // At least "ErrorCode" is an enum   public ErrorCode ErrorCode { get; set; } = ErrorCode.None;    public IsError { get { return ErrorCode != ErrorCode.None; } } }
```

```
public class UsingResultConstruct{   ...   var result = GetResult();   if (result.IsError)   {      switch (result.ErrorCode)      {         case ErrorCode.NetworkError:             HandleNetworkError();             break;         case ErrorCode.UserError:             HandleUserError();             break;         default:             HandleUnknownError();             break;      }   }   ActuallyDoSomethingWithResult(result);   ...}
```

没错。那真的很糟糕。出于某种原因，Item 属性可能仍然为空，没有实际的保证(除了约定之外)当结果不包含错误时，您可以安全地访问 Item 属性。

完成所有这些处理后，您仍然需要将您的错误代码转换成错误消息，并对其进行处理。通常，在这一点上，您已经掩盖了原始问题，以至于您可能不知道发生了什么的确切细节，因此您甚至不能有效地报告错误。

除了这些非常不必要的过度复杂和不可读的代码之外，还有一个更糟糕的问题——如果你或其他人改变你的内部实现，用新的错误代码处理新的无效状态，调用代码将**无法知道**他们需要处理的东西已经改变，并且**将以不可预知的方式使**失败。

### 如果一开始你没有成功，试一试，抓住，最后

在我们继续之前，这可能是一个很好的时机来提及代码无声地失败不是一件好事。无声无息地失败意味着错误在不方便和不可预测的时候突然爆发之前，可能会在相当长的一段时间内未被发现。通常在周末。以前的错误处理方法**允许**你无声地失败，所以也许，仅仅是也许，它们不是最好的方法。

![uJ6t46B9KpmPOCj1t9YGx48xEZcv-LSX-XdH](img/1780bd8686b6e49e7fbf44c72548899c.png)

Photo by [Scott Umstattd](https://unsplash.com/photos/iSTs6Lcu-Ek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/spanish?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这一点上，如果你已经阅读了[干净的代码](https://www.oreilly.com/library/view/clean-code/9780136083238/)，你可能会想为什么有人会这样做，而不是抛出一个异常？如果你没有，你可能会认为异常是万恶之源。我以前也有这种感觉，但现在我不确定了。请耐心听我说，让我们看看我们是否同意异常并不都是坏的，甚至可能非常有用。如果你用一种没有例外的语言写作呢？嗯，事情就是这样。

> 至少对我来说，一个有趣的附带说明是，新 C#方法的默认实现是抛出 NotImplementedException，而新 python 方法的默认实现是“pass”。

> 我不确定这是否是一个 C#惯例，或者只是我的 Resharper 是如何配置的，但结果基本上是将 python 设置为静默失败。我想知道有多少开发人员花了漫长而悲伤的调试时间试图弄清楚发生了什么，却发现他们忘记了实现占位符方法。

但是等等，你可能很容易创建一个混乱的错误检查和异常抛出，这与前面的错误检查部分非常相似！

```
public MyDataObject UpdateSomething(MyDataObject toUpdate){    if (_dbConnection == null)    {         throw new DbConnectionError();    }    try    {        var newVersion = _dbConnection.Update(toUpdate);        if (newVersion == null)        {            return null;        }        MyDataObject result = new MyDataObject(newVersion);        return result;     }     catch (DbConnectionClosedException dbcc)     {         throw new DbConnectionError();     }     catch (MyDataObjectUnhappyException dou)     {         throw new MalformedDataException();     }     catch (Exception ex)     {         throw new UnknownErrorException();     }}
```

所以，当然，抛出异常并不能保护你免受不可读和不可管理的代码。您需要将异常抛出作为一种深思熟虑的策略来应用。如果您的范围太大，您的应用程序可能会以不一致的状态结束。如果你的范围太小，你最终会陷入一片混乱。

我解决这个问题的方法如下:

一致性规则。你必须确保你的应用程序总是处于一致的状态。丑陋的代码让我难过，但没有实际的问题让我难过，不管你的代码实际上在做什么，这些问题都会影响用户。如果这意味着您必须用一个 try/catch 块来包装每一行，那么就将它们隐藏在另一个函数中。

```
def my_function():    try:        do_this()        do_that()    except:        something_bad_happened()    finally:        cleanup_resource()
```

**巩固错误。**如果**你**关心需要不同处理的不同种类的错误，这没什么，但是帮你的用户一个忙，把它藏在内部。在外部，抛出单一类型的异常只是为了让用户知道出了问题。他们不应该真的关心细节，那是你的责任。

```
public MyDataObject UpdateSomething(MyDataObject toUpdate){    try    {                var newVersion = _dbConnection.Update(toUpdate);        MyDataObject result = new MyDataObject(newVersion);        return result;     }     catch (DbConnectionClosedException dbcc)     {         HandleDbConnectionClosed();         throw new UpdateMyDataObjectException();     }     catch (MyDataObjectUnhappyException dou)     {         RollbackVersion();         throw new UpdateMyDataObjectException();     }     catch (Exception ex)     {         throw new UpdateMyDataObjectException();     }}
```

【早抓，勤抓】。在尽可能低的级别上，尽可能靠近源头地捕捉您的异常。保持一致性并隐藏细节(如上所述)，然后尽量避免处理错误，直到应用程序的顶层。希望一路上不会有太多关卡。如果您能够做到这一点，您就能够清楚地将应用程序逻辑的正常流程从错误处理流程中分离出来，从而使您的代码清晰简洁，不会混淆问题。

```
def my_api():    try:        item = get_something_from_the_db()        new_version = do_something_to_item(item)        return new_version    except Exception as ex:        handle_high_level_exception(ex)
```

感谢您阅读本文，希望对您有所帮助！此外，我刚刚开始形成我对这个问题的看法，所以我真的很高兴听到你处理错误的策略是什么。评论区开放了！