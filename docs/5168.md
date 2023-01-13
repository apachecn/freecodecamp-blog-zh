# 五个你应该停止编写的代码注释//和一个你应该开始编写的代码注释

> 原文：<https://www.freecodecamp.org/news/5-comments-you-should-stop-writing-and-1-you-should-start-4d66a367cd2c/>

用你最喜欢和最流行的开源项目的例子——React，Angular，PHP，Pandas 等等！

### 代码质量和注释之间的相关性

我们在大学里学到的第一件事就是评论是必不可少的。我们还被告知，代码质量和代码的注释数量之间存在关联——注释越多，代码越好。我们被训练成相信注释讲述了我们编写的程序的故事，并且它们表达了代码不能提供的任何东西。我们了解到，人类语言最好由人类阅读，而机器语言最好由机器阅读。

此外，仅仅教这些还不够，我们因为交作业时不加评论而被“惩罚”,我们的分数会被扣除几分。如果你设法避开了人工检查，那么你缺少的注释就会被设计来检查这一点的脚本捕捉到。

### 也许这种相关性是相反的

随着我获得更多的经验，我意识到不仅仅是相反的情况可能是正确的——好的代码和代码拥有的注释数量之间可能存在反比关系。发生这种情况有两个主要原因:

1.  太多次注释只是写烂代码的借口。程序员认为，如果他们用一些奇怪的方法编写肮脏的代码，并用 5 行注释来描述，而不是编写好的代码，那么代码是可读的，写得很好。恕我不同意——代码实际上仍然很糟糕。如果你的同事需要阅读一篇很长的评论文章来理解它，那么你就做错了。如果你的代码不是自解释的，最好改进它，不要用注释来描述它。
2.  评论会随着时间的推移而衰减，这使它们变得错误和误导。它们只有在写出来的时候才是正确的，即使这样，它们也不能被有效地执行。久而久之，人难免会做出逻辑上的改变，改变类型，把东西搬来搬去。他们有的会注意到该改的评论，有的不会。即使你找到了一种方法，当代码改变时，围绕更新注释设置了一个非常严格的规则，这也会在你第一次执行自动重构时被打破。想象一下，一个重构向一个使用了 250 多次的核心函数添加了一个参数——您真的想去手动更改所有这些注释吗？

![7YrMu1H2zUOi2iRAJC9k38eT3ARmfCC9HDhw](img/2a10e693746843630ab64d6e77c31a8f.png)

