# Node.js 缓冲区说明

> 原文：<https://www.freecodecamp.org/news/node-js-buffer-explained/>

## 什么是缓冲区？

二进制简单来说就是`1`和`0`的集合或集合。二进制中的每个数，一个集合中的每个 1 和 0 被称为一个*位*。计算机将数据转换成这种二进制格式来存储和执行操作。例如，以下是五种不同的二进制文件:

`10, 01, 001, 1110, 00101011`

JavaScript 的核心 API 中没有字节类型的数据。为了处理二进制数据，Node.js 包含一个二进制缓冲实现，带有一个名为`Buffer`的全局模块。

### **创建缓冲区**

在 Node.js 中创建缓冲区有不同的方法。可以通过创建一个大小为 10 字节的空缓冲区。

```
const buf1 = Buffer.alloc(10);
```

从 UTF 8 编码的字符串，创作是这样的:

```
const buf2 = Buffer.from('Hello World!');
```

创建缓冲区时有不同的可接受编码:

*   美国信息交换标准码
*   utf-8
*   base64:
*   拉丁语 1
*   二进制的
*   十六进制

在 Buffer API 中分配了三个独立的函数来使用和创建新的缓冲区。在上面的例子中，我们已经看到了`alloc()`和`from()`。第三个是`allocUnsafe()`。

```
const buf3 = Buffer.allocUnsafe(10);
```

返回时，此函数可能包含需要覆盖的旧数据。

### **与缓冲液的相互作用**

使用缓冲 API 可以进行不同的交互。我们将在这里介绍其中的大部分。让我们从将缓冲区转换成 JSON 开始。

```
let bufferOne = Buffer.from('This is a buffer example.');
console.log(bufferOne);

// Output: <Buffer 54 68 69 73 20 69 73 20 61 20 62 75 66 66 65 72 20 65 78 61 6d 70 6c 65 2e>

let json = JSON.stringify(bufferOne);
console.log(json);

// Output: {"type": "Buffer", "data": [84,104,105,115,32,105,115,32,97,32,98,117,102,102,101,114,32,101,120,97,109,112,108,101,46]}
```

JSON 指定被转换的对象类型是缓冲区及其数据。将一个空缓冲区转换成 JSON 会告诉我们，它只包含零。

```
const emptyBuf = Buffer.alloc(10);

emptyBuf.toJSON();

// Output: { "type": "Buffer", "data": [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ] }
```

请注意，Buffer API 还提供了一个直接函数`toJSON()`来将缓冲区转换成 JSON 对象。要检查一个缓冲区的大小，我们可以使用`length`方法。

```
emptyBuf.length;
// Output: 10
```

现在让我们将 buffer 转换成可读的字符串，在我们的例子中，是 utf-8 编码的。

```
console.log(bufferOne.toString('utf8'));

// Output: This is a buffer example.
```

默认情况下，将缓冲区转换为 utf-8 格式的字符串。这就是你如何解码一个缓冲区。如果指定一种编码，可以将缓冲区转换为另一种编码

```
console.log(bufferOne.toString('base64'));
```

## 关于缓冲区的更多信息:

*   [需要更好地了解 Node.js 中的缓冲区？看看这个。](https://www.freecodecamp.org/news/do-you-want-a-better-understanding-of-buffer-in-node-js-check-this-out-2e29de2968e8/)