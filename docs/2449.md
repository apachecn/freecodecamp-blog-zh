# 如何在 MongoDB 中使用事务来防止 Java 代码中的不一致

> 原文：<https://www.freecodecamp.org/news/mongodb-transactions-in-java/>

最新的 MongoDB 版本引入了[多文档事务](https://docs.mongodb.com/v4.2/core/transactions/)。这是大多数 NoSQL 数据库所缺少的一个关键特性(也是 SQL DBs 所吹嘘的)。

可以由一个或多个操作组成的事务充当原子操作。如果所有子操作都成功，则认为该事务已完成。否则会失败。

这叫做原子性。这是一个需要理解的重要概念，以便在并发读/写数据时保持数据的一致性。

## 文章范围和目标

本文的目标是向您展示一个真实的例子，在没有事务的情况下会出现数据不一致。然后我们将使用 MongoDB 事务在 Java 中构建一个解决方案来防止它们。

通过这样做，您将学会:

1.  避免可能导致数据不一致的[竞争条件](https://en.wikipedia.org/wiki/Race_condition)
2.  通过使用 Mongo 的内置可重试写操作来构建更具弹性的应用程序

此外，我还添加了一个包装函数`static <R> R withTransaction(final Function<ClientSession, R> executeFn);`，您可以用它来提高代码的可读性。

## 示例:如何处理同一银行账户的并发交易

假设你和你的配偶共有一个银行账户。你们每个人同时去自动取款机，开始取钱。

```
t1 -> You: Press check balance. ATM shows 100 dollars
t2 -> Spouse: Press check balance. ATM shows 100 dollars
t3 -> You & Spouse: withdraw 10 dollars
t4 -> Bank: initializes P1 and P2 to handle your and your spouse's requests.
t5 -> P1 and P2 checked the balance and saw 100 dollars
t6 -> P1 and P2 subtracted 10 dollars from the balance
t7 -> P1 updated the DB with the new balance of 90
t8 -> P2 updated the DB with the new balance of 90
```

t1 - t8 is timeline of events. P1 and P2 is a process that handle requests from Bank ATM machines.

在上面的例子中，操作没有按顺序发生。P2 银行没有等待 P1 完成它的任务。如果银行在读取最新余额之前等待 P1 完成读取余额、计算新余额并将更新后的余额写回数据库，它就不会损失 10 美元。

这个问题的解决方案是**事务**。你可以认为它们有点类似于 Java 中的[锁](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/util/concurrent/locks/package-summary.html)、信号量和同步块。在 Java 中，它保证只有锁持有者执行受锁保护的代码。

## 如何设置助手功能

现在让我们进入编码部分。我假设您已经有了一个 MongoClient 设置。您将需要 [Java Mongo 驱动程序 3.8 或更高版本](https://mongodb.github.io/mongo-java-driver/4.0/whats-new/#what-s-new-in-3-8)。

```
final static MongoClient client; // assumed you initialized this somewhere

public static ClientSession getNewClientSession() {
    return client.startSession();
}

public static TransactionOptions getTransactionOptions() {
    return TransactionOptions.builder()
        .readPreference(ReadPreference.primary())
        .readConcern(ReadConcern.LOCAL)
        .writeConcern(WriteConcern.MAJORITY)
        .build();
} 
```

Some general function needed for example below

`getNewClientSession`简单地返回一个事务的会话。`ClientSession`是特定交易的标识符。这是一条重要的数据，您可以将它传递给所有后续的 Mongo 操作，以便它可以隔离这些操作。

`getTransactionOptions`为交易提供选项。`ReadPreference.primary()`在我们读取数据时，为我们提供关于群集的最新信息。`WriteConcern.MAJORITY`导致数据库在成功写入大多数服务器后确认提交。

我们不应该到处创建客户端会话和事务选项，而是应该在单个方法上进行，只传递需要原子性的函数给它。

```
static <R> R withTransaction(final Function<ClientSession, R> executeFn) {
	final ClientSession clientSession = getNewClientSession();
	TransactionOptions txnOptions = this.getTransactionOptions();

	TransactionBody<R> txnBody = new TransactionBody<R>() {
		public R execute() {
			return executeFn.apply(clientSession);
		}
	};

	try {
		return clientSession.withTransaction(txnBody, txnOptions);
	} catch (RuntimeException e) {
		e.printStackTrace();
	} finally {
		clientSession.close();
	}
	return null;
}
```

Generic function to execute Function with in a transaction.

上面的函数在传入的函数中运行操作，参数是`executeFn`,作为原子操作或事务。让我们使用事务来实现提款功能。

注意，我返回的是`null`。您可以抛出一个新的异常，让调用者知道事务已经失败。在这个例子中，返回 null 意味着事务失败。

## Java 中的银行帐户示例

```
public class Account {
	@BsonId
    ObjectId _id;
	int balance;

    ... getters and setters
}

public class AccountService {
	public Collection<Account> getAccounts() {
    	return dbClient.getCollection('account', Account.class);
    }

    private Account currentBalance(ClientSession session, Bson accountId) {
    	return getAccounts().findOne(session, Filters.eq('_id', accountId)).first();
    }

	private int currentBalance(ClientSession session, Bson accountId) {
    	Account account = getAccounts().findOne(session, Filters.eq('_id', accountId)).first();
        return account.balance;
    }

    private int updateBalance(ClientSession session, Bson accountId, int newBalance) {
    	Account account = getAccounts().updateOne(session, Filters.eq('_id', accountId), Updates.set('balance', newBalance)).first();
        return account.balance;
    }

    public Account drawCash(ClientSession session, Bson accountId, int amount){
    	int currentBalance = this.currentBalance(accountId);
        int newBalance = currentBalance - amount;
        return updateBalance(session, accountId, amount);
    }
}
```

Note: edge cases such as checking if balance is greater than the withdrawal amount is not checked for simplicity.

在上面的代码片段中，`Account`类是用户帐户的普通 Java 类模型。`AccountService`是 accounts 集合的数据库访问器。`drawCach`方法完成了第一个例子中描述的由单个进程(P1 或 P2)执行的一组操作，将钱分发给你或你的配偶。

现在我们使用这个`withTransaction`函数来调用`drawCache`:

```
... Some REST API 
AccountService accountService = ...; // Dependency injected

@Path('/account/withdraw') // Endpoint to withdraw money
withdrawMoney() {
	ObjectId accountId = ...// some method to get current users account ID
    Account account = withTransaction(new Function<ClientSession, Account>() {
        @Override
        public Workflow apply(ClientSession clientSession) {
        	// Everything inside this block run with in the same transaction as long as you pass the argument clientSession to mongo
            accountService.drawCash(clientSession, accountId, 10);
        }
    });

    if(Objects.isNull(account)){
        return "Failed to withdraw money";
    }
    return "New account balance is " + account.balance;
}
```

现在，如果您同时调用这个端点两次，一个用户将看到最终余额为 90，另一个用户将看到 80。

您可能已经猜到第二个用户的事务应该失败了。是的，确实如此。但是 MongoDB 有一个内置的重试机制，它自动重试我们的第二次操作并成功了。

## 一个真实的用例示例

我们在我们的[PS2PDF.com 在线视频转换器](https://www.ps2pdf.com/video-converter)上使用事务来防止一个线程覆盖另一个线程更新的进程状态。

例如，对于每个视频转换过程，我们在 DB 上创建一个名为 Job 的文档。它有一个状态字段，可以接受诸如`STARTED`、`IN_PROGRESS`和`COMPLETED`的值。

一旦线程将数据库上的 Job.status 更新到`COMPLETED`，我们不希望任何缓慢的线程将该消息恢复到`IN_PROGRESS`。作业一旦完成，就不能更改。

我们使用上面提到的`withTransaction`方法来保证没有操作覆盖`COMPLETE`状态。

## 结论

我希望您现在可以使用事务来避免应用程序中的竞争情况。另外，使用内置的`retryWrite`和`retryRead`来提高容错能力。

我应该指出，MongoDB 事务是相当新的事物，有文章指出了在特殊情况下出现的一些不一致。但是你不太可能会遇到这些问题。