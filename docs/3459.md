# MD5 vs SHA-1 vs SHA-2——哪种加密哈希最安全，如何检查它们

> 原文：<https://www.freecodecamp.org/news/md5-vs-sha-1-vs-sha-2-which-is-the-most-secure-encryption-hash-and-how-to-check-them/>

# 什么是哈希函数？

散列函数接受一个输入值(例如，一个字符串)并返回一个固定长度的值。一个**理想的**哈希函数具有以下特性:

*   它[非常快](https://en.wikipedia.org/wiki/Hash_function#Efficiency)
*   它可以返回大量的哈希值
*   它为每个唯一的输入生成一个唯一的散列(无冲突)
*   它为相似的输入值生成不同的哈希值
*   生成的哈希值在其[分布](https://en.wikipedia.org/wiki/Hash_function#Uniformity)中没有可辨别的模式

当然，不存在理想的散列函数，但是每一个都旨在尽可能接近理想的运行。假设(大多数)散列函数返回固定长度的值，并且值的范围因此受到限制，则该限制实际上可以忽略。例如，一个 256 位的散列函数可能返回的值的数量大约与宇宙中原子的数量相同。

理想情况下，哈希函数实际上不会返回*冲突–*也就是说，不会有两个不同的输入生成相同的哈希值。这对于[加密哈希函数](https://en.wikipedia.org/wiki/Cryptographic_hash_function)尤其重要:哈希冲突被认为是一个[漏洞](https://en.wikipedia.org/wiki/Collision_resistance)。

最后，散列函数应该为任何输入值生成不可预测的不同散列值。比如，拿下面两个非常相似的句子来说:

```
1\. "The quick brown fox."
2\. "The quick brown fax." 
```

我们可以比较从两个句子中的每一个生成的 MD5 哈希值:

```
1\. 2e87284d245c2aae1c74fa4c50a74c77
2\. c17b6e9b160cda0cf583e89ec7b7fc22 
```

为两个相似的句子生成了两个非常不同的散列，这是一个对验证和加密都有用的属性。这是[分布](https://en.wikipedia.org/wiki/Hash_function#Uniformity)的一个推论:所有输入的哈希值应该均匀地、不可预测地分布在可能的哈希值的整个范围内。

# 常见哈希函数

有几种广泛使用的散列函数。所有这些都是由数学家和计算机科学家设计的。在进一步研究的过程中，一些已经被证明有弱点，尽管所有的都被认为对于非加密应用是足够好的。

## 讯息摘要 5

MD5 散列函数产生 128 位散列值。它是为加密而设计的，但是随着时间的推移，漏洞被发现了，所以不再推荐它用于加密目的。但是，它仍然用于数据库分区和计算校验和来验证文件传输。

## SHA-1

SHA 代表安全散列算法。该算法的第一个版本是 SHA-1，后来是 SHA-2(见下文)。

MD5 产生 128 位的散列，而 SHA1 产生 160 位的散列(20 字节)。在十六进制格式中，它是一个 40 位数的整数。像 MD5 一样，它是为密码学应用而设计的，但很快发现它也有漏洞。时至今日，人们不再认为它的抗攻击能力比 MD5 差。

## SHA-2

第二个版本的沙，称为 SHA-2，有许多变种。最常用的可能是 SHA-256，美国国家标准与技术研究所(NIST)推荐使用它来代替 MD5 或 SHA-1。

SHA-256 算法返回 256 位的哈希值，即 64 个十六进制数字。虽然不太完美，但目前的研究表明，它比 MD5 或 SHA-1 安全得多。

就性能而言，阿沙-256 哈希算法比 MD5 或 SHA-1 哈希算法慢 20-30%。

## 沙-3

这种哈希方法是在 2015 年末开发的，尚未得到广泛使用。它的算法与其前任 SHA-2 使用的算法无关。

SHA3-256 算法是一种变体，与早期的 SHA-256 具有相同的适用性，但前者的计算时间比后者稍长。

# 使用哈希值进行验证

哈希函数的典型用途是执行验证检查。一种常见的用法是验证压缩的文件集合，例如。拉链还是。tar 存档文件。

给定一个归档文件及其预期的哈希值(通常称为[校验和](https://techterms.com/definition/checksum)，您可以执行自己的哈希计算来验证您收到的归档文件是完整且未损坏的。

例如，我可以使用以下管道命令为 Unix 中的 tar 文件生成 MD5 校验和:

```
tar cf - files | tee tarfile.tar | md5sum - 
```

要在 Windows 中获取文件的 MD5 哈希，请使用 [Get-FileHash](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7) PowerShell 命令:

```
Get-FileHash tarfile.tar -Algorithm MD5 
```

生成的校验和可以发布在下载站点上，在存档下载链接的旁边。一旦接收方下载了归档文件，就可以通过运行以下命令来验证它是否正确到达:

```
echo '2e87284d245c2aae1c74fa4c50a74c77 tarfile.tar' | md5sum -c 
```

其中**2e 87284d 245 C2 aae 1c 74 fa 4c 50 a 74 c 77**是发布的生成校验和。成功执行上述命令将生成如下所示的 OK 状态:

```
echo '2e87284d245c2aae1c74fa4c50a74c77 tarfile.tar' | md5sum -ctarfile.tar: OK
```