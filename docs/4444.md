# 如何从命令行自动化您的项目和 Github repo 设置

> 原文：<https://www.freecodecamp.org/news/automate-project-github-setup-mac/>

这篇文章来自于我第一次学习编码时所面临的一个烦恼——设置我的本地 repo 并与 Github 同步。

我从做项目中学到的(经常是 freeCodeCamp 的！).但是我需要确保我没有失去我的努力，并且其他人可以看到我的努力，所以每个项目*都有*可以在 Github 上运行。[我在 Github 上的项目越完整，招聘人员就越容易找到。但是建立一个项目，初始化一个 repo，以及与 Github 同步所需的步骤真的很烦人而且重复，所以我决定解决这个问题。](https://www.freecodecamp.org/news/learned-to-code-job-ready-and-heres-why/)

坏消息:这不会是一个大的，花哨的，详细的和技术上性感的帖子。这会非常不性感。

好消息是:你不需要成为一个 shell 脚本上帝(dess)来做这件事。

所以我典型的项目设置工作流程通常是这样的:

1)进入我的`../projects`文件夹，运行`mkdir project-of-some-name`创建一个名为`project-of-some-name`的文件夹。

2) `cd`到项目文件夹中，执行`git init`在那里初始化一个本地 git repo。

3)运行`touch README.MD`创建`README`文件，打开它并添加一些基本描述，包括我在该项目中实现的资源/教程的链接。保存文件。

4)运行`git add .`，然后运行`git commit -m ' ...some initial commit message...`

5)打开浏览器，进入 Github，登录，创建一个新的(远程)存储库，复制 url，返回到我的终端，确保我在正确的项目文件夹`project-of-some-name`...然后运行将远程回购设置为“上游”回购所需的 git 脚本，并将我的本地回购连接到它。然后，最后，我可以运行一个`git push`,我的本地提交将被推高

6)躺下小睡一会儿，因为这个重复的过程而筋疲力尽。

诚然，这是我的过程，但我喜欢保持有组织性，并且总是能够访问我的项目，以便我可以参考它们。

因为自动化是练习编码技能的好方法，所以我决定编写一个小的 shell 脚本来自动化这些可怕的重复步骤。脚本在这篇文章的底部，请注意——它并不复杂也不花哨。但是它确实完成了工作，而且我不需要登录 Github 来完成所有这些步骤！

在拷贝脚本之前，您需要知道如何在 Mac 上运行它。下面是您需要执行的步骤，以便能够使用脚本来自动化您的设置工作流。

1)我将我的脚本保存在我的根/主文件夹中，在一个名为`scripts`的子文件夹中。建议你也这么做或者类似的。要进入根目录/主文件夹，在终端中输入`cd ~`，因为波浪号(`~`)是主文件夹的符号。在您的 Mac Finder 应用程序中，它显示为带有房屋图标的图标。所以我所有的脚本都存储在`~/scripts`

2)这很重要，因为要从终端中的任何目录运行 shell 脚本，您必须键入完整的路径。在我的例子中，我必须键入`~/scripts/git-script.sh`来运行脚本。但是我们太超前了。

3)复制这篇文章底部的代码块，然后打开一个文本编辑器，粘贴并保存为`[filename].sh`。`.sh`是外壳脚本的扩展。将文件保存在你想保存它的目录中——我再次推荐将`~/scripts`作为保存你的脚本的文件夹。

4)导航到终端中的文件夹。为了安全起见，在终端中运行`ls`,检查是否可以看到脚本在那里。如果不是，则说明您在错误的文件夹中，或者步骤 3 没有成功完成。

5)使外壳脚本可执行。为此，您可以在终端中键入以下内容:`chmod +x <<the-correct-filename.sh>>`。这是使 shell 脚本“可执行”的 unix 方式。我不确定我是否完全理解这意味着什么，除了它需要使你写的任何 shell 脚本可执行，所以不要问我，我不会骗你。

