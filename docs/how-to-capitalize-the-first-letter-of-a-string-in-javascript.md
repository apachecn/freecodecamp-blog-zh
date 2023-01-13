# 如何在 JavaScript 中将字符串的首字母大写

> 原文：<https://www.freecodecamp.org/news/how-to-capitalize-the-first-letter-of-a-string-in-javascript/>

要将随机字符串的第一个字母大写，您应该按照下列步骤操作:

1.  获取字符串的第一个字母；
2.  将第一个字母转换为大写；
3.  获取字符串的剩余部分；
4.  将第一个大写字母与字符串的其余部分连接起来，并返回结果；

## **1。获取字符串的第一个字母**

您应该在*索引 0* 处使用 [charAt()](http://forum.freecodecamp.com/t/javascript-string-prototype-charat/15932) 方法来选择字符串的第一个字符。

```
var string = "freeCodecamp";

string.charAt(0); // Returns "f"
```

注意:`charAt`比使用`[ ]` ( [括号符号](http://forum.freecodecamp.com/t/javascript-string-prototype-touppercase/15950))更好，因为`str.charAt(0)`为`str = ''`返回一个空字符串( *`''`* )，而不是在`''[0]`的情况下使用`undefined`。

## **2。将首字母转换成大写**

您可以使用 [toUpperCase()](http://forum.freecodecamp.com/t/javascript-string-prototype-touppercase/15950) 方法，将调用字符串转换为大写。

```
var string = "freeCodecamp";

string.charAt(0).toUpperCase(); // Returns "F"
```

## **3。获取字符串的剩余部分**

您可以使用 [slice()](https://github.com/freecodecamp/freecodecamp/wiki/js-array-prototype-slice) 方法获取字符串的剩余部分(从第二个字符 *index 1* 到字符串的末尾)。

```
var string = "freeCodecamp";

string.slice(1); // Returns "reeCodecamp"
```

## **4。返回第一个字母和字符串剩余部分相加的结果**

您应该创建一个函数，它只接受一个字符串作为参数，并返回第一个大写字母`string.charAt(0).toUpperCase()`和字符串剩余部分`string.slice(1)`的连接。

```
var string = "freeCodecamp";

function capitalizeFirstLetter(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

capitalizeFirstLetter(string); // Returns "FreeCodecamp"
```

或者，您可以将该函数添加到`String.prototype`中，以便使用下面的代码(*直接在字符串上使用它，这样该方法就不可枚举，但可以在以后被覆盖或删除*):

```
var string = "freeCodecamp";

/* this is how methods are defined in prototype of any built-in Object */
Object.defineProperty(String.prototype, 'capitalizeFirstLetter', {
    value: function () {
        return this.charAt(0).toUpperCase() + this.slice(1);
    },
    writable: true, // so that one can overwrite it later
    configurable: true // so that it can be deleted later
});

string.capitalizeFirstLetter(); // Returns "FreeCodecamp"
```

### **来源**

[stackoverflow -将 JavaScript 中字符串的第一个字母大写](http://stackoverflow.com/questions/1026069/capitalize-the-first-letter-of-string-in-javascript/1026087#1026087)