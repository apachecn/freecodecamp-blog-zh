# 如何使用 Swift 和 Laravel 创建带有推送通知的 iOS 加密跟踪应用程序

> 原文：<https://www.freecodecamp.org/news/create-a-cryptocurrency-tracking-app-with-push-notifications-using-swift-and-laravel-part-2-the-6275674a12f/>

作者:尼奥·伊戈达罗

# 如何使用 Swift 和 Laravel 创建带有推送通知的 iOS 加密跟踪应用程序

#### 第二部分

![IGYplFvYpzyybbPqcGLune2bODwCxmj9bc8v](img/84d05b553f0531de1f6f5d804036efbf.png)

Photo by [André François McKenzie](https://unsplash.com/photos/iGYiBhdNTpE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/bitcoin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 您需要在您的机器上安装以下软件:Xcode、Laravel CLI、SQLite 和 Cocoapods。熟悉 Xcode IDE 会有所帮助。你应该已经完成了这个系列的第一部分。

在本文的第一部分中，我们开始开发我们的加密货币警报应用程序。我们开发了支持 iOS 应用的应用后端。就目前情况而言，我们的后端应用程序可以根据设备的 UUID 返回设备的设置，根据设备的 UUID 保存设备的设置，并计算出当货币更新时向哪些设备发送推送通知。

在这一部分，我们将重点介绍使用 Swift 和 Xcode 创建 iOS 应用程序。

### 先决条件

要跟进，您需要满足以下要求:

*   已完成本文的第一部分。
*   安装在您机器上的 Xcode 。
*   Xcode IDE 的知识。
*   使用 [Laravel 框架](https://laravel.com/)的基础知识。
*   关于 [Swift 编程语言](http://developer.apple.com/swift)的基础知识。
*   安装在您机器上的 Laravel CLI 。
*   安装在您机器上的 SQLite。[安装指南](http://www.sqlitetutorial.net/download-install-sqlite/)。
*   安装在您机器上的 Cocoapods 。
*   [推梁](https://pusher.com/beams)和[通道](https://pusher.com/channels)应用。

### 我们将会建造什么

我们已经开始使用 Laravel 构建应用程序的后端。接下来，我们将使用 Swift 构建 iOS 应用程序。如果您想测试推送通知，那么您需要在一个活动设备上运行该应用程序。

### 客户端应用程序将如何工作

对于客户端应用程序，iOS 应用程序，我们将创建一个简单的列表，显示可用的货币和美元的当前价格。每当加密货币的价格发生变化时，我们将使用推送渠道触发一个事件，以便更新价格。

从应用程序中，您将能够设置一个最小和最大的价格变化时，你想得到提醒。例如，您可以配置应用程序，当一个以太网(ETH)的价格低于 500 美元时，向应用程序发送推送通知。你也可以配置应用程序在比特币价格超过 5000 美元时接收通知。

### 应用程序的外观

当我们完成应用程序时，下面是应用程序的外观:

![ynnaEkKLDhpnaZBrt7pd4aw5gxGjUYK89hNT](img/db529a60bead16e4579b1c801b561b88.png)

让我们开始吧。

### 设置客户端应用程序

启动 Xcode 并点击**创建新的 Xcode 项目**。选择**单视图 App** ，点击**下一步**。输入您的**产品名称**，我们将调用我们的项目 *cryptoalat* ，并从**语言**选项中选择 **Swift** 。您也可以在屏幕上更改您想要的任何其他细节，然后点击下一步的**。**

### 安装依赖项

现在您有了 Xcode 项目。关闭 Xcode 并打开一个终端窗口。`cd`到终端中的 iOS 项目目录，运行下面的命令创建一个 Podfile:

```
$ pod init
```

> Podfile 是描述一个或多个 Xcode 项目的目标的依赖关系的规范。该文件应该简单地命名为 Podfile。指南中的所有例子都是基于 CocoaPods 版本 1.0 和更高版本的。——[椰子导游](https://guides.cocoapods.org/using/the-podfile.html)

这将在项目的根目录下生成一个名为`Podfile`的新文件。在任何编辑器中打开此文件，并更新文件，如下所示:

```
// File: Podfile    platform :ios, '11.0'
```

```
 target 'cryptoalat' do      use_frameworks!
```

```
 pod 'Alamofire', '~> 4.7.2'      pod 'PushNotifications', '~> 0.10.8'      pod 'PusherSwift', '~> 6.1.0'      pod 'NotificationBannerSwift', '~> 1.6.3'    end
```

> *如果您使用了 cryptoalat 之外的项目名称，那么在 Podfile 中更改它以匹配您项目的目标名称。*

转到终端并运行下面的命令来安装您的依赖项:

```
$ pod install
```

当安装完成时，您将在项目的根目录中拥有一个`*.xcworkspace`文件。在 Xcode 中打开这个文件，让我们开始开发加密货币警报应用程序。

### 构建 iOS 应用程序

#### 创建我们的故事板

我们需要做的第一件事是为应用程序设计故事板。这是我们希望故事板完成后的样子。

![JZxbqFnKDBwtADJ8tJcYigBx9V0zq04dbu0N](img/cb10e8d977f60e5f2261f086501bdc9a.png)

打开`Main.storyboard`文件，如上图所示设计。

上面我们有三个场景。第一个场景，也是切入点，是发射场景。然后，我们用一个名为 **Main** 的标识符绘制一个手动序列。然后我们将序列**种类**设置为**呈现方式**。这将呈现下一个场景，即导航视图控制器。默认情况下，导航控制器已经有一个附加的根视图控制器。

我们将使用这个附加的视图控制器，它是一个`TableViewController`，作为应用程序的主视图。它会列出可用的货币，并向我们显示一个文本字段，允许我们在点击时更改该货币的设置。

在第三个场景中，我们将单元格的重用标识符设置为 **coin** ，并将两个标签拖到原型单元格中。第一个标签是硬币名称，第二个标签是价格。

现在我们有了场景，让我们创建一些控制器和视图类，并将它们连接到我们的故事板场景。

#### 创建控制器

在 Xcode 中，创建一个新的类`LaunchViewController`，并将下面文件的内容粘贴到其中:

```
import UIKit
```

```
 class LaunchViewController: UIViewController {
```

```
 override func viewDidAppear(_ animated: Bool) {            super.viewDidAppear(animated)
```

```
 SettingsService.shared.loadSettings {                self.routeToMainController()            }        }
```

```
 fileprivate func routeToMainController() {            performSegue(withIdentifier: "Main", sender: self)        }    }
```

> *将控制器设置为`Main.storyboard`文件中第一个场景的自定义类。*

在代码中，我们使用稍后将创建的`SettingsService`类加载设置。当为设备加载设置时，我们调用`routeToMainController`方法，该方法使用我们之前创建的 **Main** segue 将应用程序路由到主控制器。

我们将创建的下一个控制器是`CoinsTableViewController`。这将是与第三个场景(即主场景)关联的控制器。

创建`CoinsTableViewController`并用以下代码替换内容:

```
import UIKit    import PusherSwift    import NotificationBannerSwift
```

```
 struct Coin {        let name: String        let rate: Float    }
```

```
 class CoinsTableViewController: UITableViewController {
```

```
 var coins: [Coin] = []
```

```
 override func viewDidLoad() {            super.viewDidLoad()        }
```

```
 override func numberOfSections(in tableView: UITableView) -> Int {            return 1        }
```

```
 override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {            return coins.count        }
```

```
 override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {            let coin = coins[indexPath.row]            let cell = tableView.dequeueReusableCell(withIdentifier: "coin", for: indexPath) as! CoinTableViewCell
```

```
 cell.name.text = "1 \(coin.name) ="            cell.amount.text = "$\(String(coin.rate))"
```

```
 return cell        }    }
```

> *将控制器设置为`Main.storyboard`文件中第一个场景的自定义类。*

上面我们已经定义了`Coin`结构，它有一个`name`和`rate`属性。我们有一个控制器，我们将`coins`属性定义为一个`Coin`的数组。然后我们有一些创建表格视图控制器的样板代码。

`numberOfSections`方法指定表格将有多少个部分。在第一个`tableView`方法中，我们返回可用的`coins`的数量，在第二个`tableView`方法中，我们定义我们希望如何处理每一行。

#### 创建其他支持类

如果你在上面的代码中注意到了，我们在最后一个`tableView`方法中引用了一个`CoinTableViewCell`作为每一行的类。让我们来创造它。

创建一个`CoinTableViewCell`类，并将以下代码粘贴到其中:

```
class CoinTableViewCell: UITableViewCell {        @IBOutlet weak var name: UILabel!            @IBOutlet weak var amount: UILabel!    }
```

打开`Main.storyboard`文件，将该类设置为`Main.storyboard`文件第三个场景中原型单元的自定义类。设置好等级后，按照上面单元等级中的规定连接`@IBOutlet`。

我们需要创建的下一个类是`SettingsService`。这个类将负责更新和获取设备的设置。

创建一个新的`SettingsService`类，并用以下代码替换其内容:

```
import Foundation    import Alamofire    import NotificationBannerSwift
```

```
 class SettingsService {        static let key = "CryptoAlat"        static let shared = SettingsService()
```

```
 var settings: Settings? {            get {                return self.getCachedSettings()            }            set(settings) {                if let settings = settings {                    self.updateCachedSettings(settings)                }            }        }
```

```
 private init() {}
```

```
 func loadSettings(completion: @escaping() -> Void) {            fetchRemoteSettings { settings in                guard let settings = settings else {                    return self.saveSettings(self.defaultSettings()) { _ in                        completion()                    }                }
```

```
 self.updateCachedSettings(settings)                completion()            }        }
```

```
 fileprivate func defaultSettings() -> Settings {            return Settings(                btc_min_notify: 0,                 btc_max_notify: 0,                 eth_min_notify: 0,                 eth_max_notify: 0            )        }
```

```
 func saveSettings(_ settings: Settings, completion: @escaping(Bool) -> Void) {            updateRemoteSettings(settings, completion: { saved in                if saved {                    self.updateCachedSettings(settings)                }
```

```
 completion(saved)            })        }
```

```
 fileprivate func fetchRemoteSettings(completion: @escaping (Settings?) -> Void) {            guard let deviceID = AppConstants.deviceIDFormatted else {                return completion(nil)            }
```

```
 let url = "\(AppConstants.API_URL)?u=\(deviceID)"            Alamofire.request(url).validate().responseJSON { resp in                if let data = resp.data, resp.result.isSuccess {                    let decoder = JSONDecoder()                    if let settings = try? decoder.decode(Settings.self, from: data) {                        return completion(settings)                    }                }
```

```
 completion(nil)            }        }
```

```
 fileprivate func updateRemoteSettings(_ settings: Settings, completion: @escaping(Bool) -> Void) {            guard let deviceID = AppConstants.deviceIDFormatted else {                return completion(false)            }
```

```
 let params = settings.toParams()            let url = "\(AppConstants.API_URL)?u=\(deviceID)"            Alamofire.request(url, method: .post, parameters: params).validate().responseJSON { resp in                guard resp.result.isSuccess, let res = resp.result.value as? [String: String] else {                    return StatusBarNotificationBanner(title: "Failed to update settings.", style: .danger).show()                }
```

```
 completion((res["status"] == "success"))            }        }
```

```
 fileprivate func updateCachedSettings(_ settings: Settings) {            if let encodedSettings = try? JSONEncoder().encode(settings) {                UserDefaults.standard.set(encodedSettings, forKey: SettingsService.key)            }        }
```

```
 fileprivate func getCachedSettings() -> Settings? {            let defaults = UserDefaults.standard            if let data = defaults.object(forKey: SettingsService.key) as? Data {                let decoder = JSONDecoder()                if let decodedSettings = try? decoder.decode(Settings.self, from: data) {                    return decodedSettings                }            }
```

```
 return nil        }    }
```

上面我们有`SettingsService`。第一种方法`loadSettings`从 API 加载设置，然后保存在本地。如果没有远程设置，它调用`defaultSettings`方法并将响应保存到 API。

`saveSettings`方法使用`updateRemoteSettings`远程保存`Settings`，然后在本地使用`updateCachedSettings`。`fetchRemoteSettings`从 API 获取设置，并使用 [Swift decodable API](https://blog.pusher.com/swift-4-decoding-json-codable/) 对响应进行解码。

接下来，让我们定义`Settings`结构并让它扩展`Codable`。在`SettingsService`的同一个文件中，在`SettingsService`类定义上方添加以下内容:

```
struct Settings: Codable {        var btc_min_notify: Int?        var btc_max_notify: Int?        var eth_min_notify: Int?        var eth_max_notify: Int?
```

```
 func toParams() -> Parameters {            var params: Parameters = [:]
```

```
 if let btcMin = btc_min_notify { params["btc_min_notify"] = btcMin }            if let btcMax = btc_max_notify { params["btc_max_notify"] = btcMax }            if let ethMin = eth_min_notify { params["eth_min_notify"] = ethMin }            if let ethMax = eth_max_notify { params["eth_max_notify"] = ethMax }
```

```
 return params        }    }
```

上面我们有一个简单的符合`Codable`的`Settings`结构。我们还有一个`toParams`方法，将属性转换为`Parameters`类型，这样我们就可以在发出请求时与 [Alamofire](https://github.com/Alamofire/Alamofire) 一起使用。

我们需要创建的最后一个类是`AppConstants`。我们将使用这个类来保存我们希望在应用程序的整个生命周期中保持不变的所有数据。

创建一个`AppConstants`文件并粘贴以下代码:

```
import UIKit
```

```
 struct AppConstants {        static let API_URL = "http://127.0.0.1:8000/api/settings"        static let deviceID = UIDevice.current.identifierForVendor?.uuidString        static let deviceIDFormatted = AppConstants.deviceID?.replacingOccurrences(of: "-", with: "_").lowercased()        static let PUSHER_INSTANCE_ID = "PUSHER_BEAMS_INSTANCE_ID"        static let PUSHER_APP_KEY = "PUSHER_APP_KEY"        static let PUSHER_APP_CLUSTER = "PUSHER_APP_CLUSTER"    }
```

> *将`PUSHER_*`键替换为从推杆通道和横梁仪表板获得的值。*

#### 更新设备的设置

现在我们已经定义了设置服务，让我们更新我们的控制器，以便用户可以设置每种货币的最低和最高价格。

打开`CoinsTableViewController`类并添加以下方法:

```
override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {        let coin = coins[indexPath.row]
```

```
 var minTextField: UITextField?        var maxTextField: UITextField?
```

```
 let title = "Manage \(coin.name) alerts"        let message = "Notification will be sent to you when price exceeds or goes below minimum and maximum price. Set to zero to turn off notification."
```

```
 let alert = UIAlertController(title: title, message: message, preferredStyle: .alert)
```

```
 alert.addTextField { textfield in            minTextField = textfield            textfield.placeholder = "Alert when price is below"        }
```

```
 alert.addTextField { textfield in            maxTextField = textfield            textfield.placeholder = "Alert when price is above"        }
```

```
 alert.addAction(UIAlertAction(title: "Cancel", style: .cancel, handler: nil))
```

```
 alert.addAction(UIAlertAction(title: "Save", style: .default, handler: { action in            guard let minPrice = minTextField?.text, let maxPrice = maxTextField?.text else {                return StatusBarNotificationBanner(title: "Invalid min or max price", style: .danger).show()            }
```

```
 var btcMin: Int?, btcMax: Int?, ethMin: Int?, ethMax: Int?
```

```
 switch coin.name {            case "BTC":                btcMin = Int(minPrice)                btcMax = Int(maxPrice)            case "ETH":                ethMin = Int(minPrice)                ethMax = Int(maxPrice)            default:                return            }
```

```
 let settings = Settings(                btc_min_notify: btcMin,                btc_max_notify: btcMax,                eth_min_notify: ethMin,                eth_max_notify: ethMax            )
```

```
 SettingsService.shared.saveSettings(settings, completion: { saved in                if saved {                    StatusBarNotificationBanner(title: "Saved successfully").show()                }            })        }))
```

```
 present(alert, animated: true, completion: nil)    }
```

当选择一行时，上面的方法被自动调用。在这个方法中，我们显示一个带有两个文本字段的`UIAlertController`,分别表示最低价格和最高价格。提交价格时，我们之前创建的`SettingsService`负责本地和远程更新价格。

#### 添加实时加密货币更新支持

打开`CoinsTableViewController`并将`pusher`属性添加到类中，如下所示:

```
var pusher: Pusher!
```

然后用下面的代码替换`viewDidLoad`方法:

```
override func viewDidLoad() {        super.viewDidLoad()
```

```
 pusher = Pusher(            key: AppConstants.PUSHER_APP_KEY,             options: PusherClientOptions(host: .cluster(AppConstants.PUSHER_APP_CLUSTER))        )
```

```
 let channel = pusher.subscribe("currency-update")
```

```
 let _ = channel.bind(eventName: "currency.updated") { data in            if let data = data as? [String: [String: [String: Float]]] {                guard let payload = data["payload"] else { return }
```

```
 self.coins = []
```

```
 for (coin, deets) in payload {                    guard let currentPrice = deets["current"] else { return }                    self.coins.append(Coin(name: coin, rate: currentPrice))                }
```

```
 Dispatch.main.async {                    self.tableView.reloadData()                }            }        }
```

```
 pusher.connect()    }
```

在上面的代码中，我们使用 [Pusher Swift SDK](https://pusher.com/docs/ios_quick_start) 来订阅我们的`currency-update` Pusher 频道。然后，我们订阅该频道上的`currency.updated`事件。每当该事件被触发时，我们都会实时刷新加密货币的价格。

### 向我们的 iOS 新应用程序添加推送通知

要添加推送通知支持，请打开`AppDelegate`类并用以下内容替换其内容:

```
import UIKit    import PushNotifications
```

```
 @UIApplicationMain    class AppDelegate: UIResponder, UIApplicationDelegate {
```

```
 var window: UIWindow?
```

```
 func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {            PushNotifications.shared.start(instanceId: AppConstants.PUSHER_INSTANCE_ID)            PushNotifications.shared.registerForRemoteNotifications()            return true        }
```

```
 func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {            PushNotifications.shared.registerDeviceToken(deviceToken) {                if let deviceID = AppConstants.deviceIDFormatted {                    try? PushNotifications.shared.subscribe(interest: "\(deviceID)_eth_changed")                    try? PushNotifications.shared.subscribe(interest: "\(deviceID)_btc_changed")                }            }        }    }
```

在上面的类中，我们使用 [Pusher Beams Swift SDK](https://docs.pusher.com/push-notifications/reference/ios) 为推送通知注册设备。然后我们订阅`*_eth_changed`和`*_btc_changed`兴趣，其中`*`是设备的唯一 UUID。

现在我们已经完成了应用程序的逻辑，让我们在 Xcode 中启用应用程序上的推送通知。

在项目导航器中，选择您的项目，并点击 **Capabilities** 选项卡。[打开开关，启用推送通知](http://help.apple.com/xcode/mac/current/#/devdfd3d04a1)。

![4Kd00J1IvFFj633HSm-WbwTxm5pHFxhPRY82](img/5d1261c92abb6f198fcba0ab4ecfdf63.png)

这将在项目的根目录下创建一个授权文件。至此，您已经为您的应用程序提供了完全接收推送通知的能力。

### 允许我们的应用程序本地连接

如果你打算使用本地服务器测试应用程序的后端，那么我们还需要做最后一件事。打开`info.plist`文件并向`plist`文件添加一个条目，以允许连接到我们的本地服务器:

![973kwkgkzdbbxkJQzMEePM3vR2yCwC2J-FRn](img/74201719396d6b05ef6a97fb0f0ea854.png)

仅此而已。我们可以运行我们的应用程序。然而，**请记住，要演示推送通知，您需要一个实际的 iOS 设备，因为模拟器无法接收推送通知。**如果你使用的是物理设备，你需要使用 [Ngrok](https://ngrok.com) 公开你的本地 API，然后在 `AppConstants`中修改`API_URL` **。**

任何时候您想要更新货币价格，在您的 Laravel 应用程序中手动运行下面的命令:

```
$ php artisan schedule:run
```

下面是应用程序运行的屏幕记录:

![OBlHx04eWtcmomYWEAXfZlr3N2kM9ZC90Wym](img/e1ab1341a922f1a04920dfac0c2f3fce.png)

### 结论

在本文中，我们已经能够看到使用 Laravel、Swift、Pusher Channels 和 Pusher Beams 创建加密货币警报网站是多么容易。本文中构建的应用程序的源代码可以在 [GitHub](https://github.com/neoighodaro/cryptocurrency-alert-ios-app) 上获得。

本文首发于[推手](https://pusher.com/tutorials/cryptocurrency-tracking-swift-laravel-part-2)。