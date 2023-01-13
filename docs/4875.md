# 如何解决 JavaScript 中的夏洛克和字谜编码挑战

> 原文：<https://www.freecodecamp.org/news/how-to-solve-the-sherlock-and-anagrams-coding-challenge-in-javascript-a80baa908637/>

这篇文章将带你了解我对一个叫做“夏洛克和字谜”的编码挑战的解决方案你可以在 [HackerRank](https://www.hackerrank.com/challenges/sherlock-and-anagrams) 里看看。

我花了很多时间试图用 JavaScript 解决这个问题。当我试图谷歌它，我不能找到一个像样的 JS 解决方案。我只找到了一个，它不能正常工作。此外，任何解释都是完全不可能的。这就是为什么我决定写一篇关于它的文章，并试着在文章中加入一些好的、容易理解的解释。现在继续读！

⚠️CAUTION: *我将在下面推出我的解决方案，并对每个步骤进行简短的解释。如果你想亲自尝试一下，请到 HackerRank 的网站。*

### 问题

如果一个字符串的字母可以重新排列形成另一个字符串，那么这两个字符串就是彼此的变位词。给定一个字符串，找出该字符串中互为变位词的子字符串对的数量。

例如 *s = mom* ，所有语法对列表分别为[[0]，[2]，[[0，1]，[1，2]]位置的[ *m，m* ，[ *mo，om* ]。

**约束**
输入字符串的长度:2 ≤ |s| ≤ 100
字符串 *s* 只包含 ascii[a-z]范围内的小写字母。

### 分析

首先，我们需要更好地了解整个问题。什么是变位词？什么是变语法对？我能看一个吗？还有，*子串*到底是什么意思？

换句话说，在解决问题之前，我们需要对我们要解决的问题有一个清晰的认识。

从问题的描述中，我们可以推导出我们所需要的一切。继续走！？

我认为这是一个很好的机会来提一下这个挑战，它在 HackerRank 网站的“字典和散列表”部分。你可能会认为在求解时应该使用这种数据结构。？

#### 字谜

既然要找字谜，就从字谜开始吧。如上所述，一个单词的变位词是另一个单词，它具有相同的长度，并由前一个单词的相同字符构成。

![yqexlCorVcBamgbm1UuU1q2ixW74Zgd50bOs](img/ad6d1a1cbd6b1271a73018e3cd78488a.png)

Animation for the anagram “Listen = Silent”

因此，我们将不得不寻找单词，并与其他单词进行比较，以查看它们是否是语法对。一旦找到，我们就数一数。

#### 语法对

既然我们已经知道了什么是变位词，那么得出一个变位词对就是两个变位词串的结论应该是相对容易的。如“莫”和“嗡”，或“听”和“默”。我们必须计算在一个给定的字符串中有多少对这样的字符串。为此，我们需要将这个原始字符串分割成子字符串。

#### 子链

顾名思义，子字符串是字符串的一部分。这些部分可能只是一个字母或一对字母，比如我们在上面的例子中看到的“ *m* 或“ *mo”。*“在我们的解决方案中，我们会将原始字符串拆分成这样的子字符串，然后我们会检查它们并进行比较，这将告诉我们它们之间是否有语法对。

### 解决办法

现在我们已经完成了我们的分析，是时候了！？

我们总结一下:

1.  我们需要找到给定字符串的所有子字符串—为此创建一个方法。
2.  我们需要能够检查两个字符串是否是字谜——为此创建一个方法。
3.  我们需要计算给定字符串中的所有语法对——为此创建一个方法。
4.  将上面的所有内容结合起来，然后分析结果——为此创建一个方法。

#### 获取所有子字符串

这将是我们查找给定字符串的所有子字符串的辅助方法:

```
function getAllSubstrings(str) {
  let i, j, result = [];

  for (i = 0; i < str.length; i++) {
    for (j = i + 1; j < str.length + 1; j++) {
      result.push(str.slice(i, j))
    }
  }
  return result
}
```

如你所见，它的时间复杂度是 O(n)。对于我们的例子，它完成了工作，因为我们输入字符串的长度有限(最多 100 个字符)。

#### 检查字谜

这将是我们检查两个字符串是否是语法对的辅助方法:

```
function isAnagram(str1, str2) {
  const hist = {}

  for (let i = 0; i < str1.length; i++) {
    const char = str1[i]
    if (hist[char]) {
      hist[char]++
    } else {
      hist[char] = 1
    }
  }

  for (let j = 0; j < str2.length; j++) {
    const char = str2[j]
    if (hist[char]) {
      hist[char]--
    } else {
      return false
    }
  }

  return true
}
```

请记住，我们假设我们最有可能不得不使用 hashmaps 或字典之类的数据结构(考虑到在 HackerRank 上发现这一挑战的部分)。

我们使用一个简单的 JavaScript 对象来扮演 hashmap 的角色。我们进行两次迭代——每个字符串一次。当我们迭代第一个时，我们将它的字符作为键添加到 hashmap 中，并计算它们的出现次数，这些将作为它们的值存储。然后我们对第二个字符串进行另一次迭代。检查它的字符是否存储在我们的 hashmap 中。如果是—减少它们的值。如果缺少字符，这意味着这两个字符串不是语法对，我们只需返回 false。如果两个循环都完成了，我们返回 true，表示被分析的字符串是一个语法对。

#### 数数吧

这是一个方法，在这个方法中，我们将使用 helper 来检查一个 pair 是否是文法上的，并对它进行计数。我们在 JavaScript 数组及其提供的方法的帮助下做到了这一点。我们遍历包含原始字符串的所有子字符串的数组。然后我们得到正确的元素并把它从数组中移除。然后我们在数组中做另一个循环，如果我们发现有当前元素的变位词，就返回 1。如果什么都没有找到，我们返回 0。

```
function countAnagrams(currentIndex, arr) {
  const currentElement = arr[currentIndex]
  const arrRest = arr.slice(currentIndex + 1)
  let counter = 0

  for (let i = 0; i < arrRest.length; i++) {
    if (currentElement.length === arrRest[i].length && isAnagram(currentElement, arrRest[i])) {
      counter++
    }
  }

 return counter
}
```

#### 最后

现在剩下唯一要做的就是综合以上所有，吐槽想要的结果。下面是最终方法的样子:

```
function sherlockAndAnagrams(s) {
  const duplicatesCount = s.split('').filter((v, i) => s.indexOf(v) !== i).length

  if (!duplicatesCount) return 0
  let anagramsCount = 0

  const arr = getAllSubstrings(s)

  for (let i = 0; i < arr.length; i++) {
    anagramsCount += countAnagrams(i, arr)
  }

  return anagramsCount
}
```

也许你已经注意到了，在这里我首先检查重复的内容，以便知道我是否应该继续。如果没有重复的字母，那么就不可能有变位词。

最后，我们将所有子字符串放入一个数组，遍历它，计算找到的语法对，并返回这个数字。

你可以在这里找到完整的代码。

### 结论

这类练习非常有助于让你用算法思考。它们也改变了你的日常工作方式。我的建议是做我正在努力做的事情——时不时地用其中的一个来训练你的大脑。如果可以的话，分享一下。我知道有时候你没有时间接受这样的挑战，但是当你有时间的时候，就去做吧。

完成后我个人的感觉是完全满意，这是完全可以理解的，考虑到我做这件事所花的时间。但最后，亲爱的读者，我更高兴能与你分享这一经历？！

感谢阅读。在 [mihail-gaberov.eu](https://mihail-gaberov.eu/sherlock-and-anagrams/) 阅读更多我的文章。