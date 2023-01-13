# Redis 散列表扫描功能如何工作

> 原文：<https://www.freecodecamp.org/news/redis-hash-table-scan-explained-537cc8bb9f52/>

作为一名软件开发人员，我面临的最大挑战之一是阅读他人的代码。在这篇文章中，我读到了一段我以前不知道的有趣的 C 代码，我将把它呈现给你。我要讲的代码是 [**Redis**](https://en.wikipedia.org/wiki/Redis) 数据库的一部分，在这里可以找到[。](https://github.com/antirez/redis/blob/e504583b7806d946da9c3627784d551a742be4d0/src/dict.c#L838)

Redis 是一个键值数据库。数据库中的每个条目都是从键到值的映射。值可以有几种类型。有整数，列表，哈希表等等。在后台，数据库本身也是一个哈希表。在这篇文章中，我们将探索 Redis 中的 SCAN 命令。

![1*eUYhCqLYo_cQpMSQvelQOw](img/dfed9ab427f46ccbdd8c6ebb0b21c268.png)

By © User:Colin / Wikimedia Commons, CC BY-SA 4.0, [https://commons.wikimedia.org/w/index.php?curid=30343877](https://commons.wikimedia.org/w/index.php?curid=30343877)

### 重定向扫描

SCAN 是一个基于光标的迭代命令，允许客户端遍历表中的所有元素。这个基于光标的扫描器在每次调用时接受一个整数**光标**，并向**返回一批项目**和**光标值**，以便在下一次调用中进行扫描。初始游标值为 0，当 SCAN 返回 0 作为下一个游标值时，这意味着扫描已经完成，所有元素都被客户端看到了。

SCAN 命令有几个有趣的属性:

1.  它保证表中出现的所有项目将至少返回*一次*。
2.  就是**无状态**。该表不保存任何有关其活动扫描仪的数据。这也意味着扫描不会锁定数据库。
3.  它对表格大小调整具有弹性。为了维持 O(1)的访问时间，哈希表维持一定的[负载因子](https://en.wikipedia.org/wiki/Hash_table#Key_statistics)。负载系数衡量在给定时间表有多“满”。当负载系数变得过大或过小时，会调整表格的大小。即使在调整表大小时调用 SCAN，它也将保持其保证。

### 履行

扫描在 dict.c 中的函数`dictScan()`中实现。这是该函数的签名和附加内务处理:

```
unsigned long dictScan(dict *d,
                       unsigned long v,
                       dictScanFunction *fn,
                       dictScanBucketFunction* bucketfn,
                       void *privdata)
{
    dictht *t0, *t1;
    const dictEntry *de, *next;
    unsigned long m0, m1;

    if (dictSize(d) == 0) return 0;
    // ...
```

没有价值的东西:

*   该函数接受 5 个参数:`dict *d`、要扫描的字典、`unsigned long v`、光标和其他 3 个参数，我们将在后面讨论。
*   该函数返回下一次调用该函数时使用的光标值。如果函数返回 0，意味着扫描完成。
*   `if (dictSize(d) == 0) return 0;`。当字典为空时，该函数返回 0，表示扫描完成。

#### 1.正常扫描

以下代码扫描一组元素:

```
if (!dictIsRehashing(d)) {
    t0 = &(d->ht[0]);
    m0 = t0->sizemask;

    /* Emit entries at cursor */
    if (bucketfn) bucketfn(privdata, &t0->table[v & m0]);
    de = t0->table[v & m0];
    while (de) {
        next = de->next;
        fn(privdata, de);
        de = next;
    }

    /* Set unmasked bits so incrementing the reversed cursor
     * operates on the masked bits */
    v |= ~m0;

    /* Increment the reverse cursor */
    v = rev(v);
    v++;
    v = rev(v);

} else {
    // ...
```

让我们一步一步地检查它。让我们从下面的第一行开始:

```
if (!dictIsRehashing(d)) {
    t0 = &(d->ht[0]);
    m0 = t0->sizemask;
```

重新散列是在调整大小后将元素均匀分布在整个表格中的过程。dict.c 哈希表递增地重散列*，这意味着它不会一次重散列整个表，而是一点一点地。对表进行的每个操作，如添加、删除、查找，也会执行一个重散列步骤。这允许在重新散列期间保持表可用于操作。由于实现再散列的方式不同，该函数在再散列过程中的工作方式也不同。我们首先来看看当表没有被重散列时会发生什么。*

*指向哈希表的指针保存在局部变量`t0`中，其**大小掩码**保存在`m0`中。**大小掩码:** dict.c 哈希表的大小总是`2^n`。对于给定的表大小，大小掩码是`2^n-1`，它是一个二进制数，其`n`最低有效位设置为 1。比如对于`n=4; 2^n-1 = 00001111`。对于给定的键，它在哈希表中的位置将是键的**哈希**的最后`n`位。一会儿我们会看到它的实际应用。*

*dict.c 哈希表使用[开放寻址](https://en.wikipedia.org/wiki/Open_addressing)。表中的每个条目都是具有冲突哈希值的元素的链表。这叫做**斗**。在下一部分中，扫描元素的**桶**:*

```
*`/* Emit entries at cursor */
if (bucketfn) bucketfn(privdata, &t0->table[v & m0]);
de = t0->table[v & m0];
while (de) {
    next = de->next;
    fn(privdata, de);
    de = next;
}`*
```

*注意使用**尺寸的遮罩** : `t0->table[v &` m0 `]`。v 可能在表的可索引范围之外`e. v &` m0 使用大小掩码只保留 la `s` t n 位数字`o` f v，并生成表中的有效索引。*

*你现在可能已经猜到`bucketfn`是干什么的了。`bucketfn`由调用者提供，并应用于每个元素桶。它也被传递给`privdata`，这是调用者传递给`dictScan()`的任意数据。以类似的方式，`fn`被逐个应用于桶中的所有条目。注意，桶可能是空的，在这种情况下，它的值是`NULL`。*

*好，我们迭代了一桶元素。下一步是什么？我们将在下一次调用`dictScan()`时返回光标值。这是通过增加当前光标`v`来实现的，但是有点扭曲！光标先反向，然后递增，再反向:*

```
 *`/* Set unmasked bits so incrementing the reversed cursor
     * operates on the masked bits */
    v |= ~m0;
    /* Increment the reverse cursor */
    v = rev(v);
    v++;
    v = rev(v);`*
```

*首先，`v |= ~m0`将`v`中所有未屏蔽的位设置为 1。其效果是，当反转`v`并递增时，这些位将被有效忽略。然后，`v`反转，递增，再反转。让我们看一个例子:*

```
*`Table size = 16 (n = 4, m0 = 16-1 = 00001111)
v = 00001000 (Current cursor)
v |= ~m0;    // v == 11111000  (~m0 = 11110000)
v = rev(v);  // v == 00011111
v++;         // v == 00100000
v = rev(v);  // v == 00000100`*
```

*在这个位魔术之后，`v`被返回。*

***为什么光标在递增之前是反向的？**表格可能会在迭代之间增长。这保证了光标保持有效。当表格增长时，从左边开始，额外的位被添加到其大小掩码**中。通过增加反转的数字，我们可以将较小的表的索引扩展到较大的表中。***

*例如:假设旧表格的大小为 16(大小掩码`00001111`)，光标为`00001000`。当表格增长到 32 个元素时，它的大小掩码将是`00011111`。先前在`00001000`槽中的所有元素将映射到新表中的`00001000`或`00011000`。这些游标与较小和较大的表都兼容！*

#### *2.表重新散列期间扫描*

*我们需要理解的最后一部分是，当表被重新散列时，扫描是如何工作的。**增量重散列**在 dict.c 中通过同时拥有两个活动表来实现。调整哈希表大小时，会创建第二个表。新项目被添加到新表中。在每个重新散列步骤中，旧表中的元素被移动到新表中。当旧表变空时，它将被删除。*

*执行扫描时，从较小的****表**开始，扫描新旧表中的元素。扫描较小表格中的项目后，扫描较大表格中的*补充项目*。因此，光标`v`覆盖的所有元素都被扫描。让我们看看这是什么样子。这是完整的代码片段，我们将把它分解如下:***

```
 ***`} else {  // dictIsRehashing(d)
        t0 = &d->ht[0];
        t1 = &d->ht[1];

        /* Make sure t0 is the smaller and t1 is the bigger table */
        if (t0->size > t1->size) {
            t0 = &d->ht[1];
            t1 = &d->ht[0];
        }

        m0 = t0->sizemask;
        m1 = t1->sizemask;

        /* Emit entries at cursor */
        if (bucketfn) bucketfn(privdata, &t0->table[v & m0]);
        de = t0->table[v & m0];
        while (de) {
            next = de->next;
            fn(privdata, de);
            de = next;
        }

        /* Iterate over indices in larger table that are the expansion
         * of the index pointed to by the cursor in the smaller table */
        do {
            /* Emit entries at cursor */
            if (bucketfn) bucketfn(privdata, &t1->table[v & m1]);
            de = t1->table[v & m1];
            while (de) {
                next = de->next;
                fn(privdata, de);
                de = next;
            }

            /* Increment the reverse cursor not covered by the smaller mask.*/
            v |= ~m1;
            v = rev(v);
            v++;
            v = rev(v);

            /* Continue while bits covered by mask difference is non-zero */
        } while (v & (m0 ^ m1));
    }`***
```

***首先，`t0`和`t1`分别用于存储较小的和较大的表格，`m0`和`m1`分别用于存储大小掩码。然后扫描较小的表，就像我们之前看到的那样。***

***接下来，使用更大尺寸的掩码`m1` : `de = t1->table[v & m1]`，将光标用于索引到更大的表中。在内部循环中，游标递增以覆盖较小表的索引的所有扩展。***

***例如，如果在较小的表中桶的索引是`0100`，而较大的表是两倍大，那么在这个循环中覆盖的索引将是`00100`和`10100`。do-while 的条件阻止将光标增加到小表的桶所覆盖的范围之外:`while (v & (m0 ^ m1));`。这最后一点我就留给你理解了:)***

***就是这样！我们已经讨论了整个哈希表扫描函数。唯一缺少的部分是`rev(v)`的实现，它是一个通用函数，用于反转数字中的位。dict.c 中使用的实现特别有趣，因为它实现了 O(log n)运行时间。我可能会在以后的文章中介绍它。***

***感谢阅读！非常感谢 [Dvir Volk](https://www.freecodecamp.org/news/redis-hash-table-scan-explained-537cc8bb9f52/undefined) 的鼓励和支持！感谢 [Jason Li](https://www.freecodecamp.org/news/redis-hash-table-scan-explained-537cc8bb9f52/undefined) 的反馈，帮助我纠正了帖子中的一个错误。***