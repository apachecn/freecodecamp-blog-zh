# 如何编码凯撒密码:基本加密介绍

> 原文：<https://www.freecodecamp.org/news/how-to-code-the-caesar-cipher-an-introduction-to-basic-encryption-3bf77b4e19f7/>

作者布兰登·梅西

凯撒密码是早期加密的一个著名实现。它会提取一个句子，然后根据字母表上的一个键重新组织它。举个例子，一个键 3 和句子，“我喜欢戴帽子。”

当这个句子用密钥 3 加密时，它变成:

L olnh wr zhdu kdwv。

这使得它很难阅读，并允许邮件不被发现地通过。

虽然这是一个非常简单的加密示例，但对于学习编码的人来说，这是一个很好的练习项目。

#### 理解密码

要实现这段代码，至少在 JAVA 中，您需要仔细考虑实际在做什么。因此，让我们来看看编码这个代码所必须采取的步骤。

第一步:识别句子中的字符。

第二步:找出该字符在字母表中的位置。

步骤 3:识别字符位置+字母表中的键。

注*如果 location + key > 26，循环返回并从 1 开始计数。

第四步:用新字代替原来的字造一个新句子。

第五步:重复，直到句子长度达到。(用于循环)。

第六步:返回结果。

#### 给密码编码

虽然这些都是很好的后续步骤，但是我们应该考虑在代码中需要做什么。

步骤 0:建立一个读入消息和密钥的函数。

大概是这样的:

```
public String Encrypt(String message, int key) {
```

```
}
```

第一步:识别句子中的字符。

为此，我们需要建立一个字母表来查看。

建立一个由 26 个字母组成的可变“字母表”。

```
String alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";String alphabet2 = alphabet.toLowerCase();
```

第二步:找出该字符在字母表中的位置。

然后创建一个贯穿消息中每个字符的 for 循环。如果我们建立一个 StringBuilder，这将更容易做到。

```
StringBuilder encrypted = new StringBuilder(message);
```

```
for (int q = 0; q < encrypted.length(); q++) {    char currchar = encrypted.charAt(q);    int index = alphabet.indexOf(currchar);}
```

此时，我们应该确定斑点是一个字母。

```
if (index != -1) {
```

```
} 
```

第三步:识别字符的位置+字母表中的键。

如果是一个字母，那么我们必须在修改后的字母表中找到那个点。我们还没有建立一个修改过的字母变量，所以我们现在应该这样做。

第四步:用新字代替原来的字造一个新句子。

一旦我们在修改后的字母表中找到了值，我们应该将它设置到我们创建的 StringBuilder 中的相同位置。

```
public String Encryption(String input, int key){        StringBuilder encrypted = new StringBuilder(input);        String alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";        String alphabet2 = alphabet.toLowerCase();        String keyedalphabet = alphabet.substring(key) + alphabet.substring(0, key);for (int q = 0; q < encrypted.length(); q++) {            char currChar = encrypted.charAt(q);            int index = alphabet.indexOf(currChar);            if (index != -1) {                char newChar = keyedalphabet.charAt(index);                encrypted.setCharAt(q, newChar);            }
```

第五步:重复，直到句子长度达到。(用于循环)

现在，我们已经检查了字符是否是大写的，但是我们还需要检查字符是否是小写的。为此，我们需要访问我们之前建立的 alphabet2。

```
index = alphabet2.indexOf(currChar);            if (index != -1) {                String keyedalphabet2 = keyedalphabet.toLowerCase();                char newChar = keyedalphabet2.charAt(index);                encrypted.setCharAt(q, newChar);            }
```

第六步:返回结果。

现在，我们已经完成了 For 循环。我们剩下的就是退出它并返回字符串。

```
public String Encryption(String input, int key){        StringBuilder encrypted = new StringBuilder(input);        String alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";        String alphabet2 = alphabet.toLowerCase();        String keyedalphabet = alphabet.substring(key) + alphabet.substring(0, key);        for (int q = 0; q < encrypted.length(); q++) {            char currChar = encrypted.charAt(q);            int index = alphabet.indexOf(currChar);            if (index != -1) {                char newChar = keyedalphabet.charAt(index);                encrypted.setCharAt(q, newChar);            }            index = alphabet2.indexOf(currChar);            if (index != -1) {                String keyedalphabet2 = keyedalphabet.toLowerCase();                char newChar = keyedalphabet2.charAt(index);                encrypted.setCharAt(q, newChar);            }        }        return encrypted    }
```

第七步:调试。

但是等等！那不行！encrypted 不是一个字符串，它是一个 StringBuilder，这个函数特别要求返回一个字符串！

幸运的是，有一个非常简单的函数可以弥补这个疏忽。

```
public String Encryption(String input, int key){        StringBuilder encrypted = new StringBuilder(input);        String alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";        String alphabet2 = alphabet.toLowerCase();        String keyedalphabet = alphabet.substring(key) + alphabet.substring(0, key);        for (int q = 0; q < encrypted.length(); q++) {            char currChar = encrypted.charAt(q);            int index = alphabet.indexOf(currChar);            if (index != -1) {                char newChar = keyedalphabet.charAt(index);                encrypted.setCharAt(q, newChar);            }            index = alphabet2.indexOf(currChar);            if (index != -1) {                String keyedalphabet2 = keyedalphabet.toLowerCase();                char newChar = keyedalphabet2.charAt(index);                encrypted.setCharAt(q, newChar);            }        }        return encrypted.toString();    }
```

这就是你如何得到原始句子的加密版本。你自己试试吧！

感谢您的阅读！