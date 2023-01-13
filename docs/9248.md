# 隐晦的 Github 评论是什么意思？

> 原文：<https://www.freecodecamp.org/news/what-do-cryptic-github-comments-mean-9c1912bcc0a4/>

让艾丽克丝准备好

你是 Github 的新手和/或正在为开源项目做贡献吗？你看过像 LGTM，ACK，NACK 等短信吗？想知道它们是什么意思？

给你:

*   LGTM——我觉得不错
*   ACK — *确认*，即同意/接受变更
*   NACK/NAK — *否定确认*，即不同意变更和/或概念
*   RFC —征求意见，即我认为这是一个好主意，让我们讨论一下
*   WIP 工作正在进行中，尚未合并
*   阿法克/AFAICT——据我所知/据我所知
*   IIRC——如果我没记错的话
*   IANAL——“我不是律师”，*但我嗅到了许可问题*

加密领域的很多项目也使用了下面的(*被[比特币](https://github.com/bitcoin/bitcoin/issues/6100)的[黑客行话](https://twitter.com/jgarzik/status/601815506291531776)*普及的):

*   概念确认—同意概念，但尚未审查变更
*   尤塔克(又名。未测试的确认)—同意变更并检查了它们，但是没有测试
*   经过测试的确认—同意变更，经过审查和测试

这些答案通常是代码审查过程的一部分，你可以在 Github 的*问题*或*拉请求*中找到它们。

*荣誉提及: **+1*** 作为 ACK 的简称(很多情况下也是概念 ACK)。在[著名的“亲爱的 Github”信](https://github.com/dear-github/dear-github)之后，平台引入了[对 declutter 评论的适当反应](https://github.com/blog/2119-add-reactions-to-pull-requests-issues-and-comments)。不，这不是让 Github 成为你的下一个脸书:)

您还会看到 ack 被包含在提交消息中，就像由于使用了 Git，Linux 内核是如何做的一样:

```
Add get_random_long().Signed-off-by: Daniel Cashman <dcashman@android.com>Acked-by: Kees Cook <keescook@chromium.org>Cc: "Theodore Ts'o" <tytso@mit.edu>Cc: Arnd Bergmann <arnd@arndb.de>Cc: Greg Kroah-Hartman <gregkh@linuxfoundation.org>Cc: Catalin Marinas <catalin.marinas@arm.com>Cc: Will Deacon <will.deacon@arm.com>Cc: Ralf Baechle <ralf@linux-mips.org>Cc: Benjamin Herrenschmidt <benh@kernel.crashing.org>Cc: Paul Mackerras <paulus@samba.org>Cc: Michael Ellerman <mpe@ellerman.id.au>Cc: David S. Miller <davem@davemloft.net>Cc: Thomas Gleixner <tglx@linutronix.de>Cc: Ingo Molnar <mingo@redhat.com>Cc: H. Peter Anvin <hpa@zytor.com>Cc: Al Viro <viro@zeniv.linux.org.uk>Cc: Nick Kralevich <nnk@google.com>Cc: Jeff Vander Stoep <jeffv@google.com>Cc: Mark Salyzyn <salyzyn@android.com>Signed-off-by: Andrew Morton <akpm@linux-foundation.org>Signed-off-by: Linus Torvalds <torvalds@linux-foundation.org>
```

查看“[如何将您的改变融入 Linux 内核](https://www.kernel.org/doc/Documentation/SubmittingPatches)”指南，获得详细的解释。

类似的简短回答在软件工程和开源社区中被广泛使用，因为它们使交流更加有效。

你肯定在源代码中看到过下面的代码——TODO、FIXME、XXX 和 NOTE——只是想知道 *XXX* 是什么意思？

有兴趣看更多的首字母缩略词和解释，也许还有一点历史？查看行话文件。这是自 1975 年以来最权威的来源。

**花絮**:ACK/NACK 从何而来？

我会说它来自网络/接口协议，也许 TCP 的流行导致了广泛的使用。

> *视力，视力/ACK，唉，唉，唉，唉，唉，唉。*