# 如何用 Swift 构建实时地图

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-realtime-map-with-swift-67fb0e977e48/>

作者:尼奥·伊戈达罗

# 如何用 Swift 构建实时地图

![IxlZRMwTCK2070izDrv3rY52ZZjzcLrGtxPp](img/37b840df366459111c0249fae035840f.png)

Photo by [NASA](https://unsplash.com/photos/_SFJhRPzJHs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/map?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

实时地图现在非常流行。尤其是现在有许多按需运输服务，如优步和 Lyft，都有实时位置报告。在本文中，我们将学习如何使用 Pusher 在 iOS 上构建实时地图。

在我们继续之前，您需要确保满足以下所有要求:

*   一台 MacBook (Xcode 只能在 Mac 上运行)。
*   安装在您机器上的 Xcode 。
*   JavaScript 知识(Node.js)。
*   Swift 和 Xcode 使用知识。这里可以开始[。](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/)
*   本地安装的 [NPM 和 Node.js](https://docs.npmjs.com/getting-started/installing-node) 。
*   本地安装的包管理器。
*   一个谷歌 iOS API 密钥。参见[此处](https://developers.google.com/maps/documentation/ios-sdk/start#step_4_get_an_api_key)获取如何获取密钥的说明。
*   推送应用程序。在这里创建一个。

遵循本教程需要对 Swift 和 Node.js 有基本的了解。

假设你有所有的要求，让我们开始吧。这是我们将要构建的内容的屏幕记录:

![oNEt5czqqZUmTL5r6SvWAjv2QBSnWgYEr8RT](img/e018cba3f1d506f6e128362b557fe4d2.png)

正如您在演示中看到的，每次位置更新时，这种变化都会反映在两台设备上。这就是我们想要复制的。让我们开始吧。

### 设置我们的 iOS 应用

启动 Xcode 并创建一个新的“单应用程序”项目。你可以随意称呼这个项目。

![SykobSVDuoF5VSM3PZeNvC4sISPAbkoYUwIb](img/0ba28a88af34773b6c1ddc6032ca9e78.png)

创建项目后，关闭 Xcode。打开您的终端，`cd`到您的应用程序的根目录，运行下面的命令来初始化项目上的 Cocoapods:

```
$ pod init
```

上面的命令将在我们的应用程序的根目录中创建一个`Podfile`。在这个`Podfile`中，我们将指定我们的项目依赖关系，并让 Cocoapods 拉取和管理它们。打开`Podfile`，将文件内容替换为以下内容:

```
platform :ios, '10.0'target 'application_name' do    use_frameworks!
```

```
 pod 'GoogleMaps'    pod 'Alamofire', '~> 4.4.0'    pod 'PusherSwift', '~> 4.1.0'end
```

> *⚠️确保用你的应用程序的名字替换`application_name`。*

运行下面的命令开始安装我们在`Podfile`中指定的包:

```
$ pod install
```

安装完成后，打开添加到应用程序目录根目录的`*.xcworkspace`文件。这应该会启动 Xcode。

### 设置我们的 Node.js 模拟器应用程序

在回到我们的 iOS 应用程序之前，我们需要创建一个简单的 Node.js 应用程序。该应用程序将向 Pusher 发送带有数据的事件。发送到推进器的数据将是模拟的 GPS 坐标。当我们的 iOS 应用程序从 Pusher 获取事件数据时，它会将地图标记更新到新的坐标。

创建一个新目录来保存我们的 Node.js 应用程序。打开终端并`cd`到 Node.js 应用程序的目录。在这个目录中，创建一个新的`package.json`文件。打开该文件并粘贴以下 JSON:

```
{      "main": "index.js",      "dependencies": {        "body-parser": "^1.16.0",        "express": "^4.14.1",        "pusher": "^1.5.1"      }    }
```

现在运行下面的命令来安装作为依赖项列出的 NPM 软件包:

```
$ npm run install
```

在目录中创建新的`index.js`文件，并将下面的代码粘贴到该文件中:

```
//    // Load the required libraries    //    let Pusher     = require('pusher');    let express    = require('express');    let bodyParser = require('body-parser');    //    // initialize express and pusher    //    let app        = express();    let pusher     = new Pusher(require('./config.js'));    //    // Middlewares    //    app.use(bodyParser.json());    app.use(bodyParser.urlencoded({ extended: false }));
```

```
//    // Generates 20 simulated GPS coords and sends to Pusher    //    app.post('/simulate', (req, res, next) => {      let loopCount = 0;      let operator = 0.001000        let longitude = parseFloat(req.body.longitude)      let latitude  = parseFloat(req.body.latitude)      let sendToPusher = setInterval(() => {        loopCount++;        // Calculate new coordinates and round to 6 decimal places...        longitude = parseFloat((longitude + operator).toFixed(7))        latitude  = parseFloat((latitude - operator).toFixed(7))        // Send to pusher        pusher.trigger('mapCoordinates', 'update', {longitude, latitude})        if (loopCount === 20) {          clearInterval(sendToPusher)        }      }, 2000);      res.json({success: 200})    })
```

```
//    // Index    //    app.get('/', (req, res) => {      res.json("It works!");    });
```

```
//    // Error Handling    //    app.use((req, res, next) => {        let err = new Error('Not Found');        err.status = 404;        next(err);    });
```

```
//    // Serve app    //    app.listen(4000, function() {        console.log('App listening on port 4000!')});
```

上面的代码是一个简单的 Express 应用程序。我们已经初始化了 Express `app`和`pusher`实例。在`/simulate`路线中，我们以 2 秒的间隔跑一圈，并在跑完第 20 圈后停止。每次循环运行时，都会生成新的 GPS 坐标并发送给 Pusher。

创建一个新的`config.js`文件，并将下面的代码粘贴到其中:

```
 module.exports = {        appId: 'PUSHER_APP_ID',        key: 'PUSHER_APP_KEY',        secret: 'PUSHER_APP_SECRET',        cluster: 'PUSHER_APP_CLUSTER',    };
```

将`*PUSHER_APP_ID*`、`*PUSHER_APP_KEY*`、`PUSHER_APP_SECRET`和`PUSHER_APP_CLUSTER`的值替换为推动器应用仪表板中的值。我们的 Node.js 应用程序现在可以在应用程序触发 GPS 坐标时模拟它了。

既然我们已经完成了 Node.js 应用程序的创建，我们可以继续创建 iOS 应用程序了。

### 在 Xcode 中创建我们的实时地图视图

用我们的项目重新打开 Xcode 并打开`Main.storyboard`文件。在`ViewController`中我们将添加一个`UIView`，在`UIView`中我们将添加一个模拟按钮。大概是这样的:

![rOVmC-h3c2R2xCeeuPcTlp8g7fG5Yr-tWJl6](img/319c66d4ba6e5707f3af37f9be17a3e3.png)

从按钮到`ViewController`创建一个`@IBAction`。为此，点击 Xcode 工具集右上角的“显示助理编辑器”。这将把屏幕分成故事板和代码编辑器。现在将`ctrl`从按钮中拖到代码编辑器中来创建`@IBAction`。我们将称这个方法为`simulateMovement`。

![sirx-Tu8WaaGQUQVz4IKvuTdFlhZELT2qM0F](img/a2603ee6dc5bcac6d05bedb0a4b49acc.png)

接下来，点击 Xcode 工具栏上的“显示标准编辑器”按钮，关闭分屏，只显示`Main.storyboard`。从最后一个`UIView`的底部开始添加另一个`UIView`到屏幕底部。该视图将显示地图。

在“身份检查器”中将`UIView`的自定义类设置为`GMSMapView`。现在点击 Xcode 工具集右上角的“显示助理编辑器”。`ctrl`并从`UIView`中拖拽到代码编辑器中。创建一个`@IBOutlet`，命名为`mapView`。

点击 Xcode 工具栏上的“显示标准编辑器”按钮，关闭分割视图。打开`ViewController`文件，用下面的代码替换内容:

```
//    // Import libraries    //    import UIKit    import PusherSwift    import Alamofire    import GoogleMaps
```

```
 //    // View controller class    //    class ViewController: UIViewController, GMSMapViewDelegate {        // Marker on the map        var locationMarker: GMSMarker!
```

```
 // Default starting coordinates        var longitude = -122.088426        var latitude  = 37.388064
```

```
 // Pusher        var pusher: Pusher!
```

```
 // Map view        @IBOutlet weak var mapView: GMSMapView!
```

```
 //        // Fires automatically when the view is loaded        //        override func viewDidLoad() {            super.viewDidLoad()
```

```
 //            // Create a GMSCameraPosition that tells the map to display the coordinate            // at zoom level 15\.            //            let camera = GMSCameraPosition.camera(withLatitude:latitude, longitude:longitude, zoom:15.0)            mapView.camera = camera            mapView.delegate = self
```

```
 //            // Creates a marker in the center of the map.            //            locationMarker = GMSMarker(position: CLLocationCoordinate2D(latitude: latitude, longitude: longitude))            locationMarker.map = mapView
```

```
 //            // Connect to pusher and listen for events            //            listenForCoordUpdates()        }
```

```
 //        // Send a request to the API to simulate GPS coords        //        @IBAction func simulateMovement(_ sender: Any) {            let parameters: Parameters = ["longitude":longitude, "latitude": latitude]
```

```
 Alamofire.request("http://localhost:4000/simulate", method: .post, parameters: parameters).validate().responseJSON { (response) in                switch response.result {                case .success(_):                    print("Simulating...")                case .failure(let error):                    print(error)                }            }        }
```

```
 //        // Connect to pusher and listen for events        //        private func listenForCoordUpdates() {            // Instantiate Pusher            pusher = Pusher(key: "PUSHER_APP_KEY", options: PusherClientOptions(host: .cluster("PUSHER_APP_CLUSTER")))
```

```
 // Subscribe to a Pusher channel            let channel = pusher.subscribe("mapCoordinates")
```

```
 //            // Listener and callback for the "update" event on the "mapCoordinates"            // channel on Pusher            //            channel.bind(eventName: "update", callback: { (data: Any?) -> Void in                if let data = data as? [String: AnyObject] {                    self.longitude = data["longitude"] as! Double                    self.latitude  = data["latitude"] as! Double
```

```
 //                    // Update marker position using data from Pusher                    //                    self.locationMarker.position = CLLocationCoordinate2D(latitude: self.latitude, longitude: self.longitude)                    self.mapView.camera = GMSCameraPosition.camera(withTarget: self.locationMarker.position, zoom: 15.0)                }            })
```

```
 // Connect to pusher            pusher.connect()        }    }
```

在上面的控制器类中，我们导入了所有需要的库。然后我们实例化这个类的一些属性。在`viewDidLoad`方法中，我们在`mapView`上设置了坐标，还向其添加了`locationMarker`。

用同样的方法，我们调用`listenForCoordUpdates()`。在`listenForCoordUpdates`方法中，我们创建一个到 Pusher 的连接，并监听`mapCoordinates`通道上的`update`事件。

当`update`事件被触发时，回调获取新的坐标并用它们更新`locationMarker`。请记住，您需要将`PUSHER_APP_KEY`和`PUSHER_APP_CLUSTER`更改为您的推动器应用提供的实际值。

在`simulateMovement`方法中，我们只是向本地 web 服务器(我们之前创建的 Node.js 应用程序)发送一个请求。该请求将指示 Node.js 应用程序生成几个 GPS 坐标。

> *？我们所攻击的端点的 URL(h[TTP://localhost:3000/simulate)](http://localhost:3000/simulate)是一个本地 web 服务器。这意味着在构建真实案例时，您将需要更改端点 URL。*

### 为 iOS 配置谷歌地图

我们需要配置 Google Maps iOS SDK 来使用我们的应用程序。首先，[创建一个 Google iOS SDK 密钥](https://developers.google.com/maps/documentation/ios-sdk/start#step_4_get_an_api_key)，然后，当您有了 API 密钥后，在 Xcode 中打开`AppDelegate.swift`文件。

在类别中，查找以下类别:

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {        // Override point for customization after application     launch.        return true    }
```

用这个替换它:

```
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {        GMSServices.provideAPIKey("GOOGLE_IOS_API_KEY")        return true }
```

> *？你需要用你创建 Google iOS API 密匙时得到的密匙替换 G `OOGLE_IOS_API_KEY` 。*

在同一文件的顶部，查找`import UIKit`,并在它下面添加以下内容:

```
import GoogleMaps
```

至此，我们完成了在 iOS 上配置谷歌地图的工作。

### 测试我们的实时 iOS 地图

为了测试我们的应用程序，我们需要启动 Node.js 应用程序，指示 iOS 允许连接到本地 web 服务器，然后运行我们的 iOS 应用程序。

要运行 Node.js 应用程序，使用您的终端将`cd`定位到 Node.js 应用程序目录，并运行以下命令来启动 Node 应用程序:

```
$ node index.js
```

现在，在我们启动应用程序之前，我们需要做一些最后的更改，这样我们的 iOS 应用程序就可以连接到我们的`localhost`后端。在 Xcode 中打开`info.plist`文件，并进行以下调整:

![k0V9gHDnWihDnOigSrwEOias6xFVD0Di-5Td](img/0b67a9dccab15e7d38dbfcb9b7605f63.png)

这一更改将使我们的应用程序能够连接到本地主机。明确地说，在生产环境中不需要这一步。

现在构建您的应用程序。您应该看到 iOS 应用程序现在显示了地图和地图上的标记。点击模拟按钮点击端点，然后将新坐标发送给 Pusher。我们的侦听器捕捉事件并更新`locationMarker`，从而移动我们的标记。

### 结论

在本文中，我们看到了如何使用 Pusher 和 Swift 在 iOS 上构建实时地图。我希望你学到了一些如何创建实时 iOS 应用程序的东西。如果你有任何问题或建议，请在下面留下评论。

这篇文章最初发表在 Pusher 上。