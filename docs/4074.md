# 如何在 Swift 中进行 API 调用

> 原文：<https://www.freecodecamp.org/news/how-to-make-your-first-api-call-in-swift/>

如果你想成为一名 iOS 开发人员，有一些基本技能值得了解。首先，熟悉创建表格视图很重要。其次，您应该知道如何用数据填充这些表视图。第三，如果您可以从 API 获取数据并在您的表视图中使用这些数据，那就太好了。

第三点是我们将在本文中讨论的内容。自从在 Swift 4 中引入了`Codable`之后，进行 API 调用就容易多了。以前大多数人使用 Alamofire 和 SwiftyJson 这样的 pod(你可以在这里阅读如何做到这一点)。现在，快捷方式是更好的开箱即用，所以没有理由下载一个豆荚。

让我们来看一些经常用来进行 API 调用的构件。我们将首先介绍这些概念，因为它们是理解如何进行 API 调用的重要部分。

*   完成处理程序
*   `URLSession`
*   `DispatchQueue`
*   保留周期

最后，我们会把它放在一起。我将使用开源的星球大战 API 来构建这个项目。你可以在 [GitHub](https://github.com/ailyntang/starwars/) 上看到我完整的项目代码。

免责声明:我是编程新手，基本上是自学的。抱歉，如果我曲解了一些概念。

## 完成处理程序

![pheobe](img/c74f7522dd80cd308fa6f207dd1ae73a.png)

Poor patient Pheobe

还记得《老友记》中菲比连着几天粘在电话上等着和客服通话的那一集吗？想象一下，如果在电话一开始，一个可爱的叫 Pip 的人说:“谢谢你打电话来。我不知道你需要等多久，但是当我们准备好了的时候我会给你回电话的。”这并不好笑，但是 Pip 主动提出成为 Pheobe 的完成处理者。

当您知道函数需要一段时间才能完成时，您可以在函数中使用完成处理程序。你不知道还有多久，你也不想暂停你的生活，等待它结束。所以当 Pip 准备好给你答案时，你让她拍拍你的肩膀。这样你就可以继续你的生活，跑跑腿，看看书，看看电视。当 Pip 用答案轻拍你的肩膀时，你可以用她的答案。

这就是 API 调用所发生的情况。您向服务器发送一个 URL 请求，向它请求一些数据。你希望服务器很快返回数据，但是你不知道需要多长时间。不是让用户耐心等待服务器给你数据，而是使用一个完成处理程序。这意味着你可以告诉你的应用程序去做其他事情，比如加载页面的其余部分。

你告诉完成处理器，一旦你的应用程序有了你想要的信息，就轻拍它的肩膀。您可以指定该信息是什么。这样，当你的应用程序被点击时，它可以从完成处理程序中获取信息并做一些事情。通常您要做的是重新加载表视图，以便数据显示给用户。

下面是一个完成处理程序的例子。第一个例子是设置 API 调用本身:

```
func fetchFilms(completionHandler: @escaping ([Film]) -> Void) {
  // Setup the variable lotsOfFilms
  var lotsOfFilms: [Film]

  // Call the API with some code

  // Using data from the API, assign a value to lotsOfFilms  

  // Give the completion handler the variable, lotsOfFilms
  completionHandler(lotsOfFilms)
}
```

A function that uses a completion handler

现在我们要调用函数`fetchFilms`。一些需要注意的事项:

*   调用函数时不需要引用`completionHandler`。你唯一一次引用`completionHandler`是在函数声明中。
*   完成处理程序会返回一些数据给我们使用。基于我们上面写的函数，我们知道期望类型为`[Film]`的数据。我们需要给数据命名，以便我们可以引用它。下面我使用的名字是`films`，但也可以是`randomData`，或者任何我喜欢的变量名。

代码看起来会像这样:

```
fetchFilms() { (films) in
  // Do something with the data the completion handler returns 
  print(films)
}
```

Implementing a function with a completion handler

## URLSession

就像一个团队的经理。经理自己什么都不做。她的工作是与团队成员分享工作，他们会完成工作。她的团队是`dataTasks`。每次你需要一些数据的时候，给老板写信，用`URLSession.shared.dataTask`。

你可以给`dataTask`不同类型的信息来帮助你实现目标。给`dataTask`信息叫初始化。我用网址缩写我的`dataTasks`。`dataTasks`也使用完成处理程序作为它们初始化的一部分。这里有一个例子:

```
let url = URL(string: "https://www.swapi.co/api/films")

let task = URLSession.shared.dataTask(with: url, completionHandler: { (data, response, error) in 
  // your code here
})

task.resume()
```

How to use URLSession to fetch some data

使用完成处理程序，它们总是返回相同类型的信息:`data`、`response`和`error`。你可以给这些数据类型取不同的名字，比如`(data, res, err)`或者`(someData, someResponse, someError)`。出于惯例的考虑，最好坚持一些显而易见的东西，而不是使用新的变量名。

先说`error`。如果`dataTask`返回一个`error`，你会想提前知道。这意味着您可以指导您的代码优雅地处理错误。这也意味着您不必费心去尝试读取数据并对其进行处理，因为返回数据时会出现错误。

下面我简单地通过在控制台上打印一个错误并退出函数来处理这个错误。如果您愿意，还有许多其他方法可以处理该错误。想想这些数据对你的应用有多重要。例如，如果您有一个银行应用程序，这个 API 调用向用户显示他们的余额，那么您可能希望通过向用户显示一个模态来处理错误，该模态说，“抱歉，我们现在遇到一个问题。请稍后再试。

```
if let error = error {
  print("Error accessing swapi.co: /(error)")
  return
}
```

Handle the error

接下来我们来看看反应。您可以将响应转换为一个`httpResponse`。这样，您可以查看状态代码，并根据代码做出一些决定。例如，如果状态代码是 404，那么您知道没有找到该页面。

下面的代码使用一个`guard`来检查两件事情是否存在。如果两者都存在，那么它允许代码继续到`guard`子句后的下一条语句。如果任何一个语句失败，我们就退出这个函数。这是一个典型的`guard`条款的用例。您希望 guard 子句后的代码是快乐的流程(即没有错误的简单流程)。

```
 guard let httpResponse = response as? HTTPURLResponse,
            (200...299).contains(httpResponse.statusCode) else {
    print("Error with the response, unexpected status code: \(response)")
    return
  }
```

最后，您处理数据本身。请注意，我们没有为`error`或`response`使用完成处理程序。这是因为完成处理程序正在等待来自 API 的数据。如果它没有到达代码的数据部分，就没有必要调用处理程序。

对于数据，我们使用`JSONDecoder`以一种很好的方式解析数据。这是相当漂亮的，但要求你已经建立了一个模型。我们的型号叫做`FilmSummary`。如果`JSONDecoder`对你来说是新的，那么在网上看看如何使用它和如何使用`Codable`。在 Swift 4 及以上相对于 Swift 3 天真的很简单。

在下面的代码中，我们首先检查数据是否存在。我们非常确定它应该存在，因为没有错误，也没有奇怪的 HTTP 响应。第二，我们检查我们是否能够以我们期望的方式解析接收到的数据。如果可以，那么我们将电影摘要返回给完成处理程序。万一没有数据从 API 返回，我们有一个空数组的回退计划。

```
if let data = data,
        let filmSummary = try? JSONDecoder().decode(FilmSummary.self, from: data) {
        completionHandler(filmSummary.results ?? [])
      }
```

因此 API 调用的完整代码如下所示:

```
func fetchFilms(completionHandler: @escaping ([Film]) -> Void) {
    let url = URL(string: domainUrlString + "films/")!

    let task = URLSession.shared.dataTask(with: url, completionHandler: { (data, response, error) in
      if let error = error {
        print("Error with fetching films: \(error)")
        return
      }

      guard let httpResponse = response as? HTTPURLResponse,
            (200...299).contains(httpResponse.statusCode) else {
        print("Error with the response, unexpected status code: \(response)")
        return
      }

      if let data = data,
        let filmSummary = try? JSONDecoder().decode(FilmSummary.self, from: data) {
        completionHandler(filmSummary.results ?? [])
      }
    })
    task.resume()
  }
```

## 保留周期

*注意:我非常不了解保留周期！以下是我在网上调查的要点。*

对于内存管理来说，理解保留周期非常重要。基本上，你希望你的程序清理掉不再需要的内存。我认为这使得应用程序更有性能。

Swift 有许多方法可以自动帮助您做到这一点。然而，有许多方法可以意外地将保留周期编码到应用程序中。一个保留周期意味着你的应用程序会一直保留某段代码的内存。一般来说，当你有两个相互指向对方的东西时，就会发生这种情况。

为了解决这个问题，人们经常使用`weak`。当代码的一边是`weak`时，你没有保留周期，你的应用程序将能够释放内存。

出于我们的目的，一个常见的模式是在调用 API 时使用`[weak self]`。这确保了一旦完成处理程序返回一些代码，应用程序就可以释放内存。

```
fetchFilms { [weak self] (films) in
  // code in here
}
```

## 调度队列

Xcode 使用不同的线程并行执行代码。多线程的优势意味着你不会在继续做下一件事情之前等待一件事情完成。希望您可以在这里看到完成处理程序的链接。

这些线程似乎也被称为调度队列。API 调用在一个队列中处理，通常是在后台的一个队列中。一旦您从 API 调用中获得了数据，您很可能想要将这些数据显示给用户。这意味着您需要刷新表格视图。

表视图是 UI 的一部分，所有的 UI 操作都应该在主调度队列中完成。这意味着在你的视图控制器文件中的某个地方，通常作为`viewDidLoad`函数的一部分，你应该有一些代码告诉你的表视图刷新。

我们只希望表格视图在从 API 获得一些新数据后进行刷新。这意味着我们将使用一个完成处理程序来轻拍我们的肩膀，告诉我们 API 调用何时完成。我们将等到点击之后再刷新表格。

代码将类似于:

```
fetchFilms { [weak self] (films) in
  self.films = films

  // Reload the table view using the main dispatch queue
  DispatchQueue.main.async {
    tableView.reloadData()
  }
}
```

## viewDidLoad 与 viewDidAppear

最后，您需要决定在哪里调用您的`fetchfilms`函数。它将位于一个视图控制器中，该控制器将使用来自 API 的数据。有两个明显的地方可以进行这个 API 调用。一个在`viewDidLoad`里面，另一个在`viewDidAppear`里面。

你的应用程序有两种不同的状态。我的理解是`viewDidLoad`在你第一次在前台加载视图时被调用。`viewDidAppear`在您每次返回该视图时都会被调用，例如当您按下 back 按钮返回该视图时。

如果您希望您的数据在用户导航到该视图和从该视图返回的这段时间内发生变化，那么您可能希望将 API 调用放在`viewDidAppear`中。然而我认为对于几乎所有的应用程序来说，`viewDidLoad`就足够了。苹果建议所有的 API 调用都使用`viewDidAppear`，但这似乎有些过头了。我想这会降低你的应用程序的性能，因为它需要进行更多的 API 调用。

## 结合所有的步骤

首先:编写调用 API 的函数。以上，这是`fetchFilms`。这将有一个完成处理程序，它将返回您感兴趣的数据。在我的例子中，完成处理程序返回一个电影数组。

第二:在视图控制器中调用这个函数。您在这里这样做是因为您想要基于来自 API 的数据更新视图。在我的例子中，一旦 API 返回数据，我就刷新表视图。

第三:决定在视图控制器的什么地方调用这个函数。在我的例子中，我在`viewDidLoad`中调用它。

第四:决定如何处理来自 API 的数据。在我的例子中，我正在刷新一个表视图。

内部`NetworkManager.swift`(如果你愿意，这个函数可以在你的视图控制器中定义，但是我使用的是 MVVM 模式)。

```
func fetchFilms(completionHandler: @escaping ([Film]) -> Void) {
    let url = URL(string: domainUrlString + "films/")!

    let task = URLSession.shared.dataTask(with: url, completionHandler: { (data, response, error) in
      if let error = error {
        print("Error with fetching films: \(error)")
        return
      }

      guard let httpResponse = response as? HTTPURLResponse,
            (200...299).contains(httpResponse.statusCode) else {
        print("Error with the response, unexpected status code: \(response)")
        return
      }

      if let data = data,
        let filmSummary = try? JSONDecoder().decode(FilmSummary.self, from: data) {
        completionHandler(filmSummary.results ?? [])
      }
    })
    task.resume()
  }
```

内部`FilmsViewController.swift`:

```
final class FilmsViewController: UIViewController {
  private var films: [Film]?

  override func viewDidLoad() {
    super.viewDidLoad()

    NetworkManager().fetchFilms { [weak self] (films) in
      self?.films = films
      DispatchQueue.main.async {
        self?.tableView.reloadData()
      }
    }
  }

  // other code for the view controller
}
```

天哪，我们成功了！谢谢你坚持和我在一起。