# 编码面试回溯问题速成班——你唯一需要的课程

> 原文：<https://www.freecodecamp.org/news/coding-interview-backtracking-problems-crash-course/>

无论你是编码面试的新手，还是已经熟悉了**回溯**算法的概念，这都是你的速成班。

其中，我们将了解到**一个解决回溯问题的通用编码模板**，并将其应用于**两个 LeetCode 难题**。准备好迎接下一次编码面试了吗？我们走吧！

如果你只想一头扎进去，[你可以在这里找到课程](https://www.youtube.com/watch?v=H2gnD7Ixeao)(链接在本文底部)。如果你想了解更多信息，请继续阅读。:)

## 课程是给谁的，回溯算法是什么？

这门课程适合任何准备编码面试的人，尤其是那些希望磨练解决**回溯**问题的技能的人。

回溯是编码面试中常见的一类问题。解决这些问题的算法通常包括**递归**和**在先前状态**的基础上逐步构建，以达到最终的有效解决方案。

回溯是谷歌、微软和脸书等顶级科技公司最喜欢的话题，正是因为它需要强大的推理和编码能力来解决这些问题。

然而，由于它的递归性质和复杂的问题定义，回溯问题通常是准备编码面试的开发人员困惑的主要来源。

为了解决这个困惑，这个速成课程旨在为你提供一个简洁的 20 行模板，你可以用它来解决大多数回溯问题。

## 课程大纲

本课程共 40 分钟，结构如下:

*   关于[模板](https://gist.github.com/RuolinZheng08/cdd880ee748e27ed28e0be3916f56fa6)的 8 分钟介绍
*   针对第 51 个 LeetCode 问题的 15 分钟动手编程会议。n 皇后
*   针对第 37 个问题[的 15 分钟实践代码会议。数独求解器](https://leetcode.com/problems/sudoku-solver/)

## 通用模板

为了方便你，我已经把模板复制过来了。这与我在编码面试中使用的模板完全相同，或者当我为我的独立游戏开发算法时也是如此。我甚至在研究一个非凸优化问题时用过一次。

```
def is_valid_state(state):
    # check if it is a valid solution
    return True

def get_candidates(state):
    return []

def search(state, solutions):
    if is_valid_state(state):
        solutions.append(state.copy())
        # return

    for candidate in get_candidates(state):
        state.add(candidate)
        search(state, solutions)
        state.remove(candidate)

def solve():
    solutions = []
    state = set()
    search(state, solutions)
    return solutions 
```

前三个都是帮助函数，最后一个也是最重要的函数`solve`，本质上是 LeetCode 问题要求你编写的函数。

## 动手解决 LeetCode 问题

接下来我们将应用这个模板来解决两个 LeetCode 难题: [LeetCode 问题 51。N-Queens](https://leetcode.com/problems/n-queens/) 和 [LeetCode 问题 37。数独求解器](https://leetcode.com/problems/sudoku-solver/)。

为了说明模板的灵活性，请看下面我们是如何解决 N 皇后问题的，除了修改四个函数(将`solve`重命名为`solveNQueens`)之外，我们什么也没做。这两个问题的完整代码可以在我的 GitHub gist 的[中找到。](https://gist.github.com/RuolinZheng08/cdd880ee748e27ed28e0be3916f56fa6)

观看[视频课程](https://youtu.be/H2gnD7Ixeao)，跟随我对模板的分析和改编。

```
class Solution:
    # solveNQueens is essentially the solve function
    def solveNQueens(self, n: int) -> List[List[str]]:
        solutions = []
        state = []
        self.search(state, solutions, n)
        return solutions

    def is_valid_state(self, state, n):
        # check if it is a valid solution
        return len(state) == n

    def get_candidates(self, state, n):
        if not state:
            return range(n)

        # find the next position in the state to populate
        position = len(state)
        candidates = set(range(n))
        # prune down candidates that place the queen into attacks
        for row, col in enumerate(state):
            # discard the column index if it's occupied by a queen
            candidates.discard(col)
            dist = position - row
            # discard diagonals
            candidates.discard(col + dist)
            candidates.discard(col - dist)
        return candidates

    def search(self, state, solutions, n):
        if self.is_valid_state(state, n):
            state_string = self.state_to_string(state, n)
            solutions.append(state_string)
            return

        for candidate in self.get_candidates(state, n):
            # recurse
            state.append(candidate)
            self.search(state, solutions, n)
            state.pop()
```

点击此处查看视频课程:

[https://www.youtube.com/embed/H2gnD7Ixeao?feature=oembed](https://www.youtube.com/embed/H2gnD7Ixeao?feature=oembed)

您可以在我的 GitHub gist 中访问模板以及两个 LeetCode 问题的解决方案( **N 皇后**和**数独解算器**):

[[Algo] Backtracking Template & N-Queens Solution[Algo] Backtracking Template & N-Queens Solution. GitHub Gist: instantly share code, notes, and snippets.![favicon](img/0973ea8ce7121c320f68413e2a2f23ab.png)262588213843476Gist![gist-og-image](img/b5d0f1167a86fdbbb8945644fc4c54f6.png)](https://gist.github.com/RuolinZheng08/cdd880ee748e27ed28e0be3916f56fa6)

## 最后的想法

记住熟能生巧，所以一定要试着用这个模板来解决 LeetCode 上更多的回溯问题。祝你下一次编码面试好运！

更多类似的内容，请查看我的 YouTube 频道:

[Lynn’s DevLabHi! I’m Lynn, a newbie software engineer and hobbyist game developer. Here at my channel, you can expect to enjoy monthly technical tutorials, my project demos, and my speed paints.![favicon_144x144](img/e6d5e3a3074e5674d276ea53c21f07c4.png)YouTube![nw0xMPaHUjClQJon7FSpearhe3P-4ybnt3w3DGHfC5Xu2tBRhvDRwt6OxxAlE5vZfScYA_I0=s900-c-k-c0x00ffffff-no-rj](img/7dce3eaab2a1722b710142dfe48b8cdd.png)](https://www.youtube.com/channel/UCZ2MeG5jTIqgzEMiByrIzsw)