# WordPress 中复杂的自定义数据世界

> 原文：<https://www.freecodecamp.org/news/the-tangled-world-of-custom-data-in-wordpress-2ee8b57d49c/>

让 kamila gorignak

# WordPress 中复杂的自定义数据世界

#### 降低风险和管理您的自定义字段

![T7IWEtq6emQ8ewTaeh51rey6BLxOwcWWD9E8](img/f9f0515ffe1888b440a4b2b74d88135f.png)

source https://pixabay.com

你有没有想过如何正确命名自定义字段的键？有什么区别吗？你应该关心吗？潜在的风险是什么？

最近我写了一篇关于过滤 WordPress 管理视图的教程。在代码中，我将自定义字段命名为 key like `kg_order_items`。你可以问为什么？为什么不直接取名为`items`？嗯…看下面！

如果你尝试谷歌命名自定义字段没有这么多的信息。甚至 [Codex 条目](https://codex.wordpress.org/Custom_Fields)也没有告诉我们如何正确命名字段键。我在 ACF 论坛上只找到[一个包含适当信息的资源。](https://support.advancedcustomfields.com/forums/topic/best-practice-for-name-the-fields/)

如果没有问题，有什么好大惊小怪的？你可以问。

![FPg2xCt4KCRLjf4JzMlakYO1-fuN8qhrY63B](img/ab858982bcf0d0b2f7133bfbd4c60b52.png)

Found on [http://dilbert.com](http://dilbert.com)

WordPress 是一个简单的 CMS，支持只有帖子和页面的小型博客网站的日子已经一去不复返了。今天，即使是最小的网站也使用过多的插件和复杂的主题。所有这些都给游戏带来了新的自定义字段。

如果您使用任何花哨的“高级”主题，情况会变得更糟。不幸的是，这些函数中有许多写得不好，而且将 1001 个函数合二为一。最终结果？速度慢，性能差，混乱的网站，只在演示内容和图片上看起来不错。他们还添加了许多自定义字段。

### 危险！

![m8se5ytEdXwvEddSjmVS3dVkmvy4XZ2x0TCr](img/bdcc45f52768ee8649a74aee8cc95bb5.png)

Source unknown

WordPress `wp_postmeta`表很简单。它是附加到特定 post_id 的键-值对。这意味着所有的**自定义字段键共享一个公共的名称空间**。对于帖子的特定 ID 来说尤其如此。

**第一个例子**
a)想象你的帖子有一个“了解更多”的链接。单击链接后，它会将您重定向到一个特定的 URL。该地址在自定义字段中提供。让我们将字段键命名为`redirect_to`。

b)现在想象你安装了一个插件，比如“重定向我，亲爱的”。这个插件非常非常简单。当用户进入页面时，它会立即根据帖子附带的自定义字段设置重定向用户。哦…它的字段键也被命名为`redirect_to`。

结果？激活插件后，所有带有“了解更多”按钮的帖子都会将用户从你的网站中重定向出去。其原因乍一看并不明显。它甚至可能在相当长的一段时间内不被注意。

当然，这个场景是虚构的，但是危险是真实存在的。随着数以千计的插件和数以千计的主题可用，遇到这样的名称冲突只是时间问题。

**第二个例子**
WordPress 可以为同一个键名和帖子 ID 存储多个值。(除非你提供名为`$unique`的特殊参数)。

这意味着如果你在键`location`下保存你的数据 5 次，当调用`get_post_meta().`时，你将得到一个由 5 个元素组成的数组

让我们假设你有一个关于你去过的城市的帖子。你去过 5 个城市，这些位置都显示在帖子中嵌入的地图上。简单吧？

立正！前面没有有用的代码。；) !

```
//NYadd_post_meta($post_id, 'location', '40.7127753, 73.989308'); 
```

```
//LAadd_post_meta($post_id, 'location', '34.0522342, -118.2436849');
```

```
//Parisadd_post_meta($post_id, 'location', '48.856614, 2.3522219000000177'); 
```

```
//Viennaadd_post_meta($post_id, 'location', '48.2081743, 16.37381890000006'); 
```

