# Clojure Hashmaps 解释:如何从 Hashmaps 中检索值和更新 Hashmaps

> 原文：<https://www.freecodecamp.org/news/clojure-hashmaps-explained-how-to-retrieve-values-from-and-update-hashmaps/>

hashmap 是将键映射到值的集合。它们在其他语言中有不同的名字——Python 称它们为字典，JavaScript 的对象本质上像 hashmaps 一样工作。

和许多集合一样，hashmap 可以通过两种方式构建。有一个构造函数:

```
;; Note that each argument is *prepended* to the hashmap, not appended.
(def a-hashmap (hash-map :a 1 :b 2 :c 3))
a-hashmap
; => {:c 3, :b 2, :a 1}
```

您也可以使用 hashmap 文字来定义它们。这样往往更简洁明了。建议在 hashmaps 中使用逗号来分隔键/值对，因为这样可以使边界更加清晰。

```
;; This hashmap is actually in the right order, unlike the one above.
(def another-hashmap {:a 1, :b 2, :c 3})
another-hashmap
; => {:a 1, :b 2, :c 3}
```

## 什么时候使用散列表？

当你想给你的变量命名时，hashmap 是很有用的。如果你曾经对自己思考过，*“如果我使用一个对象会怎么样……”在你摆脱它并意识到你正在使用 Clojure 之前，尝试使用 hashmap。*

如果您想要将两个不同的值相互关联，它们也很有用。以 ROT13 密码为例，你可以把`\A`和`\N`联系起来，把`\B`和`\M`联系起来，等等。

用大多数语言写这个会很长很无聊，但是 Clojure 有一些函数可以为你生成它，并让它变得有趣！

## **关键字和从散列表中检索值**

等一下。这是什么？`:a`？`:b`？`:c`？那些看起来很奇怪。你看，那些是关键词。它们被称为*键*，因为它们经常在散列表中被用作键。

为什么它们经常被用作钥匙？嗯，与字符串不同，关键字可以用作从 hashmap 中提取值的函数；不需要`get`或者`nth`！

```
(def string-hashmap {"a" 1, "b" 2, "c" 3})
("a" string-hashmap)
; => ClassCastException java.lang.String cannot be cast to clojure.lang.IFn

(def keyword-hashmap {:a 1, :b 2, :c 3})
(:a keyword-hashmap)
; => 1

;; You can also pass a keyword a default value in case it's not found, just like get.
(:not-in-the-hashmap keyword-hashmap "not found!")
; => "not found!"
```

## 更新散列表

您可以使用`assoc`更新 hashmap 中的值。这允许您添加新的键/值对或更改旧的键/值对。

```
(def outdated-hashmap {:a 1, :b 2, :c 3})

(def newer-hashmap (assoc outdated-hashmap :d 4))
newer-hashmap
; => {:a 1, :b 2, :c 3, :d 4}

(def newest-hashmap (assoc newer-hashmap :a 22))
newest-hashmap
; => {:a 22, :b 2, :c 3, :d 4}

;; Note that outdated-hashmap has not been mutated by any of this.
;; Assoc is pure and functional.
outdated-hashmap
; => {:a 1, :b 2, :c 3}
```

## **将其他集合转换为散列表**

转换成散列表是很棘手的。为了演示，让我们试着像使用`vec`或`seq`一样使用它。

```
(hash-map [:a 1 :b 2 :c 3])
; => IllegalArgumentException No value supplied for key: [:a 1 :b 2 :c 3]
```

`hash-map`函数认为我们正试图创建一个以`[:a 1 :b 2 :c 3]`作为键之一的散列表。如果我们给它适当数量的参数，看看会发生什么:

```
(hash-map [:a 1 :b 2 :c 3] "foo")
; => {[:a 1 :b 2 :c 3] "foo"}
```

要将一个序列转换成 hashmap，您需要使用并理解`apply`。幸运的是，这非常简单:`apply`本质上是在对集合应用函数之前先对其进行析构。

```
;; These two expressions are exactly the same.
(+ 1 2 3)
; => 6
(apply + [1 2 3])
; => 6
```

这就是将 vector 转换成 hashmap 的方式:

```
(apply hash-map [:a 1 :b 2 :c 3])
; => {:c 3, :b 2, :a 1}

;; This is the same as:
(hash-map :a 1 :b 2 :c 3)
; => {:c 3, :b 2, :a 1}
```

这应该是您在 Clojure 中开始使用 hashmaps 所需要的一切。现在出去，开始和他们中最好的人较量。