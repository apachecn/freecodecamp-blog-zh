# 什么是同态加密？

> 原文：<https://www.freecodecamp.org/news/introduction-to-homomorphic-encryption/>

在本文中，我们将讨论同态加密，它解决的问题，以及存在的不同类型。

然后我们将使用 Python 编写代码来展示它的一些功能。

## 以下是我们将要介绍的内容:

1.  什么是同态加密？
2.  同态加密的优势
3.  同态加密的类型
4.  派利尔密码系统
5.  结论和了解更多信息的资源

## 什么是同态加密？

同态这个名称来自代数术语同态。

> 同态是同一类型的两个代数结构(如两个群、两个环或两个向量空间)之间的保结构映射(来源:维基百科 )

`Homomorphic Encryption`是一种加密形式，允许用户在不解密数据的情况下对加密数据执行二进制操作。

这种形式的加密允许信息被加密并外包给云服务/环境进行处理，而无需访问原始数据。

## 同态加密的优势

在当今世界，如果我们想对加密数据进行计算，如数学运算，我们必须首先解密它们。然后，我们必须进行计算，最后再次加密数据，以便将它们发送回去。

但是，当加密的数据非常敏感，而我们不希望其他服务访问它们时，会发生什么呢？这就是`Homomorphic encryption`发挥作用的地方。

一个更实际的例子是处理医疗信息以便诊断病人是否有疾病的系统/服务。

我们要分享的数据可能包括关于患者病史的非常敏感的信息。所以这是我们想要确保其他任何人都无法接触到的东西。

使用`Homomorphic Encryption`，系统/服务可以对加密数据进行所需的计算，返回诊断结果，而不知道正在处理哪些信息。

通过不同平台分享敏感信息会泄露我们的隐私。另一方面，能够在数据被加密时对其进行修改和执行操作确保了数据的私密性。

## 同态加密的类型

同态加密的目标如下:给定任何输入，如
`input := Enc(x1),...,Enc(xn)`，对于任何应用无限数量的加法或乘法的任意函数`f`，如`value := f(Enc(x1),...,Enc(xn))`，可以在加密输入的同时计算值。

算术运算，说到底，是在算术或布尔电路下的硬件级实现的。

我们要执行的操作是同态加法和同态乘法。加法和乘法之所以被称为加法和乘法，是因为逻辑门“异或”和“与”具有与二进制加法和二进制乘法相似的行为。这两个门的组合可以表示任何布尔函数。

这些因素使得复杂性根据操作的数量和种类而变化。

由于这些限制和构造完全同态加密算法(支持同态加法和同态乘法)的问题，随着时间的推移，已经实现了不同的方案。

最常见的同态加密类型有:

*   部分同态加密(PHE)
*   某种同态加密(SHE)
*   全同态加密(FHE)

部分同态加密(PHE)只允许对密文执行一次操作无限次。这种运算只能是加法或乘法。

部分同态加密算法更容易设计，并且在使用一种算术运算的应用中非常有用。

有些同态加密(SHE)允许执行加法和乘法，但次数有限。这种限制在电路逻辑中被评估到一定深度。这是达到全同态加密的一个非常重要的里程碑。

全同态加密(FHE)允许对密文执行无限次加法和乘法，支持对加密数据的任意计算。

与明文操作相比，全同态加密的主要问题是在速度和存储要求方面的成本效率。

## 派利尔密码系统

派利尔密码系统是帕斯卡尔·派利尔在 1999 年发明的。这是一个部分同态加密(PHE)方案和加法同态。

它只支持两个密文的相加，而不支持它们之间的相乘。此外，明文数字可以添加或乘以密文。

在这个例子中，我们使用了`python-paillier`，这是一个使用 Paillier 密码系统进行部分同态加密的 Python 库。

```
from phe import paillier

num1 = 10
num2 = 20

pub_key, priv_key = paillier.generate_paillier_keypair()
cipher_num1, cipher_num2 = pub_key.encrypt(num1), pub_key.encrypt(num2)

# add two encrypted numbers together
result = cipher_num1 + cipher_num2
result = priv_key.decrypt(result)
print("add two encrypted numbers together:",result)

# add an encrypted number to a plaintext number
result = cipher_num1 + 5
result = priv_key.decrypt(result)
print("add an encrypted number to a number:",result)

# multiply an encrypted number by a plaintext number
result = cipher_num1 * 10
result = priv_key.decrypt(result)
print("multiply an encrypted number to a number:",result) 
```

在上面的例子中，我们生成了一对公钥和私钥。接下来，我们用公钥对`num1`和`num2`进行加密，并对它们的密文执行操作。

首先，我们添加了两个密码。之后，我们将`cipher_num1`加上一个明文数字。最后，我们做了与之前相同的过程，但是这次我们用一个明文数字乘以`cipher_num1`,而不是加法。

这些操作的计算在数据加密时进行。此外，我们每次都可以通过使用私钥解密来验证结果的完整性。

## 结论

当谈到数据隐私和保护时，同态加密(HE)看起来像一个梦。但是其较差的性能和较高的成本仍然使其无法用于商业/生产应用。

但是最近在速度方面有了很多改进。以目前的速度，我相信在接下来的几年里，它将会在世界范围内被采用。

### 资源

*   [同态](https://en.wikipedia.org/wiki/Homomorphism)
*   [同态加密](https://en.wikipedia.org/wiki/Homomorphic_encryption)
*   [Paillier 密码系统](https://en.wikipedia.org/wiki/Paillier_cryptosystem)
*   [python-pairing](https://python-paillier.readthedocs.io/en/develop/)