# 更改 GitHub 用户名的快速指南

> 原文：<https://www.freecodecamp.org/news/a-quick-guide-to-changing-your-github-username/>

## 在 GitHub 上更改用户名后需要考虑的一些额外步骤。

这是我第 238947234 次，也可能是最后一次更改我的用户名，(婚姻是永久的，对吗？)我想我最好写一篇关于如何尽可能平稳地实现这种转变的快速帖子。你可以在这里阅读关于如何更改你的 GitHub 用户名的[官方说明，他们会告诉你怎么做，会发生什么。以下是一些事后需要考虑的 **的快速指南。**](https://help.github.com/en/articles/changing-your-github-username?source=post_page---------------------------)

# 哪里需要改变

1.  在 [GitHub 账户设置中更改用户名。](https://github.com/settings/admin?source=post_page---------------------------)
2.  如果使用 GitHub 页面，请更改“username.github.io”存储库的名称。
3.  如果使用指向您的“username.github.io”存储库地址的其他服务，请更新它们。
4.  如果使用 Netlify，您 **可能** 想要登录并重新连接您的存储库。(我的仍然有效，但是由于一个可能不相关的问题，我不确定。)
5.  登录 Travis CI 和其他集成(在您的存储库设置选项卡->集成和服务中找到它们)。这将更新您的用户名。
6.  使用 **非常小心地执行** `find`和`sed`命令来更新您的本地文件和存储库链接，并将更改推回到 GitHub。
7.  用你更新的 GitHub 链接重新部署你所有的网站。
8.  修复网络上任何链接到你的个人资料，你的知识库，或者你可能已经分享的 Gists。

# 本地文件更新

这里有一些字符串搜索和替换您的用户名的建议。

*   `github.com/username`(在 READMEs 或网站副本中引用您的 GitHub 页面)
*   `username.github.io`(链接到您的 GitHub 页面)
*   `git@github.com:username` (Git 配置远程 ssh URLs)
*   `travis-ci.com/username`(READMEs 中的 Travis 徽章)
*   `shields.io/github/.../username`(READMEs 中的盾徽，类型有`contributors`、`stars`、`tags`等)

对每个字符串使用以下命令，可以快速确定上述字符串的位置:

`grep -rnw -e 'foobar'`

这将递归地(`r`)搜索所有文件，查找匹配所提供的整个(`w`)模式(`e`)的字符串，并在结果前面加上行号(`n`)，这样您就可以很容易地找到它们。

使用`find`和`sed`可以使这些变化更快。参见[这篇关于搜索和替换](https://victoria.dev/verbose/how-to-replace-a-string-in-a-dozen-old-blog-posts-with-one-sed-terminal-command/?source=post_page---------------------------)的文章。

享受你的新手柄！(希望能坚持下去。)