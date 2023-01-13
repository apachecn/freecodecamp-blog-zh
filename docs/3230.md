# HTML 实体–HTML 空间和其他 HTML 符号以及特殊字符代码的列表

> 原文：<https://www.freecodecamp.org/news/html-entities-symbols-special-character-codes-list/>

大多数 ASCII 字符都有一个特殊的代码，您可以在 HTML 中使用它来使该字符可靠地出现。

这些 HTML 实体尤其有助于在 HTML 中手动插入空格。

这些代码中的每一个都以一个&符号开始，以一个分号结束。

您可以在 HTML 中的任何地方使用它们来可靠地创建该字符。不管用户的浏览器设置为哪种语言，它都应该呈现相同的效果。

有些符号有更容易记忆的代码。例如，欧元货币字符()是`&euro;`

在可能的情况下，我使用这些更容易记忆的代码，而不是数字代码。

## 如何使用 空白字符代码

例如，如果您想插入一个空白字符，您可以这样做:

```
<span>Superpower:&nbsp;listening</span>
```

您甚至可以在一行中插入几个，以创建临时文本填充:

```
<span>Superpower:&nbsp;&nbsp;&nbsp;listening</span>
```

## 如何在 HMTL 使用 换行符代码换行

如果您想强制换行:

```
<p>This is paragraph text and &#13; woops there is a new line.</p>
```

是的，你也可以连续使用其中几个:

```
<p>This is paragraph text and &#13;&#13;&#13; woops there are several new lines.</p>
```

## 常用 HTML 实体字符代码的完整列表

下面是一个很好的 ASCII 格式的表格，列出了最常用的符号和字符。我花了一段时间来组装这些东西，让它们看起来不错。

作为一名开发人员，当我搜索这些代码时，我经常得到基于图像的结果。这些对于有视觉障碍的人来说是不可访问的，并且使得每个人都很难复制粘贴代码。

所以如果你觉得这很有帮助，请链接到它并与你的朋友分享，这样更多的人可以从中受益。？

