# 你需要知道的 WordPress 漏洞——以及如何修复它们

> 原文：<https://www.freecodecamp.org/news/wordpress-vulnerabilities-you-need-to-know-about-and-how-to-fix-them-497a2d8b2c3e/>

乔尔·s·西德

# 你需要知道的 WordPress 漏洞——以及如何修复它们

![JgyzQHVAbg5p2fp8dAymhNJzTmE7yBb8HNeN](img/b89f9a643c9d047405f13897a38c502e.png)

Photo by [rawpixel](https://unsplash.com/photos/zHNS-vrAVGU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/website?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

WordPress 对于各种博客来说是一个非常有用的多功能平台。它变得非常受欢迎。不幸的是，这种流行带来了相当多的可被黑客利用的漏洞。了解这些弱点并采取措施防止它们被利用是很重要的。这里有六个你需要知道的 WordPress 漏洞。

#### **WordPress 登录**

“有人访问你的 WordPress 最简单的方法就是攻击你的登录。这种暴力攻击是让你的 WordPress 受损的最常见的方式。”这种方法的工作原理是通过使用某些软件反复猜测你的登录名和密码，”玛莎·戈斯建议道，她是 [WriteMyX](https://writemyx.com/) 和[英国学生](https://britstudent.com/)的 WP 程序员。

WordPress 不限制登录尝试的次数，所以暴力攻击可能非常有效。防止此漏洞的最佳方法是安装一个插件来限制允许的登录尝试次数，如 iThemes Security Pro。您还可以使用密码管理器来生成不太容易被猜到的随机密码。远离那些看似显而易见的密码——不要使用 123456、密码或任何与你相关的东西。

例如，不要用宠物名、孩子名、伴侣名、街道名等等，因为所有这些都可能被用来对付你。如果您认为将它们混合在一起可能被证明是更好的安全措施，但仍然很容易记住，请三思。你的社交媒体资料是了解你心理的一个简单途径，任何人都可以在你的帖子中找到这些密码。

最好选择一些与你无关的东西——确保你使用了字母、大写字母、数字和符号的良好组合。你不能太小心或低估你的敌人。

来源:[https://www . word fence . com/blog/2017/12/aggressive-brute-force-WordPress-attack/](https://www.wordfence.com/blog/2017/12/aggressive-brute-force-wordpress-attack/)

#### **默认管理员用户帐户**

黑客可以通过利用你的默认管理员帐户来获得你的 WordPress 帐户的后端。尝试删除您的管理员帐户，并从具有管理员权限的普通帐户访问您的网站。人们可以通过使用 cookie 值参数注入 SQL 命令来获得访问权限。为了防止这种危害，请尝试将您的安全软件更新到最新的 IPS。

季布专业版也可以通过跨站点脚本访问，因为它不能正确清除用户提供的输入。要防范此漏洞，请加强您站点上的权限。你甚至可以隐藏你的管理员登录，这样就没有人能找到它。最好的方法——也是最简单的方法——是给它取别的名字。Admin 是显而易见的，黑客会搜索它。但是如果你给它起一个完全不同的名字——猫、太空猴或者从各种各样有趣和聪明的可能性中选择——黑客将很难找到它。默认情况下，他们将很难注入 SQL 命令——他们不知道在哪里注入。

来源:[https://www . acune tix . com/blog/articles/exploining-SQL-injection-example/](https://www.acunetix.com/blog/articles/exploiting-sql-injection-example/)

#### **URL 黑客**

“WordPress 使用 PHP 来执行所有的服务器端脚本，不幸的是，这使得它容易受到 URL 攻击。“黑客很容易潜入 WordPress 的操作系统，给你制造麻烦，”科技作家多萝西·考克斯警告说。由于其数据库的性质，黑客有足够的机会窃取您的敏感信息。为了避免这些漏洞，把你的 WordPress 放在 Apache Web 服务器上，它使用。htaccess 文件，将保护您免受网址黑客攻击。

确保卸载并删除网站上所有不必要的插件。这限制了黑客的访问点——主题也可能是你的敌人，尤其是如果你有大量你没有使用的主题。PHP 太容易被利用了，你应该尽你所能保护你的网站。作为奖励，这些主题和额外的插件可能会减缓你的加载时间，所以你会提高你的搜索引擎优化，同时保护你的网站。

来源:[https://blog . web securify . com/2017/02/hacking-WordPress-4-7-0-1 . html](https://blog.websecurify.com/2017/02/hacking-wordpress-4-7-0-1.html)

#### **过时的软件**

任何时候你使用过时的软件运行你的 WordPress，你都有被入侵的风险。人们很容易陷入这样一个陷阱，认为更新只是改善了一些小的错误和烦恼，但它们对它们所包含的安全补丁也很重要。

保持您的软件更新是一件非常容易的事情，它将保护您免受许多黑客威胁。如果你担心，你会忘记更新你的软件，简单地安装自动更新插件，如 iThemes 的 WordPress 版本管理。这些功能既能节省您的时间，又能确保您不会错过安全补丁并危及您的网站。

黑客会寻找容易破解的东西。通过更新，您可能正在安装新的安全补丁，可以防止任何黑客进入。此外，请确保不要安装来自不安全来源的任何东西。你可能会邀请黑客进来。

来源:[https://www . Kimi web . com/WordPress-help/updating-WordPress-and-plugins/](https://www.kimiweb.com/wordpress-help/updating-wordpress-and-plugins/)

#### **数据库表的默认前缀**

WordPress 数据库中有许多表格，在许多安装中，它们以一个默认前缀命名，前缀以“ *wp* 开头这听起来没什么大不了的，但是这给了黑客一点优势，因为他们少了一件需要破解的东西。

您可以通过简单地更改这些默认前缀来使事情变得更加困难。会阻止每一个黑客吗？不，但它会停止很多，这只是一个更有用的工具来捍卫你的 WordPress 帐户。这是一个简单的过程，你可以自己轻松完成——黑客总是先寻找一个简单的切入点，如果你排除了这些，他们可能会放弃。

来源:[https://www . cloudways . com/blog/change-WordPress-database-table-prefix-manually/](https://www.cloudways.com/blog/change-wordpress-database-table-prefix-manually/)

#### **PHP 漏洞**

黑客可以通过利用您的 PHP 代码来访问您的帐户，这比您想象的更常见。通过卸载然后删除任何你不需要的插件来限制你的风险。每一个都是潜在的访问点，可能会危及您的信息安全。尽量避免使用不再更新的插件；如果更新已经超过六个月了，是时候考虑删除它了。

来源:[https://www . cloudways . com/blog/change-WordPress-database-table-prefix-manually/](https://www.cloudways.com/blog/change-wordpress-database-table-prefix-manually/)

#### **切换到更好的主机**

当你刚刚起步时，最便宜的主机服务似乎是最好的主意。但是，廉价主机也缺乏安全功能，这可能是有害的。确保你有最好和最安全的主机来保护你自己和你的客户。

来源:[https://www . webhostingsecretrevealed . net/blog/we b-hosting-guides/switching-we b-host/](https://www.webhostingsecretrevealed.net/blog/web-hosting-guides/switching-web-host/)

#### **结论**

当你运行一个 WordPress 账户时，保持你的网站安全是最重要的责任之一。限制您登录的尝试次数。更改表的默认前缀。下载本文推荐的插件，看看它们如何为您工作。WordPress 是一个很好的博客资源，但是请记住，有很多人在网站上寻找容易被黑客攻击的账户。确保你的不是其中之一。

乔尔·西德是 Originwritings.com和 Australia2write.com的 WP 教练。他喜欢帮助人们建立他们梦想中的网站，也喜欢为辅导服务公司[Academicbrits.com](https://academicbrits.com/)撰写让他兴奋的文章。