[www.imagewishes.com](http://www.imagewishes.com)

### 你应该尽量避免的最常见的评论是什么？

所有这些并不意味着你应该马上停止写评论或者不惜任何代价减少你的评论数量。我也不建议仔细检查你的代码，并试图清除所有不必要的或误导性的注释——这将花费太多的时间，你的时间最好用在其他地方。相反，我建议在你添加下一条评论之前，要更加深思熟虑，问自己以下三个问题:

1.  这个评论真的需要吗？它增加了什么价值？
2.  有没有一种方法可以改进代码，使这个注释没有必要？
3.  我加这个评论是不是只是在掩盖我的 a**呢？

为了帮助你，我列出了我在一段时间内看到的 5 个最糟糕的评论——这些类型的评论应该在你添加之前发出危险信号。我用了一些很常见的开源项目来得到一些例子。不要误解我，我不认为这些项目写得不好。相反，那些是我最喜欢的项目。但是生活中没有什么是完美的；所有代码都可以改进。

#### 1.陈述明显的事实

这些注释解释了代码的作用。你可能在附近见过这些:

来自 [react.js](https://github.com/facebook/react/blob/master/scripts/jest/noHaste.js#L5) 的一个例子:

```
getHasteName() {
   // We never want Haste.
   return null;
}
```

另一个来自 [vscode](https://github.com/Microsoft/vscode/blob/master/src/cli.js#L11) :

```
// Avoid Monkey Patches from Application Insights// Avoid Monkey Patches from Application Insights
bootstrap.avoidMonkeyPatchFromAppInsights();
// Enable portable support
bootstrap.configurePortable();
// Enable ASAR support
bootstrap.enableASARSupport();
// Load CLI through AMD loader
require('./bootstrap-amd').load('vs/code/node/cli');
```

信不信由你，阅读你代码的人本身就是程序员。他们很有可能和你在同一家公司工作，或者从事同一个项目。他们有一定的背景，而且相当聪明(希望…如果你认为你周围都是白痴，你可能会考虑更新你的 Linkedin)。他们可以阅读代码，即使没有脚注。如果你的变量、函数和类有有意义的名字，那么不要用在下一次代码修改或重构中会过时的无意义的解释来混淆它们。

声明:像许多其他人一样，我有评论盲。我忽略注释，并且很可能永远不会注意到在更改或重构代码时应该更新的注释。

回到这个例子——如果我们删除了上面代码中的所有注释，会发生什么？真的很难读吗？

#### 2.解释您的代码

如果你的代码是干净的，并且使用了正确的抽象层次，你就不需要解释它是做什么的。如果你仍然发现自己在解释代码，这可能是你多年来养成的某种习惯的结果。你可能想要考虑摆脱它，或者不得不忍受一个不能自我表达的代码

看看这段来自 [react.js](https://github.com/facebook/react/blob/master/dangerfile.js#L35) 的代码:

```
if (!existsSync('./scripts/rollup/results.json')) {
  // This indicates the build failed previously.
  // In that case, there's nothing for the Dangerfile to do.
  // Exit early to avoid leaving a redundant (and potentially      confusing) PR comment.
  process.exit(0);
}
```

如果我们像这样重构它，不是更简洁吗:

```
if (buildFailedPreviously())
  process.exit(0);
```

另一个常见的例子可以是命名；函数、变量或类。好的命名是最难做到的事情之一，但这并不意味着我们需要无条件地举起白旗，并使用注释来描述我们的代码做什么。看看这段来自 [php](https://github.com/php/php-src/blob/master/main/alloca.c#L226) 的代码:

```
struct stack_control_header
{ 
  long shgrow:32;    /* Number of times stack has grown.  */
  long shaseg:32;    /* Size of increments to stack.  */
  long shhwm:32;     /* High water mark of stack.  */
  long shsize:32;    /* Current size of stack (all segments).  */
};
```

如果您将它四处传递，然后尝试使用它，您可能不会立即理解 shgrow、shaseg 和其他字段是什么。如果我们这样写呢:

```
struct stack_control_header
{
  long num_of_time_grown:32;
  long size_of_inc:32;
  long high_water_mark:32;
  long current_size:32;
};
```

看到了吗？好多了。读者可以更好地理解每个字段做什么，而不需要跳到结构定义并阅读注释。

#### 3.长篇评论

用来描述你所做的每一个决定的长篇大论。这些注释可能会详细解释每一行:为什么你选择这样写，有什么选择，导致它的代码历史是什么。这使得流畅地阅读代码变得非常困难，并且会给读者带来更多的困惑。最终导致弊大于利。尽量保持评论的简短，尽量少用上下文。

如果你添加注释的原因是因为代码粗糙或复杂，那么通过重构使其可读——而不是添加另一个令人困惑的层。选择更好的名字，打破函数做一件事，使用抽象。无论你需要什么来使你的代码更易读，用代码来做，而不是注释。

来自 [vue.js](https://github.com/vuejs/vue/blob/dev/src/core/observer/scheduler.js#L36) 的一个例子:

```
// Async edge case #6566 requires saving the timestamp when event listeners are
// attached. However, calling performance.now() has a perf overhead especially
// if the page has thousands of event listeners. Instead, we take a timestamp
// every time the scheduler flushes and use that for all event listeners
// attached during that flush.
export let currentFlushTimestamp = 0
// Async edge case fix requires storing an event listener's attach timestamp.
let getNow: () => number = Date.now
// Determine what event timestamp the browser is using. Annoyingly, the
// timestamp can either be hi-res (relative to page load) or low-res
// (relative to UNIX epoch), so in order to compare time we have to use the
// same timestamp type when saving the flush timestamp.
if (inBrowser && getNow() > document.createEvent('Event').timeStamp) {
// if the low-res timestamp which is bigger than the event timestamp
// (which is evaluated AFTER) it means the event is using a hi-res timestamp,
// and we need to use the hi-res version for event listeners as well.
getNow = () => performance.now()
}
```

这可能需要更多的重构来将焦点从注释转移到实际的代码上。

#### 4.标题、页眉和其他“美化”

编写漂亮的代码是必不可少的，但这并不意味着你应该像装饰一本书一样装饰它。我们偶尔会创建代码块，并给它们命名，以区分不同的代码块。让我们看看这个来自 [angular.js](https://github.com/angular/angular.js/blob/master/lib/grunt/utils.js#L134) 的例子:

```
...
build: function(config, fn) {
var files = grunt.file.expand(config.src);
  // grunt.file.expand might reorder the list of files
  // when it is expanding globs, so we use prefix and suffix
  // fields to ensure that files are at the start of end of
  // the list (primarily for wrapping in an IIFE).
  if (config.prefix) {
    files = grunt.file.expand(config.prefix).concat(files); 
  }
  if (config.suffix) {
   files = files.concat(grunt.file.expand(config.suffix));
  }
  var styles = config.styles;
  var processedStyles;
  //concat
  var src = files.map(function(filepath) {
    return grunt.file.read(filepath);
  }).join(grunt.util.normalizelf('\n'));
  //process
  var processed = this.process(src, grunt.config('NG_VERSION'), config.strict);
  if (styles) {
  processedStyles = this.addStyle(processed, styles.css, styles.minify);
  processed = processedStyles.js;
  if (config.styles.generateCspCssFile) {
    grunt.file.write(removeSuffix(config.dest) + '-csp.css', CSP_CSS_HEADER + processedStyles.css);
  }
}
//write
grunt.file.write(config.dest, processed);
grunt.log.ok('File ' + config.dest + ' created.');
fn();
... 
```

如果你发现自己这样做，你的函数无疑不止做一件事。它可能太长、太明确，并且缺乏一些抽象层次。在上面的例子中，函数至少有四个部分:获取文件、连接、处理和写入。这些部分中的每一部分都有详细的实现，这使得函数很长，也很难阅读。这可以通过将每个块扩展到不同的功能来解决。

```
build: function(config, fn) {
  files = this.fetch_files(config)
  var src = this.concat(files)
  var processed = this.process(src)
  write(processed, config)
}
```

随着代码的增长,“标题”不够大胆。这就是我们发挥创造力，在评论中添加额外的“美化”的地方——星号、破折号、等号等等。看看这段来自[熊猫](https://github.com/pandas-dev/pandas/blob/master/pandas/core/algorithms.py)的代码:

```
...
# --------------- #
# dtype access    #
# --------------- #
def _ensure_data(values, dtype=None):
...
def _reconstruct_data(values, dtype, original):
...
def _get_hashtable_algo(values):
...
# --------------- #
# top-level algos #
# --------------- #
def match(to_match, values, na_sentinel=-1):
...
def unique(values):
...
def isin(comps, values):
...
# --------------- #
# select n        #
# --------------- #
class SelectN(object):
...
class SelectNSeries(SelectN):
...
class SelectNFrame(SelectN):
...
# ------------ #
# searchsorted #
# ------------ #
def searchsorted(arr, value, side="left", sorter=None):
...
# ---- #
# diff #
# ---- #
_diff_special = {
...
}
def diff(arr, n, axis=0):
...
```

该模块包括一个函数、变量和类的列表，所有这些都混合在一个包中，具有耦合的依赖关系。这可以通过一个简单的规则来避免——如果你觉得你需要标题来将函数或类集合在一起，这将是将你的代码分成更小部分的好时机。

如果你的类有来自不同类型的“组”方法——每组函数应该是它自己的一个类。如果您的文件有太多需要分组的类或函数，那么是时候将每个组分成自己的文件了。

如果我们把上面的代码分解成文件，它会更容易理解和浏览。通过这样做，我们还解耦了依赖关系，因此我们可以只导入我们需要的代码:

```
date_acces.py:
def _ensure_data(values, dtype=None)
def _reconstruct_data(values, dtype, original)
def _get_hashtable_algo(values):
top_level_algos.py
def match(to_match, values, na_sentinel=-1):
def unique(values):
def isin(comps, values):
selectn.py
class SelectN(object):
selectn_series.py
class SelectNSeries(SelectN):
selectn_frames.py
class SelectNFrame(SelectN):
search_sorted,py
def searchsorted(arr, value, side="left", sorter=None):
diff.py
_diff_special = {
...
}
def diff(arr, n, axis=0):
...
```

#### 5./* TODO: */

from [react.js](https://github.com/facebook/react/blob/master/packages/react-dom/server.browser.js#L14) :

```
// TODO: decide on the top-level export form.
// This is hacky but makes it work with both Rollup and Jest
module.exports = ReactDOMServer.default || ReactDOMServer;
```

不管是/* TODO */， [#TODO](https://paper.dropbox.com/?q=%23TODO) ，还是<！— TODO →，有一点是肯定的—永远不会有人这么做。可以，哪怕你在旁边加个名字，分配给某个人。在解决这个问题之前，受让人将离开公司很久。我从未听过任何人说类似这样的话:“嘿，伙计们，我们有一些空闲时间，为什么不在我们的代码中修正所有的待办事项呢？”(如果你有一些时间，那么你的公司有一个更大的问题，但我们将把那个留给另一篇文章)。

![5ynOP9pwo8quY5qsMsN8P5U8nlxpbDQo7xWV](img/085fd07da9710eb9190541ea9e0affe1.png)

www.xkcd.com

todos 的主要问题是，它不仅是编写糟糕代码的借口，而且读者也不清楚代码的状态——它会很快改变吗？这个问题已经解决了，作者忘记删除评论了吗？是否有应能解决此问题的等待中的拉请求？代码作者是不是把它留给我们去修复了？—做一个决定，要么修复它，要么接受后果。

一个例外是，如果您正在处理一个特性，并且希望将代码更改分成多个提交。在这种情况下，添加 todo 注释，并将您的任务编号/链接添加到您的任务管理系统中的实际任务。这样，你可以跟踪它，并确保它在你的路线图上。如果出于某种原因，你决定不处理这个任务，不要忘记也删除评论

### 最后，这里是你应该写的评论

经验法则——用评论来回答“为什么？”以及回答“如何做”的代码

即使代码是自我解释的，我们决定采用一种方法的原因并不总是清楚的，特别是如果读者没有上下文的话。这可能是由于产品需求、系统限制、效率或者只是一个你没有时间重构的糟糕代码。

用评论来强调你为什么这样做是好的，但是要简短和集中。如果你想记录，使用维基；如果你想广泛地谈论你的决策，使用文档；如果您想记录代码更改历史，这就是 git 注释的用途。

来自 [linux](https://github.com/torvalds/linux/blob/master/lib/xz/xz_dec_bcj.c#L337) 的一个很好的例子:

```
/*
Apply the selected BCJ filter. Update *pos and s->pos to match the amount of data that got filtered. NOTE: This is implemented as a switch statement to avoid using function pointers, which could be problematic in the kernel boot code, which must avoid pointers to static data (at least on x86).
*/
static void bcj_apply(struct xz_dec_bcj *s, uint8_t *buf, size_t *pos, size_t size)
```

**如果你能从这篇文章中得到什么的话——用代码来讲述你的故事和评论来扭转“WTF** ？”**到“哦……啊？**

![q1VxxU9Mei14U0INJ4pfOSgIydZPscPlRLvc](img/1aa5a542f21650891aa4eb1a49cfd1fa.png)

www.giphy.com

感谢您花几分钟时间。如果你喜欢，请随意？或者回复/*评论*/

**-阿龙**

***特别感谢:***

*   **[Rina Artstain](https://www.freecodecamp.org/news/5-comments-you-should-stop-writing-and-1-you-should-start-4d66a367cd2c/undefined) *和 [Keren](https://www.freecodecamp.org/news/5-comments-you-should-stop-writing-and-1-you-should-start-4d66a367cd2c/undefined) 进行校对，审阅本文并给出给力的技术反馈***