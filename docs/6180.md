# 如何用 Dagger 2 在你的应用中实现依赖注入

> 原文：<https://www.freecodecamp.org/news/kriptofolio-app-series-part-4/>

#### Kriptofolio app series - Part 4

依赖注入将显著改善您的代码。它使你的代码更加模块化、灵活和可测试。实际上，它的名字听起来比它背后的想法更复杂。

在本系列的这一部分，我们将学习依赖注入。然后，我们将在“Kriptofolio”(以前的“我的加密硬币”)应用程序实现它。我们将使用匕首 2。Dagger 2 是 Android 最流行的开源依赖注入框架。这对于创建现代应用程序来说是一项有价值的技能，尽管学习曲线已经够难了。

### 系列内容

*   [简介:2018–2019 年打造现代 Android 应用的路线图](https://www.freecodecamp.org/news/kriptofolio-app-series)
*   [第一部分:固体原理介绍](https://www.freecodecamp.org/news/kriptofolio-app-series-part-1)
*   第 2 部分:如何开始构建你的 Android 应用:创建模型、UI 和 XML 布局
*   第 3 部分:关于架构的一切:探索不同的架构模式以及如何在你的应用中使用它们
*   第 4 部分:如何用 Dagger 2(你在这里)在你的应用中实现依赖注入
*   [第 5 部分:使用 refuge、OkHttp、Gson、Glide 和协程处理 RESTful Web 服务](https://www.freecodecamp.org/news/kriptofolio-app-series-part-5)

### 什么是依赖注入？

为了解释依赖注入，我们首先要理解依赖在编程中的含义。依赖是指一个对象依赖于另一个对象的具体实现。每当在一个对象中实例化另一个对象时，都可以在代码中标识依赖项。让我们来看一个实际的例子。

```
class MyAppClass() {

    private val library: MyLibrary = MyLibrary(true)
    ...
}

class MyLibrary(private val useSpecialFeature: Boolean) {

    ...
}
```

正如你从这个例子中看到的，你的类`MyAppClass` 将直接依赖于你的库类`MyLibrary`的具体配置和实现。如果有一天你想使用第三方库呢？如果您希望在另一个类中使用完全相同的库配置，该怎么办？每次你都必须搜索你的代码，找到确切的位置并修改它。这只是几个例子。

这个想法是，随着项目的增长，应用程序组件之间的这种紧密耦合将使您的开发工作更加努力。为了避免任何问题，让我们使用依赖注入来放松所描述的耦合。

```
class MyAppClass(private val library: MyLibrary) {

    ...
}

class MyLibrary(private val useSpecialFeature: Boolean) {

    ...
}
```

就是这样，这是一个非常原始的依赖注入例子。不用在你的类`MyAppClass`中创建和配置一个新的`MyLibrary`类对象，你只需将它传递或注入到构造函数中。所以`MyAppClass`完全可以对`MyLibrary`不负责任。

### 匕首 2 是什么？

Dagger 是一个完全静态的编译时开源依赖注入框架，适用于 Java 和 Android。在这篇文章中，我将谈论谷歌维护的第二个版本。Square 创建了它的早期版本。

Dagger 2 被认为是迄今为止构建的最有效的依赖注入框架之一。事实上，如果你比较匕首 1，匕首 2 和匕首 2.10，你会发现每个实现是不同的。你每次都需要重新学习，因为作者做了重大的修改。写这篇文章时，我使用 Dagger 2.16 版本，我们将只关注它。

正如您现在对依赖注入的理解，我们的类不应该创建或拥有依赖。相反，他们需要从外部获取一切。所以在使用 Dagger 2 的时候，这个框架会提供所有需要的依赖。

它通过为我们生成大量样板代码来做到这一点。所生成的代码将是完全可追踪的，并且将模仿用户可能手写的代码。Dagger 2 是用 Java 编写的，其注释处理器生成的代码也将是 Java 代码。

然而，它与 Kotlin 一起工作，没有任何问题或修改。请记住，Kotlin 完全可以与 Java 互操作。如果与类似的框架相比，Dagger 2 是一个不太动态的框架。它在编译时工作，而不是在反射的运行时工作。根本没有反射用法。这意味着这个框架将更难建立和学习。它将提供编译时安全性的性能提升。

### 无需工具的手动依赖注入

您可能已经注意到，在上一部分的 My Crypto Coins app [源代码中，有一段代码用于在不使用任何依赖注入工具的情况下注入对象。它工作得很好，这个解决方案对于这样一个小应用程序来说已经足够好了。看一下实用程序包:](https://github.com/baruckis/Kriptofolio/tree/Part-3)

```
/**
 * Static methods used to inject classes needed for various Activities and Fragments.
 */
object InjectorUtils {

    private fun getCryptocurrencyRepository(context: Context): CryptocurrencyRepository {
        return CryptocurrencyRepository.getInstance(
                AppDatabase.getInstance(context).cryptocurrencyDao())
    }

    fun provideMainViewModelFactory(
            application: Application
    ): MainViewModelFactory {
        val repository = getCryptocurrencyRepository(application)
        return MainViewModelFactory(application, repository)
    }

    fun provideAddSearchViewModelFactory(
            context: Context
    ): AddSearchViewModelFactory {
        val repository = getCryptocurrencyRepository(context)
        return AddSearchViewModelFactory(repository)
    }
}
```

如您所见，这个类将完成所有工作。它将为需要它们的活动或片段创建视图模型工厂。

```
/**
 * Factory for creating a [MainViewModel] with a constructor that takes a
 * [CryptocurrencyRepository].
 */
class MainViewModelFactory(private val application: Application, private val repository: CryptocurrencyRepository) : ViewModelProvider.NewInstanceFactory() {

    @Suppress("UNCHECKED_CAST")
    override fun <T : ViewModel?> create(modelClass: Class<T>): T {
        return MainViewModel(application, repository) as T
    }

}
```

然后像这样使用`InjectorUtils`类，您需要得到一个特定的视图模型工厂:

```
/**
 * A placeholder fragment containing a simple view.
 */
class MainListFragment : Fragment() {

    ...

    private lateinit var viewModel: MainViewModel

    ...

    override fun onActivityCreated(savedInstanceState: Bundle?) {

        super.onActivityCreated(savedInstanceState)

        setupList()
        ...
    }

    ...

    private fun subscribeUi(activity: FragmentActivity) {

        // This is the old way how we were injecting code before using Dagger.
        val factory = InjectorUtils.provideMainViewModelFactory(activity.application)

        // Obtain ViewModel from ViewModelProviders, using parent activity as LifecycleOwner.
        viewModel = ViewModelProviders.of(activity, factory).get(MainViewModel::class.java)

        ...
    }

}
```

如你所见，我们的`MainListFragment`班甚至不知道`CryptocurrencyRepository`或`AppDatabase`。它从 InjectorUtils 类获得一个成功构造的工厂。实际上这是一种简单的方法。我们将摆脱它，并学习如何设置 Dagger 2 工具进行高级依赖注入。如果这个应用程序在功能和代码上有所扩展，我不怀疑我们会很快看到使用专业的依赖注入框架比手动解决方案的好处。

所以让我们现在删除`InjectorUtils`类，学习如何在我的加密硬币应用程序源代码中设置 Dagger 2。

### 使用 Kotlin 为 MVVM 进行依赖注入

#### 如何用视图模型、活动和片段设置 Dagger 2

现在，我们将通过匕首 2 逐步安装我的加密硬币应用程序项目。

首先，你应该启用 Kotlin 自己的[注释处理工具](https://kotlinlang.org/docs/reference/kapt.html) (kapt)。然后添加特殊的匕首 2 依赖。

您可以通过将这些行添加到 gradle 文件中来做到这一点:

```
apply plugin: 'kotlin-kapt' // For annotation processing

...

implementation "com.google.dagger:dagger:$versions.dagger"
implementation "com.google.dagger:dagger-android:$versions.dagger"
implementation "com.google.dagger:dagger-android-support:$versions.dagger"
kapt "com.google.dagger:dagger-compiler:$versions.dagger"
kapt "com.google.dagger:dagger-android-processor:$versions.dagger"
```

Module: app

Kapt 插件将使编译器能够生成 Java 和 Kotlin 之间互操作所需的存根类。为了方便起见，我们将在一个单独的 gradle 文件中定义具体的 Dagger 2 版本，正如我们对所有依赖项所做的那样。

```
def versions = [:]

versions.dagger = "2.16"

ext.versions = versions
```

要找到可用的最新版本，请查看 Github 上的 [Dagger 2 官方资源库的发布版本。](https://github.com/google/dagger)

**现在，创建您的应用程序`App`类。**

如果您已经设置了此类，请跳过此步骤。在你做了那件事之后，我们将暂时让它保持原样，但是稍后再回来。

```
class App : Application() {

    override fun onCreate() {
        super.onCreate()
    }

}
```

对于我的加密硬币应用程序，我们已经在前面创建了应用程序类。

**接下来，更新您的清单文件以启用您的`App`类。**

如果您之前已经这样做了，请跳过此步骤。

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.baruckis.mycryptocoins">

    <application
        android:name=".App"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        ...
```

对于我的 Crypto Coins 应用程序，我们之前已经在清单中设置了`App`类。

**现在让我们创建一个名为`dependencyinjection`的新包。**

在这里，我们将保留所有与 Dagger 实现相关的文件。

**创建`AppModule`类模块，该模块将在整个应用程序中提供依赖关系。**

```
/**
 * AppModule will provide app-wide dependencies for a part of the application.
 * It should initialize objects used across our application, such as Room database, Retrofit, Shared Preference, etc.
 */
@Module(includes = [ViewModelsModule::class])
class AppModule() {

    @Singleton // Annotation informs Dagger compiler that the instance should be created only once in the entire lifecycle of the application.
    @Provides // Annotation informs Dagger compiler that this method is the constructor for the Context return type.
    fun provideContext(app: App): Context = app // Using provide as a prefix is a common convention but not a requirement.

    @Singleton
    @Provides
    fun provideCryptocurrencyRepository(context: Context): CryptocurrencyRepository {
        return CryptocurrencyRepository.getInstance(AppDatabase.getInstance(context).cryptocurrencyDao())
    }
}
```

如您所见，要创建 Dagger 模块，我们需要用特殊的`@Module`注释对其进行注释。项目通常有多个 Dagger 模块。其中一个通常会提供应用程序范围的依赖关系。这个`AppModule`将用于初始化应用程序中使用的对象，例如房间数据库、翻新、共享偏好等。

作为一个例子，我们可以讨论一个非常常见的场景，让 AppModule 提供一个上下文对象，以防我们需要它在我们的应用程序中的任何地方访问它。让我们分析一下代码，看看如何做到这一点。

我们需要使用特殊的匕首注释`@Provides`。它告诉 Dagger 这个方法提供了一个特定类型的依赖，在我们的例子中，是一个上下文对象。因此，当我们在应用程序中的某个地方请求注入上下文时，AppModule 是 Dagger 找到它的地方。我们的方法名称无关紧要，因为 Dagger 只关心返回类型。用 provide 前缀命名方法是唯一常见的做法，但是它可以是您想要的任何名称。

您看到的应用于相同方法的`@Singleton`注释不是 Dagger 注释的一部分。它包含在 javax 包中。这个注释告诉 Dagger，这个依赖项应该只有一个实例。

您不需要编写样板代码来检查对象的另一个实例是否已经可用。在生成代码时，Dagger 会因为这个注释而为您处理所有的逻辑。请注意，我们的 AppModule 包括另一个模块 ViewModelsModule。让我们现在创建它。

**创建一个`ViewModelsModule`类模块。这个模块将负责在整个应用程序中提供视图模型。**

```
/**
 * Will be responsible for providing ViewModels.
 */
@Module
abstract class ViewModelsModule {

    // We'd like to take this implementation of the ViewModel class and make it available in an injectable map with MainViewModel::class as a key to that map.
    @Binds
    @IntoMap
    @ViewModelKey(MainViewModel::class) // We use a restriction on multibound map defined with @ViewModelKey annotation, and if don't need any, we should use @ClassKey annotation provided by Dagger.
    abstract fun bindMainViewModel(mainViewModel: MainViewModel): ViewModel

    @Binds
    @IntoMap
    @ViewModelKey(AddSearchViewModel::class)
    abstract fun bindAddSearchViewModel(addSearchViewModel: AddSearchViewModel): ViewModel

    @Binds
    abstract fun bindViewModelFactory(factory: ViewModelFactory): ViewModelProvider.Factory
}
```

这个模块使用 Dagger 2 特征映射多重绑定。使用它，我们可以将我们选择的对象添加到地图中，地图可以在我们的应用程序中的任何地方注入。使用 Dagger 注释`@Binds`、`@IntoMap`和我们的自定义注释`@ViewModelKey`(我们将要创建的这个)，我们在我们的地图中创建一个带有键`MainViewModel::class`和值`MainViewModel`实例的条目。我们借助一些通用的`ViewModelFactory`类绑定特定的工厂。我们需要创建这个类。

**创建一个自定义注释类`ViewModelKey`。**

```
/**
 * An annotation class which tells dagger that it can be used to determine keys in multi bound maps.
 */
@MustBeDocumented
@Target(
        AnnotationTarget.FUNCTION,
        AnnotationTarget.PROPERTY_GETTER,
        AnnotationTarget.PROPERTY_SETTER
)
@Retention(AnnotationRetention.RUNTIME)
@MapKey
annotation class ViewModelKey(val value: KClass<out ViewModel>) // We might use only those classes which inherit from ViewModel.
```

该类用于绑定`ViewModelsModule`中的视图模型。具体标注`@ViewModelKey`代表我们地图的关键。我们的密钥只能是从`ViewModel`继承的类。

**创建`ViewModelFactory`类。**

```
/**
 * Factory to auto-generate a Class to Provider Map.
 * We use Provider<T> to create an injectable object at a later time.
 */
@Suppress("UNCHECKED_CAST")
@Singleton
class ViewModelFactory @Inject constructor(private val viewModelsMap: Map<Class<out ViewModel>,
        @JvmSuppressWildcards Provider<ViewModel>>) : ViewModelProvider.Factory {

    override fun <T : ViewModel> create(modelClass: Class<T>): T {
        var creator: Provider<out ViewModel>? = viewModelsMap[modelClass]
        if (creator == null) {
            for (entry in viewModelsMap.entries) {
                if (modelClass.isAssignableFrom(entry.key)) {
                    creator = entry.value
                    break
                }
            }
        }
        if (creator == null) {
            throw IllegalArgumentException("Unknown model class $modelClass")
        }

        try {
            return creator.get() as T
        } catch (e: Exception) {
            throw RuntimeException(e)
        }
    }
}
```

这个`ViewModelFactory`是一个帮助你动态创建视图模型的工具类。这里，您提供生成的地图作为参数。`create()`方法将能够从地图中选择正确的实例。

**创建`ActivityBuildersModule`类模块。**

```
/**
 * All activities intended to use Dagger @Inject should be listed here.
 */
@Module
abstract class ActivityBuildersModule {

    @ContributesAndroidInjector(modules = [MainListFragmetBuildersModule::class]) // Where to apply the injection.
    abstract fun contributeMainActivity(): MainActivity

    @ContributesAndroidInjector
    abstract fun contributeAddSearchActivity(): AddSearchActivity
}
```

该模块负责构建您的所有活动。它将为该类中定义的所有活动生成`AndroidInjector`。然后可以使用活动生命周期中的`onCreate`函数中的`AndroidInjection.inject(this)`将对象注入到活动中。注意，这个模块还使用了另一个负责片段的独立模块。接下来我们将创建这个模块。

**创建`MainListFragmetBuildersModule`类模块。**

```
/**
 * All fragments related to MainActivity intended to use Dagger @Inject should be listed here.
 */
@Module
abstract class MainListFragmetBuildersModule {

    @ContributesAndroidInjector() // Attaches fragment to Dagger graph.
    abstract fun contributeMainListFragment(): MainListFragment
}
```

这个模块将构建你所有与`MainActivity`相关的片段。它将为这个类中定义的所有片段生成`AndroidInjector`。可以使用片段生命周期中`onAttach`函数中的`AndroidSupportInjection.inject(this)`将对象注入到片段中。

**创建`AppComponent`类组件。**

```
/**
 * Singleton component interface for the app. It ties all the modules together.
 * The component is used to connect objects to their dependencies.
 * Dagger will auto-generate DaggerAppComponent which is used for initialization at Application.
 */
@Singleton
@Component(
        modules = [
            // AndroidSupportInjectionModule is a class of Dagger and we don't need to create it.
            // If you want to use injection in fragment then you should use AndroidSupportInjectionModule.class else use AndroidInjectionModule.
            AndroidSupportInjectionModule::class,
            AppModule::class,
            ActivityBuildersModule::class
        ]
)
interface AppComponent {

    @Component.Builder // Used for instantiation of a component.
    interface Builder {

        @BindsInstance // Bind our application instance to our Dagger graph.
        fun application(application: App): Builder

        fun build(): AppComponent
    }

    // The application which is allowed to request the dependencies declared by the modules
    // (by means of the @Inject annotation) should be declared here with individual inject() methods.
    fun inject(app: App)
}
```

组件是一个非常重要的类。它将使上述所有内容开始协同工作。它通过将对象连接到它们的依赖项来实现这一点。Dagger 将使用这个接口来生成执行依赖注入所必需的代码。

要创建一个组件类，你需要使用 Dagger 注释`@Component`。它接受一个模块列表作为输入。另一个注释`@Component.Builder`允许我们将一些实例绑定到组件。

**然后生成一个图形对象。**

此时，您已经拥有了所有的模块和组件设置。您可以通过在 Android Studio IDE 中选择 Build -> Make Module 来生成您的 graph 对象。我们需要这一代人来迈出未来的步伐。

**现在创建一个`Injectable`接口。**

```
/**
 * It is just a plain empty marker interface, which tells to automatically inject activities or fragments if they implement it.
 */
interface Injectable
```

这也是未来步骤所需要的。接口应该由我们希望自动注入的活动或片段来实现。

**创建一个名为`AppInjector`的新助手类。**

```
/**
 * It is simple helper class to avoid calling inject method on each activity or fragment.
 */
object AppInjector {
    fun init(app: App) {
        // Here we initialize Dagger. DaggerAppComponent is auto-generated from AppComponent.
        DaggerAppComponent.builder().application(app).build().inject(app)

        app.registerActivityLifecycleCallbacks(object : Application.ActivityLifecycleCallbacks {
            override fun onActivityPaused(activity: Activity) {

            }

            override fun onActivityResumed(activity: Activity) {

            }

            override fun onActivityStarted(activity: Activity) {

            }

            override fun onActivityDestroyed(activity: Activity) {

            }

            override fun onActivitySaveInstanceState(activity: Activity, outState: Bundle?) {

            }

            override fun onActivityStopped(activity: Activity) {

            }

            override fun onActivityCreated(activity: Activity, savedInstanceState: Bundle?) {
                handleActivity(activity)
            }
        })
    }

    private fun handleActivity(activity: Activity) {
        if (activity is HasSupportFragmentInjector || activity is Injectable) {
            // Calling inject() method will cause Dagger to locate the singletons in the dependency graph to try to find a matching return type.
            // If it finds one, it assigns the references to the respective fields.
            AndroidInjection.inject(activity)
        }

        if (activity is FragmentActivity) {
            activity.supportFragmentManager.registerFragmentLifecycleCallbacks(object : FragmentManager.FragmentLifecycleCallbacks() {
                override fun onFragmentCreated(fragmentManager: FragmentManager, fragment: Fragment, savedInstanceState: Bundle?) {
                    if (fragment is Injectable) {
                        AndroidSupportInjection.inject(fragment)
                    }
                }
            }, true)
        }
    }

}
```

它只是一个简单的助手类，以避免在每个活动或片段上调用 inject 方法。

**接下来，设置我们之前已经创建的`App`类。**

```
class App : Application(), HasActivityInjector {

    @Inject // It implements Dagger machinery of finding appropriate injector factory for a type.
    lateinit var dispatchingAndroidInjector: DispatchingAndroidInjector<Activity>

    override fun onCreate() {
        super.onCreate()

        // Initialize in order to automatically inject activities and fragments if they implement Injectable interface.
        AppInjector.init(this)

        ...
    }

    // This is required by HasActivityInjector interface to setup Dagger for Activity.
    override fun activityInjector(): AndroidInjector<Activity> = dispatchingAndroidInjector
}
```

因为应用程序有活动，我们需要实现`HasActivityInjector`接口。如果你看到 Android Studio 在`DaggerAppComponent`上调出一个错误，那是因为你没有生成一个新的文件，就像上一步指出的那样。

**因此，设置`MainActivity`来注入主视图模型工厂，并添加对片段注入的支持。**

```
// To support injecting fragments which belongs to this activity we need to implement HasSupportFragmentInjector.
// We would not need to implement it, if our activity did not contain any fragments or the fragments did not need to inject anything.
class MainActivity : AppCompatActivity(), HasSupportFragmentInjector {

    @Inject
    lateinit var dispatchingAndroidInjector: DispatchingAndroidInjector<Fragment>

    @Inject
    lateinit var viewModelFactory: ViewModelProvider.Factory
    private lateinit var mainViewModel: MainViewModel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Obtain ViewModel from ViewModelProviders, using this activity as LifecycleOwner.
        mainViewModel = ViewModelProviders.of(this, viewModelFactory).get(MainViewModel::class.java)

        ...
    }

    ...

    override fun supportFragmentInjector(): AndroidInjector<Fragment> = dispatchingAndroidInjector

    ...
}
```

因为我们的活动有子片段，所以我们需要实现`HasSupportFragmentInjector`接口。我们也需要这个，因为我们计划在我们的片段中进行注射。我们的活动不应该知道它是如何被注入的。我们在重写`onCreate()`方法时使用了`AndroidInjection.inject(this)`代码行。

调用`inject()`方法将导致 Dagger 2 定位依赖图中的单例，以试图找到匹配的返回类型。然而，我们不需要在这里写任何代码，因为它已经由之前创建的`AppInjector` helper 类为我们完成了，我们在应用程序类中初始化了它。

**然后，设置`MainListFragment`来注入主 ViewModel 工厂。**

```
/**
 * A placeholder fragment containing a simple view.
 */
class MainListFragment : Fragment(), Injectable {

    ...

    @Inject
    lateinit var viewModelFactory: ViewModelProvider.Factory
    private lateinit var viewModel: MainViewModel

    ...

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)

        ...
        subscribeUi(activity!!)
    }

    ...

    private fun subscribeUi(activity: FragmentActivity) {

        // Obtain ViewModel from ViewModelProviders, using parent activity as LifecycleOwner.
        viewModel = ViewModelProviders.of(activity, viewModelFactory).get(MainViewModel::class.java)

        ...

    }

}
```

类似于活动，如果我们希望我们的片段是可注入的，那么在它的`onAttach`方法中我们应该编写代码`AndroidSupportInjection.inject(this)`。但是同样这是由`AppInjector`助手完成的工作，所以我们可以跳过它。请注意，我们需要添加之前为助手创建的`Injectable`接口。

恭喜你，我们已经在我的加密硬币应用程序项目中实现了匕首 2。当然，这篇文章是在你的应用中直接部署 Dagger 2 的快速指南，但不是它的深度报道。如果你对基础知识感到迷茫，我建议你继续研究这个话题。

### 贮藏室ˌ仓库

在 GitHub 上查看更新后的“Kriptofolio”(之前的“我的加密硬币”)应用程序的源代码。

#### [在 GitHub 上查看源代码](https://github.com/baruckis/Kriptofolio/tree/Part-4)

* * *

***aěIū！感谢阅读！我最初于 2018 年 10 月 7 日为我的个人博客【www.baruckis.com[发表了这篇文章。](https://www.baruckis.com/android/kriptofolio-app-series-part-4/)***