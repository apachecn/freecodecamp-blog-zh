# 那个 bug 是怎么发生的？Git 平分救援！

> 原文：<https://www.freecodecamp.org/news/how-did-that-bug-happen-git-bisect-to-the-rescue-4368105f8149/>

亚历克斯·纳达林

`git bisect`是一个非常方便的命令，可以让你[隔离哪个提交引入了一个错误](https://git-scm.com/docs/git-bisect)。你告诉它你的存储库的哪个版本是没有 bug 的，它在你当前的提交和一个似乎有 bug 的提交之间运行一个[二分搜索法](https://en.wikipedia.org/wiki/Binary_search_algorithm)，要求你在搜索的每一步确认是否有 bug。

好奇？让我们看看它的实际效果吧！

让我们首先创建一个包含一堆“假”提交的存储库:

```
/tmp ᐅ mkdir test-repo
```

```
/tmp ᐅ cd test-repo
```

```
/tmp/test-repo ᐅ git initInitialized empty Git repository in /tmp/test-repo/.git/
```

```
/tmp/test-repo (master ✔) ᐅ touch test.txt
```

```
/tmp/test-repo (master ✔) ᐅ for i in $(seq 1 100); do echo $i > test.txt && git add test.txt && git commit -m "Now: $i"; done[master (root-commit) 28ea863] Now: 1 1 file changed, 1 insertion(+) create mode 100644 test.txt[master fc57245] Now: 2 1 file changed, 1 insertion(+), 1 deletion(-)[master 81e693c] Now: 3 1 file changed, 1 insertion(+), 1 deletion(-).........[master b68f338] Now: 100 1 file changed, 1 insertion(+), 1 deletion(-)
```

假设导致我们的错误的提交是在`test.txt`文件中的数字大于 9 的地方(所以从 10 开始的提交是罪魁祸首)——我们在现实生活中如何发现它呢？

输入`git bisect` —让我们告诉 git:

*   我们想开始将*一分为二*
*   我们当前的最新提交似乎被破坏了
*   历史记录中的提交似乎没有这个错误

…让 git 帮我们完成繁重的工作:

```
/tmp/test-repo (master ✔) ᐅ git bisect start
```

```
/tmp/test-repo (master ✔) ᐅ git bisect bad # Our last commit seems to have a bug
```

```
/tmp/test-repo (master ✔) ᐅ git checkout 28ea863 # let's go back to a commit we're sure does not have the bugNote: checking out '28ea863'.
```

```
You are in 'detached HEAD' state. You can look around, make experimentalchanges and commit them, and you can discard any commits you make in thisstate without impacting any branches by performing another checkout.
```

```
If you want to create a new branch to retain commits you create, you maydo so (now or later) by using -b with the checkout command again. Example:
```

```
git checkout -b <new-branch-name>
```

```
HEAD is now at 28ea863... Now: 1
```

```
/tmp/test-repo (28ea863 ✔) ᐅ git bisect goodBisecting: 49 revisions left to test after this (roughly 6 steps)[bcba603c516783f6ad42b9410f6889e10aea0717] Now: 50
```

现在 git 将在这两次提交的中间进行检查。它要求您测试您的更改，并询问您这个提交是好是坏。让我们继续吧:

```
/tmp/test-repo (bcba603 ✔) ᐅ cat test.txt50
```

```
/tmp/test-repo (bcba603 ✔) ᐅ git bisect badBisecting: 24 revisions left to test after this (roughly 5 steps)[b276476e9f1d989f011db4fefc5b92df1685b313] Now: 25
```

```
/tmp/test-repo (b276476 ✔) ᐅ cat test.txt25
```

```
/tmp/test-repo (b276476 ✔) ᐅ git bisect badBisecting: 11 revisions left to test after this (roughly 4 steps)[ba653f4df25a0192d83c813e14ca5851653ab30f] Now: 13
```

```
/tmp/test-repo (ba653f4 ✔) ᐅ cat test.txt  13
```

```
/tmp/test-repo (ba653f4 ✔) ᐅ git bisect badBisecting: 5 revisions left to test after this (roughly 3 steps)[a77f93ed29fe3bfaac69c686ce140a4284acee68] Now: 7
```

```
/tmp/test-repo (a77f93e ✔) ᐅ cat test.txt  7
```

```
/tmp/test-repo (a77f93e ✔) ᐅ git bisect goodBisecting: 2 revisions left to test after this (roughly 2 steps)[affade823e7f0cb72a1a97052f700c31dc90cfee] Now: 10
```

```
/tmp/test-repo (affade8 ✔) ᐅ cat test.txt   10
```

```
/tmp/test-repo (affade8 ✔) ᐅ git bisect badBisecting: 0 revisions left to test after this (roughly 1 step)[11e5f969458ad51f4009e2e3ac81f38d1ede6d07] Now: 9
```

```
/tmp/test-repo (11e5f96 ✔) ᐅ cat test.txt  9
```

```
/tmp/test-repo (11e5f96 ✔) ᐅ git bisect goodaffade823e7f0cb72a1a97052f700c31dc90cfee is the first bad commitcommit affade823e7f0cb72a1a97052f700c31dc90cfeeAuthor: odino &lt;some.one@gmail.com>Date:   Sun Jun 24 23:29:02 2018 +0400
```

```
Now: 10
```

```
:100644 100644 ec635144f60048986bc560c5576355344005e6e7 f599e28b8ab0d8c9c57a486c89c4a5132dcbd3b2 M    test.txt
```

令人惊讶的是— `git bisect`发现了引入我们的 bug 的确切位置。不多不少:这只是一个惊人的技巧，可以节省你几个小时的调试时间！

*最初发表于[odino.org](https://odino.org/how-did-that-bug-happen-git-bisect-to-the-rescue/)(2018 年 6 月 24 日)。*
*你可以在[推特](https://twitter.com/_odino_)上关注我——欢迎吐槽！*？