```
 +----------+--------+-----------------------------+
|  &code   | symbol |         description         |
+----------+--------+-----------------------------+
| &#33;    | !      | exclamation point           |
| &#34;    | "      | double quotation mark       |
| &#35;    | #      | hash symbol (octothorpe)    |
| &#36;    | $      | dollar sign                 |
| &#37;    | %      | percentate sign             |
| &#38;    | &      | ampersand                   |
| &#39;    | '      | apostrophe                  |
| &#40;    | (      | left parenthesis            |
| &#41;    | )      | right parenthesis           |
| &#42;    | *      | asterisk                    |
| &#43;    | +      | plus sign                   |
| &#44;    | ,      | comma                       |
| &#45;    | -      | hyphen                      |
| &#46;    | .      | period                      |
| &#47;    | /      | forward slash               |
| &#48;    | 0      | the number 0                |
| &#49;    | 1      | the number 1                |
| &#50;    | 2      | the number 2                |
| &#51;    | 3      | the number 3                |
| &#52;    | 4      | the number 4                |
| &#53;    | 5      | the number 5                |
| &#54;    | 6      | the number 6                |
| &#55;    | 7      | the number 7                |
| &#56;    | 8      | the number 8                |
| &#57;    | 9      | the number 9                |
| &#58;    | :      | colon                       |
| &#59;    | ;      | semicolon                   |
| &#60;    | <      | less-than symbol            |
| &#61;    | =      | equals symbol               |
| &#62;    | >      | greater-than symbol         |
| &#63;    | ?      | question mark               |
| &#64;    | @      | at symbol                   |
| &#65;    | A      | uppercase A                 |
| &#66;    | B      | uppercase B                 |
| &#67;    | C      | uppercase C                 |
| &#68;    | D      | uppercase D                 |
| &#69;    | E      | uppercase E                 |
| &#70;    | F      | uppercase F                 |
| &#71;    | G      | uppercase G                 |
| &#72;    | H      | uppercase H                 |
| &#73;    | I      | uppercase I                 |
| &#74;    | J      | uppercase J                 |
| &#75;    | K      | uppercase K                 |
| &#76;    | L      | uppercase L                 |
| &#77;    | M      | uppercase M                 |
| &#78;    | N      | uppercase N                 |
| &#79;    | O      | uppercase O                 |
| &#80;    | P      | uppercase P                 |
| &#81;    | Q      | uppercase Q                 |
| &#82;    | R      | uppercase R                 |
| &#83;    | S      | uppercase S                 |
| &#84;    | T      | uppercase T                 |
| &#85;    | U      | uppercase U                 |
| &#86;    | V      | uppercase V                 |
| &#87;    | W      | uppercase W                 |
| &#88;    | X      | uppercase X                 |
| &#89;    | Y      | uppercase Y                 |
| &#90;    | Z      | uppercase Z                 |
| &#91;    | [      | left square bracket         |
| &#92;    | \      | backslash                   |
| &#93;    | ]      | right square bracket        |
| &#94;    | ^      | caret                       |
| &#95;    | _      | underscore                  |
| &#96;    | `      | backtick                    |
| &#97;    | a      | lowercase a                 |
| &#98;    | b      | lowercase b                 |
| &#99;    | c      | lowercase c                 |
| &#100;   | d      | lowercase d                 |
| &#101;   | e      | lowercase e                 |
| &#102;   | f      | lowercase f                 |
| &#103;   | g      | lowercase g                 |
| &#104;   | h      | lowercase h                 |
| &#105;   | i      | lowercase i                 |
| &#106;   | j      | lowercase j                 |
| &#107;   | k      | lowercase k                 |
| &#108;   | l      | lowercase l                 |
| &#109;   | m      | lowercase m                 |
| &#110;   | n      | lowercase n                 |
| &#111;   | o      | lowercase o                 |
| &#112;   | p      | lowercase p                 |
| &#113;   | q      | lowercase q                 |
| &#114;   | r      | lowercase r                 |
| &#115;   | s      | lowercase s                 |
| &#116;   | t      | lowercase t                 |
| &#117;   | u      | lowercase u                 |
| &#118;   | v      | lowercase v                 |
| &#119;   | w      | lowercase w                 |
| &#120;   | x      | lowercase x                 |
| &#121;   | y      | lowercase y                 |
| &#122;   | z      | lowercase z                 |
| &#123;   | {      | left curly brace            |
| &#124;   | |      | vertical bar                |
| &#125;   | }      | right curly brace           |
| &#126;   | ~      | tilde                       |
| &larr;   | ←      | left arrow                  |
| &uarr;   | ↑      | up arrow                    |
| &rarr;   | →      | right arrow                 |
| &darr;   | ↓      | down arrow                  |
| &harr;   | ↔      | left-right arrow            |
| &lArr;   | ⇐      | left double arrow           |
| &uArr;   | ⇑      | up double arrow             |
| &rArr;   | ⇒      | right double arrow          |
| &dArr;   | ⇓      | down double arrow           |
| &hArr;   | ⇔      | left-right double arrow     |
| &lsquo;  | ‘      | left single smart quote     |
| &rsquo;  | ’      | right single smart quote    |
| &ldquo;  | “      | left double smart quote     |
| &rdquo;  | ”      | right double smart quote    |
| &#8218;  | ‚      | single low quotation mark   |
| &#8222;  | „      | double low quotation mark   |
| &ndash;  | -      | en dash                     |
| &mdash;  | –      | em dash                     |
| &nbsp;   |        | nonbreaking space           |
| &iexcl;  | ¡      | inverted exclamation mark   |
| &sect;   | §      | section sign (used in law)  |
| &brvbar; | ¦      | broken vertical bar         |
| &copy;   | ©      | copyright symbol            |
| &reg;    | ®      | registered trademark symbol |
| &#8482;  | ™      | trademark sign              |
| &cent;   | ¢      | cent sign                   |
| &pound;  | £      | Pound Sterling sign         |
| &yen;    | ¥      | Yen sign                    |
| &euro;   | €      | Euro sign                   |
| &plusmn; | ±      | plus-or-minus sign          |
| &micro;  | µ      | micro symbol (mu)           |
| &183;    | ·      | middle dot character        |
| &deg;    | °      | degree sign                 |
| &sup1;   | ¹      | superscript one             |
| &sup2;   | ²      | superscript two (squared)   |
| &sup3;   | ³      | superscript three (cubed)   |
| &para;   | ¶      | paragraph sign              |
| &middot; | ·      | middle dot                  |
| &frac14; | ¼      | one quarter fraction        |
| &frac12; | ½      | one half fraction           |
| &frac34; | ¾      | three quarters fraction     |
| &iquest; | ¿      | inverted question mark      |
| &#8224;  | †      | dagger                      |
| &#8225;  | ‡      | double dagger               |
| &#8226;  | •      | bullet                      |
| &#8230;  | …      | ellipsis (three dots)       |
+----------+--------+-----------------------------+
```