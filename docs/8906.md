# Trie 数据结构(前缀树)

> 原文：<https://www.freecodecamp.org/news/trie-prefix-tree-algorithm-ee7ab3fe3413/>

朱莉娅·艾斯特

> `Trie`(也称为前缀树)是一种特殊类型的树，用于存储关联数据结构

trie(发音为 try)得名于 re**trie**val——它的结构使其成为一种出色的匹配算法。

### 语境

```
Write your own shuffle method to randomly shuffle characters in a string. 
```

```
Use the words text file, located at /usr/share/dict/words, and your shuffle method to create an anagram generator that only produces real words.
```

```
Given a string as a command line argument, print one of its anagrams. 
```

本周，我在 Make School 的产品学院接受了这个挑战。

文本文件中的单词由新行分隔。它的格式使得将单词放入数据结构变得容易得多。现在，我将它们存储在一个列表中——每个元素都是文件中的一个单词。

应对这一挑战的一种方法是:

*   随机打乱字符串中的字符
*   然后，对照 */usr/share/dict/words* 中的所有单词进行检查，以验证它是一个真实的单词。

然而，这种方法要求我检查新字符串中随机打乱的字符是否与该文件中的 235，887 个单词中的一个相匹配——这意味着对于我想要验证为真实单词的*每个*字符串，需要 **235，887 次操作** **。**

这是我无法接受的解决方案。我首先查找已经实现的库来检查语言中是否存在单词，并找到了 [pyenchant](https://github.com/rfk/pyenchant) 。我首先使用这个库，用几行代码完成了这个挑战。

```
def generateAnagram(string, language="en_US"):   languageDict = enchant.Dict(language)    numOfPossibleCombinationsForString = math.factorial(len(string))   for i in range(0, numOfPossibleCombinationsForString):       wordWithShuffledCharacters = shuffleCharactersOf(string)
```

```
 if languageDict.check(wordWithShuffledCharacters):            return wordWithShuffledCharacters     return "There is no anagram in %s for %s." % (language, string)
```

在我的代码中使用几个库函数是一个快速简单的解决方案。但是，找图书馆帮我解决问题，并没有学到多少东西。

我确信图书馆没有使用我前面提到的方法。我很好奇，翻遍了源代码——我找到了一个 trie。

### 特里

trie 以“步骤”的形式存储数据。每一步都是 trie 中的一个节点。

存储单词是这种树的完美用例，因为有有限数量的字母可以放在一起组成一个字符串。

语言 trie 中的每个步骤或节点将代表一个单词的一个字母。当字母的顺序与 trie 中的其他单词不同时，或者当一个单词结束时，这些步骤开始分支。

我在我的桌面上的目录中创建了一个 trie 来可视化*通过节点向下移动。这是一个包含两个单词的 trie:apple 和 app。*

*![1*vuZ47z2Ff_EyAuRi087ICQ](img/04bd3a372f6a0a44150a7608b8ca1fcd.png)

You can visualize stepping down through nodes in a trie as changing directories.* 

*请注意，单词的结尾用“$”表示。我使用' $ '是因为它是一个独特的字符，不会出现在任何语言的任何单词中。*

*如果我要将单词“aperture”添加到这个 trie 中，我将遍历单词“aperture”中的字母，同时遍历 trie 中的节点。如果该字母作为当前节点的子节点存在，则向下进入该节点。如果该字母不作为当前节点的子节点存在，则创建它，然后下一步进入它。要使用我的目录可视化这些步骤:*

*![1*zX2hBSdXJTGI0jMzYS3HfA](img/13cc51fccab2ee8ba7ba1391b69ae9b2.png)*

*在向下遍历 trie 时,“aperture”的前两个字母已经存在于 trie 中，所以我向下遍历这些节点。*

*然而，第三个字母“e”不是“p”节点的子节点。创建一个新节点来表示字母“e”，从 trie 中的其他单词中分支出来。也为后面的字母创建了新的节点。*

*为了从单词文件生成 trie，这个过程将对每个单词进行，直到每个单词的所有组合都被存储。*

*您可能会想:“等等，从这个包含 235，887 个单词的文本文件中生成 trie 树会花很长时间吗？循环遍历每个单词中的每个字符有什么意义？”*

*是的，遍历每个单词的每个字符来生成一个 trie 确实需要一些时间。然而，创建 trie 所花费的时间*是非常值得的*——因为检查一个单词是否存在于文本文件中，它最多需要与单词本身的*长度*一样多的操作。*比之前的 235，887 次操作好得多。**

*我使用嵌套字典编写了最简单的 trie。这并不是实现 trie 的最有效的方法，但是这是理解 trie 背后的逻辑的一个很好的练习。*

```
*`endOfWord = "$"`*
```

```
*`def generateTrieFromWordsArray(words):   root = {}   for word in words:      currentDict = root      for letter in word:         currentDict = currentDict.setdefault(letter, {})      currentDict[endOfWord] = endOfWord   return root`*
```

```
*`def isWordPresentInTrie(trie, word):   currentDict = trie   for letter in word:      if letter in currentDict:         currentDict = currentDict[letter]      else:          return False   return endOfWord in currentDict`*
```

*你可以在我的 Github 上看到我的字谜生成器的解决方案。自从探索了这种算法之后，我决定将这篇博文作为众多博文中的一篇——每篇博文都涵盖一种算法或数据结构。代码可以在我的[算法和数据结构](https://github.com/juliascript/Algorithms-and-Data-Structures) repo 上找到——开始更新吧！*

### *后续步骤*

*我建议看看雷·温德里希的[特里回购](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Trie)。虽然是用 Swift 写的，但它是解释各种算法的宝贵资源。*

*与 trie 相似(但更有效的内存)的是后缀树，或 radix。简而言之，不是在每个节点存储单个字符，而是存储单词的结尾，即它的后缀，并且相对地创建路径。*

*然而，基数比 trie 实现起来更复杂。如果你感兴趣的话，我建议你看看雷·温德里希的 [radix repo](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Radix%20Tree) 。*

*这是我的算法和数据结构系列的第一篇文章。在每篇文章中，我将提出一个可以用算法或数据结构更好地解决的问题，以说明算法/数据结构本身。*

*在 Github 上开始我的[算法报告](https://github.com/juliascript/Algorithms),如果你想关注的话，在[推特](https://twitter.com/JuliaGeist)上关注我！*

*你通过阅读这篇文章获得了价值吗？[点击此处](http://ctt.ec/X041V)在 Twitter 上分享！如果你想更经常地看到这样的内容，请在 Medium 上关注我，并订阅我每月一次的简讯。请随意[也给我买杯咖啡](https://buymeacoff.ee/juliageist)。*