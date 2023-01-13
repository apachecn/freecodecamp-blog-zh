# 拉宾-卡普算法解释了

> 原文：<https://www.freecodecamp.org/news/the-rabin-karp-algorithm-explained/>

Rabin-Karp 算法是由 Michael O. Rabin 和 Richard M. Karp 开发的字符串匹配/搜索算法。它使用 *****哈希***** 手法和 *****蛮力***** 进行对比，是一个很好的抄袭检测候选。

## 重要术语

*   *****模式***** 是要搜索的字符串。考虑模式的长度为 M 个字符。
*   *****文本***** 是要从中搜索模式的整个文本。将文本长度视为 *****N***** 个字符。

## 什么是蛮力比较？

在强力比较中，模式的每个字符与文本的每个字符进行比较，直到找到不匹配的字符。

## 拉宾-卡普算法如何工作

1.  计算*模式*的哈希值
2.  计算*文本*的前 *M* 个字符的哈希值
3.  比较两个哈希值
4.  如果不相等，计算*文本*的下 *M* 个字符的哈希值并再次比较。
5.  如果它们相等，则执行强力比较。

```
hash_p = hash value of pattern
hash_t = hash value of first M letters in body of text
do
	if (hash_p == hash_t) 
		brute force comparison of pattern and selected section of text
	hash_t= hash value of next section of text, one character over
while (end of text or brute force comparison == true)
```

## 优于简单字符串匹配算法

这种技术导致每个文本子序列仅进行一次比较，并且仅当散列值匹配时才需要强力。