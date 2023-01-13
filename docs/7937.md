# “tmux 实践”系列文章

> 原文：<https://www.freecodecamp.org/news/tmux-in-practice-series-of-posts-ae34f16cfab0/>

阿列克谢·萨莫什金

#### 探索 tmux 及其棘手的问题和技巧，包括特性、嵌套会话、回滚缓冲区、剪贴板集成和 iTerm2 集成。

我刚刚完成了关于 tmux 主题的文章。

到目前为止，我已经讨论了几个主题。这篇文章的目标是成为“tmux 实践”系列所有部分的索引页面。快乐阅读:

第一部分。 [Tmux 实践:探索本地和嵌套的远程 Tmux 会话](https://medium.com/@alexeysamoshkin/tmux-in-practice-local-and-nested-remote-tmux-sessions-4f7ba5db8795)。它还讨论了 tmux 的一般特性，它们与本地和远程场景的相关性，以及如何设置和配置 tmux 以支持嵌套会话。

第二部分。 [tmux 实践:iTerm2 和 tmux 集成](https://medium.com/@alexeysamoshkin/tmux-in-practice-iterm2-and-tmux-integration-7fb0991c6c01)包括在本地使用 iterm2 和 tmux 的优缺点。它展示了如何设置 iTerm2 配置文件以覆盖按键映射来触发模拟 tmux 操作。

第三部分。 [tmux 在实践中:回卷缓冲](https://medium.com/@alexeysamoshkin/tmux-in-practice-scrollback-buffer-47d5ffa71c93)。探究终端和 tmux 回滚缓冲区之间的差异。演示如何调整复制模式、滚动和鼠标选择 tmux 行为。

第四部分。 [tmux 实践:与系统剪贴板集成](https://medium.com/@alexeysamoshkin/tmux-in-practice-integration-with-system-clipboard-bcd72c62ff7b)。在 tmux 复制缓冲区和系统剪贴板之间建立一个桥梁，以解决本地和远程使用场景的方式在 OSX 或 Linux 系统剪贴板上存储选定的文本。

第五部分。 [tmux 实践:使用 SSH 远程隧道和 systemd 服务从远程会话复制文本](https://medium.com/@alexeysamoshkin/tmux-in-practice-copy-text-from-remote-session-using-ssh-remote-tunnel-and-systemd-service-dd3c51bca1fa)。另一种从远程会话复制文本到本地剪贴板的方法。

这里讨论的一切你都可以通过查看我在 Github 上的 tmux 配置项目来看到