6)导航到您的项目文件夹，并创建一个新文件夹来存放您的项目。实际上，你必须这样做:`mkdir` -在保存所有项目的文件夹中创建一个`project-of-some-name`。所以你的项目最终会放在`my-computer/my-projects/project-of-some-name`里面。`cd`进入这个文件夹，然后键入`pwd`获得完整路径。复制它-你需要马上粘贴它。它应该看起来像`my-computer/my-projects/project-of-some-name`

7)再次打开您的终端，然后输入`~/scripts/`<<the-correct-filename.sh>>`` 。脚本运行！您将被引导完成一些输入...主要步骤有:
>你想怎么称呼你的 Github repo ( **不要用空格-**‘我的-牛逼的-项目’就好。不要使用“我的了不起的项目”作为回购名称。

输入显示在 Github repo 描述中的描述。为此，使用空格是安全的。

>输入您在步骤 6 中得到的项目路径，在终端中键入`pwd`并得到类似`my-computer/my-projects/project-of-some-name`的内容后得到的路径

>输入您的 Github 用户名(不是电子邮件地址)，然后输入您的 Github 密码。输入时要小心，因为这些值不会显示在屏幕上。

> ....就是这样。该脚本将在`my-computer/my-projects/project-of-some-name`中本地设置您的 git repo，然后创建一个`README.MD`(空白)并本地提交，然后在 Github 中设置一个远程 repo(通过 API 登录)等，然后将所有内容推上来！

>最后，您将看到您正在交互的终端已经将当前活动目录更改为您的项目文件夹。现在它将在`my-computer/my-projects/project-of-some-name`处，你可以输入`ls`并看到`README.MD`文件。如果你输入`git status`，你会看到你的本地回购的状态(你的本地项目的状态)，如果你输入`git remote`，它会显示你的项目的 Github 网址！

搞定了。编码快乐！

Annnd.....最后......这是剧本！我对每一步都做了注释，所以你可以通过推理来完成它。

```
# Make executable with chmod +x <<filename.sh>>

CURRENTDIR=${pwd}

# step 1: name of the remote repo. Enter a SINGLE WORD ..or...separate with hyphens
echo "What name do you want to give your remote repo?"
read REPO_NAME

echo "Enter a repo description: "
read DESCRIPTION

# step 2:  the local project folder path
echo "what is the absolute path to your local project directory?"
read PROJECT_PATH

echo "What is your github username?"
read USERNAME

# step 3 : go to path 
cd "$PROJECT_PATH"

# step 4: initialise the repo locally, create blank README, add and commit
git init
touch README.MD
git add README.MD
git commit -m 'initial commit -setup with .sh script'

# step 5 use github API to log the user in
curl -u ${USERNAME} https://api.github.com/user/repos -d "{\"name\": \"${REPO_NAME}\", \"description\": \"${DESCRIPTION}\"}"

#  step 6 add the remote github repo to local repo and push
git remote add origin https://github.com/${USERNAME}/${REPO_NAME}.git
git push --set-upstream origin master

# step 7 change to your project's root directory.
cd "$PROJECT_PATH"

echo "Done. Go to https://github.com/$USERNAME/$REPO_NAME to see." 
echo " *** You're now in your project root. ***"
```

#### 感谢阅读！

如果你想了解更多关于我的代码之旅，请查看[免费代码营播客](http://podcast.freecodecamp.org/)的[第 53 集](http://podcast.freecodecamp.org/53-zubin-pratap-from-lawyer-to-developer)，昆西(免费代码营的创始人)和我分享了我们作为职业改变者的经验，可能对你的旅程有所帮助。你也可以在 [iTunes](https://itunes.apple.com/au/podcast/ep-53-zubin-pratap-from-lawyer-to-developer/id1313660749?i=1000431046274&mt=2) 、 [Stitcher](https://www.stitcher.com/podcast/freecodecamp-podcast/e/59201373?autoplay=true) 和 [Spotify](https://open.spotify.com/episode/4lG0RGpzriG5vXRMgza05C) 上访问播客。

在接下来的几个月里，我还将举办一些 ama 和网络研讨会。如果您对此感兴趣，请点击[此处](http://www.matchfitmastery.com/)告知我。当然，你也可以在 [@ZubinPratap](https://twitter.com/zubinpratap) 给我发微博。