```
//Romeadd_post_meta($post_id, 'location', '41.90278349999999, 12.496365500000024');
```

```
//Lets check what we have herevar_dump(get_post_meta($post_id, 'location');
```

```
array (size=5)0 => string '40.7127753, 73.989308' (length=21)1 => string '34.0522342, -118.2436849' (length=24)2 => string '48.856614, 2.3522219000000177' (length=29)3 => string '48.2081743,16.37381890000006' (length=28)4 => string '41.90278349999999,12.496365500000024' (length=36)
```

如果过一段时间你使用一个新的主题或插件。它有一个功能，可以设置一个职位在首页的位置。你可以选择滑动条，侧边栏或者特色文章等等。这个场景可能会这样结束:

```
array (size=6) 0 => string ‘40.7127753, 73.989308’ (length=21) 1 => string ‘34.0522342, -118.2436849’ (length=24) 2 => string ‘48.856614, 2.3522219000000177’ (length=29) 3 => string ‘48.2081743,16.37381890000006’ (length=28) 4 => string ‘41.90278349999999,12.496365500000024’ (length=36) 5 => string ‘left_sidebar’ (length=12) // Yeah, right…
```

或者更糟:

```
array (size=5)  0 => string 'left_sidebar' (length=12)  1 => string 'left_sidebar' (length=12)  2 => string 'left_sidebar' (length=12)  3 => string 'left_sidebar' (length=12)  4 => string 'left_sidebar' (length=12)
```

你漂亮的小地图**现在坏了！**您丢失了所有输入的数据。不好笑对吗？

![UVx09NmJbiPAIBJ1GJ7CgaDLvClyDxRghgYS](img/9ec0a417feaa9edfc13fe1adff038d0d.png)

B-b-b-b-roken?? ;( source https://pixabay.com

### 解决办法

您永远无法保护自定义域数据不被覆盖或删除。这就是 WordPress 的工作方式，也是它如此灵活的原因。你可以通过来降低风险。

**如何？**
通过避免常用名和命名空间**所有**你的自定义字段键。

我提议的惯例是:

*   **cpt-name_field-name**
    像`books_author`而不是`author`，`order_items`而不是`items`(针对大多数懒人的解决方案:)。
*   **目的 _ 字段名称**
    喜欢用`front_page_location`代替位置，`visited_cities_locations`代替`location`。
*   **前缀 _(CPT-name/purpose)_ 字段名**
    如`kg_books_author`、`kg_visited_cities_locations`(最严格的)。

这还不是全部。此外，你应该注意内置 WordPress 函数的可选参数:

*   [add_post_meta()](https://codex.wordpress.org/Function_Reference/add_post_meta) 有`$unique`不添加自定义字段，如果它已经存在。
*   [get_post_meta()](https://developer.wordpress.org/reference/functions/get_post_meta/) 使用`$single`只检索一条记录(如果你只期望一条记录)。
*   [update_post_meta()](https://codex.wordpress.org/Function_Reference/update_post_meta) 和 [delete_post_meta()](https://codex.wordpress.org/Function_Reference/delete_post_meta) 利用`$previous_value`来确保您更新/删除您想要的密钥。

这些参数有助于编写更好、更清晰、更可预测的代码。

这还不是全部。使用测试良好、编写良好且可扩展的插件，如 [Pods 框架](https://pods.io/)或[高级定制字段](https://www.advancedcustomfields.com/)。这些将有助于管理您的自定义字段。在管理复杂的定制数据时，它们非常有用。

### 摘要

在理想的世界中，我们应该时刻意识到你正在向系统中添加什么。我们应该知道你的插件、主题和自定义功能在做什么。不幸的是，这并不总是可能的。

因此，我们应该关注我们产生的代码，并收紧所有那些松散的结束。

那都是乡亲们！我希望你喜欢它，并有一个伟大的一天！

这篇文章最初发表在[我的私人博客](https://kamilgrzegorczyk.com/2017/10/12/best-practices-naming-convention-for-wordpress-custom-fields/)上，在那里我写了关于 WordPress 和一般开发的文章。