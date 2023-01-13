# HTML URL 编码字符参考

> 原文：<https://www.freecodecamp.org/news/url-encoded-characters-reference/>

URL 是网站的地址。就像邮政地址必须遵循特定的格式才能被邮递员理解一样，URL 也必须遵循一种格式才能被理解并把你带到正确的位置。

URL 字符串中只允许使用某些字符、字母字符、数字和少数具有特殊含义的字符`; , / ? : @ & = + $ - _ . ! ~ * ' ( ) #`。

## 保留字符

![Screen-Shot-2020-03-25-at-1.55.13-PM](img/b82421f5d107d5508d62dd272723bda9.png)

## 编码

任何不是字母字符、数字或保留字符的字符都需要编码。

URL 使用 ASCII(“美国信息交换标准码”)字符集，因此编码必须是有效的 ASCII 格式。

大多数 web 语言中都有函数来为你做这种编码，例如在 JavaScript `encodeURI()`和 PHP `rawurlencode()`中。

![Screen-Shot-2020-03-25-at-1.57.33-PM](img/3d4eb6528dbf7adcc337a96caaa1ba08.png)![Screen-Shot-2020-03-25-at-1.57.53-PM](img/1c51ba68c795f242f3404f227cf0a798.png)![Screen-Shot-2020-03-25-at-1.58.06-PM](img/604c817d28b02b527f8a0e8762a0f458.png)![Screen-Shot-2020-03-25-at-1.58.18-PM](img/57ca68ef4ab6f5915485f7ed24ef5b58.png)![Screen-Shot-2020-03-25-at-1.58.32-PM](img/eb0d720256c9e194ee3239b21d65efda.png)![Screen-Shot-2020-03-25-at-1.58.43-PM](img/b162433aa18934040b626d64699ae507.png)![Screen-Shot-2020-03-25-at-1.58.57-PM](img/e16c834e5abe990d97ff6c7f7bd0428d.png)![Screen-Shot-2020-03-25-at-1.59.07-PM](img/f9a66100ac301baf2d74e3dcdfe3bc5c.png)![Screen-Shot-2020-03-25-at-1.59.18-PM](img/27b58168fc143a64afd2d87079a50ac4.png)![Screen-Shot-2020-03-25-at-1.59.27-PM](img/dbf7462f7a39a38ec243d62db52bdcb1.png)![Screen-Shot-2020-03-25-at-1.59.46-PM](img/2fc3d33a5cfded2fc80715642d6aa24b.png)![Screen-Shot-2020-03-25-at-1.59.55-PM](img/ac51985d2ff28aaea21a55322e654a79.png)

### 示例:

```
encodeURI(Free Code Camp);
// Free%20Code%20Camp
```