# Laravel 子域——如何在你的应用中创建和管理子域

> 原文：<https://www.freecodecamp.org/news/laravel-subdomians/>

现代 web 应用程序通常执行不止一个功能。他们通常有不止一个部门，提供不止一种服务，并有几个客户。

但是应用程序的功能越多，你的路线就会越笨拙。

如果有一种方法可以将所有这些部分分成更小的组件，并且有更好、更干净的路线，会怎么样？在同一个网站下，用户可以很容易地独立访问和使用的东西？

还好有这么一个办法:**子域**。

## 什么是子域？

![image-75](img/25ab61cebed62d58c9bd088653bf777a.png)

Credit: [Electroica Blog](https://www.google.com/url?sa=i&url=https%3A%2F%2Fblog.electroica.com%2Fgoogles-top-searches-india-2019%2F&psig=AOvVaw2bx9ZwYjA8ldb4CuGhccN-&ust=1631749915996000&source=images&cd=vfe&ved=0CAwQjhxqFwoTCMiBuKbU__ICFQAAAAAdAAAAABAh)

以下是子域的[基本定义:](https://www.domain.com/blog/subdomain/)

> 子域名，顾名思义，是你的主**域名**的附加部分。您可以创建子域来帮助组织和导航到主网站的不同部分。在你的主域名中，你可以根据需要设置多个子域名来访问你网站的不同页面。

假设你有一个叫 mysite.com 的网站。您有一个博客部分、一个商店部分和一个关于和联系页面的通用网站部分。该网站可以像 blog.mysite.com 和 store.mysite.com 的子域名，主网站将使用主域名。

## 为什么应该使用子域？

子域非常有用，以下是它们的一些主要优势:

*   用户可以很容易地记住你的网站域名，这意味着他们可能会更多地使用你的网站。
*   您可以将您的大型应用程序分成更小的组，这样管理、调试、更新或升级就更容易了。
*   子域也允许个性化——例如，一个博客应用可以给每个用户一个自己的子域(像*username.domain.com*)。
*   子域还允许开发人员在投入生产之前测试他们应用程序的版本。在将变更部署到主站点之前，您可以使用一个*beta.site.com*来预览变更。

让我们通过构建一个实际的项目并进行测试来看看所有这些是如何工作的。

## 如何创建新的 Laravel 项目

我的笔记本电脑上有 [Docker](https://www.docker.com/) 设置，所以我将使用 [Laravel](https://laravel.com/docs/8.x/) 附带的 [Sail](https://laravel.com/docs/8.x/sail) 设置。

```
curl -s "https://laravel.build/example-app" | bash
```

Create Laravel project using sail

> 你可以使用任何你觉得舒服的方法。见[文档](https://laravel.com/docs/8.x/installation)寻求帮助。

### 启动 Laravel 服务器

```
./vendor/bin/sail up -d 
```

Start Laravel server using Sail

## 如何配置路由文件

在您的`web.php`文件中，您可以用它们的域(或子域)定义单独的路由，如下所示:

```
Route::get('/', function () {
    return 'First sub domain';
})->domain('blog.' . env('APP_URL'));
```

Domain definition for single route

现在你可以在*blog.domain.com*访问该页面。

但是通常情况下，在一个应用程序中你会有不止一个路径，比如一个域和子域。因此，使用路由组来覆盖同一域或子域中的所有路由是一个好主意。

```
Route::domain('blog.' . env('APP_URL'))->group(function () {
    Route::get('posts', function () {
        return 'Second subdomain landing page';
    });
    Route::get('post/{id}', function ($id) {
        return 'Post ' . $id . ' in second subdomain';
    });
});
```

Domain definition for route group

现在，该域的所有路由都可以在一个地方处理。

## 如何使子域动态化

正如我前面提到的，你可以使用子域来实现 web 应用程序的个性化，所以它们需要是动态的。例如， [Medium](https://medium.com) 给了作者像【username.domain.com】T2 这样的域名。

你可以在 Laravel 中很容易地做到这一点，因为子域可能被分配路由参数，就像路由 URIs 一样。这允许您捕获子域的一部分，以便在路由闭包或控制器中使用。

```
Route::domain('{username}.' . env('APP_URL'))->group(function () {
    Route::get('post/{id}', function ($username, $id) {
        return 'User ' . $username . ' is trying to read post ' . $id;
    });
});
```

在这个例子中，您可以有一个像*zubair.domain.com*这样的域，它也有路由参数*。*

## 路线服务提供商

对于非常大的应用程序，如果路由不断增加，`web.php`可能会变得有点混乱。最好将路由分成不同的文件，最好是按子域。

在您的`RouteServiceProvider.php`文件中，您将在`boot`方法中看到这段代码:

```
public function boot()
    {
        $this->configureRateLimiting();

        $this->routes(function () {
            Route::prefix('api')
                ->middleware('api')
                ->namespace($this->namespace)
                ->group(base_path('routes/api.php'));

            Route::middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/web.php'));
        });
    }
```

Route Service Provider

这是 Laravel 的默认路由配置，用于将 API 路由与 web 路由分开。我们将使用同一个文件来分隔子域。

将以下内容添加到方法中:

```
Route::domain('blog.' . env('APP_URL'))
                ->middleware('web')
                ->namespace($this->namespace)
                ->group(base_path('routes/blog.php'));
```

这告诉 Laravel，每当有人到达*blog.domain.com*终点时，寻找 blog.php 的路线(我们还没有创建)。

我们可以继续在`routes`文件夹中创建`blog.php`文件，并添加以下内容:

```
<?php

use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    return 'Route using separate file';
}); 
```

至此，您已经完成了所有代码！剩下的只是一些服务器配置。

## 服务器配置

如果你使用的是类似于 [Laravel Valet](https://laravel.com/docs/8.x/valet) 这样的服务，设置起来就容易多了。

在项目的根目录中，运行:

```
valet link domain
valet link blog.domain
```

Valet setup

如果您没有使用 Laravel Valet，您可以将它添加到您的`/etc/hosts/`文件中:

```
127.0.0.1       domain.test
127.0.0.1       blog.domain.test
```

/etc/hosts configuration

这基本上只是将域映射到 IP。

## **总结**

现在你知道如何在你的 Laravel 应用程序中设置和管理子域了。你可以在这里找到这篇文章的所有代码。

如有任何问题或相关建议，欢迎联系我分享。

要阅读更多我的文章或关注我的工作，您可以在 [LinkedIn](https://www.linkedin.com/in/idris-aweda-zubair-5433121a3/) 、 [Twitter](https://twitter.com/AwedaIdris) 和 [Github](https://github.com/Zubs) 上与我联系。又快又简单，还免费！