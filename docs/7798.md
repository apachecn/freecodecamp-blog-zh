# 一种可销售智能合同协议

> 原文：<https://www.freecodecamp.org/news/a-protocol-for-sellable-smart-contracts-829bc2ce02b3/>

巴勃罗·鲁伊斯

![qSXdo1JeMJI0hV5XJqu5lkL-WCZX0e1zNArx](img/c14c05f8af7611c21c76316ddfa4a824.png)

Photo by [Jezael Melgoza](https://unsplash.com/photos/HYQvV8wWX18?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 一种可销售智能合同协议

以太坊没有内置智能契约所有权的概念。
尽管智能合同的创建和部署是由一个帐户完成的——无论是外部拥有的帐户(EOA)还是另一个合同——作为智能合同的创建者并不赋予该帐户对他们部署的合同的任何特权。

智能合约的大多数用例需要有人拥有合约。该“所有者”被赋予智能合同的特权和责任。

在众卖合同中，他们可能负责管理整个过程，并在出现问题时暂停众卖。

在彩票/Ruffle Dapp 中，他们的任务可能是执行号码抽取。

在任何持有资金的合同中，他们可能被设定为建造物破坏的受益人。

![BXe8PzvTK9LjO7-H1962DwtjrlaAXfNmD8kC](img/236ad9a302acffaa8965dd7f1b338ad4.png)

Photo by [Ricardo Resende](https://unsplash.com/photos/OvitgXQeN0Q?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

许多智能合约使用的常见模式是将所有者设置为部署合约的帐户，如下所示:

```
pragma solidity 0.4.19;
```

```
contract MyContract {  address owner;
```

```
 function MyContract(){    owner = msg.sender;  }}
```

然后，添加一个修饰符:

```
modifier onlyOwner {  require(msg.sender == owner);  _;    }
```

最后，使用该修饰符强制规定关键操作只能由合同所有者执行:

```
// Suicide the contract and transfer funds to the owner// Only available to the owner, for obvious reasons.
```

```
function destroyContract() public onlyOwner {  selfdestruct(owner);}
```

### 改变合同所有权的问题是

在某些情况下，需要将合同的所有权交给其他人。仅举几个例子:

*   部署合同的人是代表其他人做的，比如为公司做合同工作的开发人员或顾问
*   一家公司想要清算/出售其资产，其中包括智能合同
    ,这些合同可能有也可能没有余额
*   智能合约的所有者想要把它送人、捐赠出去，或者只是转手获利

![NFbHtRcIE62fv8DdiE144boi720LyXmFMp7I](img/969c16615a4bb79a759d540b0741b6dc.png)

Photo by [rawpixel.com](https://unsplash.com/photos/5x8kipLwVug?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

一些合同，但不幸的是不是很多，包括一个功能，把合同的所有权给一些其他帐户。其中一些还包括另一个功能，让那个人接受赋予他的所有权。

```
function changeOwner(address _newOwner)public onlyOwner {  ownerCandidate = _newOwner;}
```

```
function acceptOwnership()public {  require(msg.sender == ownerCandidate);    owner = ownerCandidate;}
```

现在，上面提到的情况有一些共同的问题，这些不是很广泛使用的`changeOwner()`和`acceptOwnership()`函数没有解决:

*   合同的买方如何确定一旦他们支付了合同的所有权，卖方就会实际执行相应的`changeOwner()`函数？
*   这可能反过来发生。合同的卖方如何确定，如果他们先放弃所有权，他们会得到报酬？
*   合同的买方如何确定合同的当前所有者在放弃所有权之前不会修改它(嗯，是数据)？

### 可销售合同协议

我提出的解决方案是实现一系列功能，允许智能合约的所有者将它出售给自己选择的人，以换取乙醚，或者只是将它拿出来出售，让任何人以先到先得的原则按要价购买。这可以扩展到允许使用不同拍卖风格的不同销售方法。

协议的细节可以在相应的 EIP 上[阅读和讨论。](https://github.com/ethereum/EIPs/issues/798)

在接下来的段落中，我将介绍一个实现示例，它可以在我的 [Github 库中找到。](https://github.com/pabloruiz55/Saleable)

#### 处理所有权

处理合同的所有权是非常基本的。按照通常的做法，我们在初始化时将已部署契约的所有者设置为`msg.sender`:

```
function Sellable() public {        owner = msg.sender;        Transfer(now,address(0),owner,0);    }
```

然后，我们添加`onlyOwner`修饰符，它将用于我们希望仅由当前拥有合同的人执行的每个函数:

```
modifier onlyOwner {        require(msg.sender == owner);        _;    }
```

我们想要我们的合同做的是允许`owner`在特定条件下被改变。

#### 出售合同

契约的所有者可以通过调用以下函数来出售它:

```
function initiateSale(uint _price, address _to) onlyOwner public {        require(_to != address(this) && _to != owner);        require(!selling);                selling = true;                // Set the target buyer, if specified.        sellingTo = _to;                askingPrice = _price;    }
```

`initiateSale()`需要两个参数:

*   **uint _price** :这是所有者想要出售合同的价格。
*   **address _to:** 可选，对应业主希望将合同卖给谁。

当出售合同时，业主有两种选择:他们可以选择买方，在这种情况下，销售是预先安排好的。或者他们可以简单地“宣布”合同待售，第一个认领(并支付价格)的人得到它。

另外，要价可以设置为 0。这意味着合同的所有者被允许赠与、捐赠或赠送合同。

还有一件更重要的事情需要注意:有一个`ifNotLocked`修饰符可以添加到合同的函数中，以防止合同在销售过程中被执行。如果使用得当，这可以防止合同数据在购买前被修改。

最后，还有`cancelPurchase()`功能，允许所有者在有人完成之前取消销售。

```
function cancelSale() onlyOwner public {        require(selling);                // Reset sale variables        resetSale();    }
```

#### 购买合同

一旦合同待售，买方(如果指定了)或任何人(如果没有指定特定的买方)只需调用以下函数即可完成销售:

```
function completeSale() public payable {        require(selling);        require(msg.sender != owner);        require(msg.sender == sellingTo || sellingTo == address(0));        require(msg.value == askingPrice);                // Swap ownership        address prevOwner = owner;        address newOwner = msg.sender;        uint salePrice = askingPrice;                owner = newOwner;                // Transaction cleanup        resetSale();                prevOwner.transfer(salePrice);                Transfer(now,prevOwner,newOwner,salePrice);    }
```

`completeSale()`功能是需要发送乙醚的`payable`功能。发送的金额必须与所有者设定的要价完全相同。

当`completeSale()`被执行时，乙醚将被转让给业主，然后所有权将被转让给买方。这就完成了交易，并为新的所有者清理了合同，他现在可以正常使用它，甚至可以再次出售它。

#### 一个示例用例

这里有一个非常简单的例子来说明如何使用这个基础契约:

```
contract Kitty is Sellable {        string public name;    uint public kittyValue = 0;        function Kitty(string _name) public {        name = _name;    }        function findNewOwner() public onlyOwner {        kittyValue = kittyValue + 1 ether;           super.initiateSale(kittyValue,address(0));    }        function renameKitty(string newName) ifNotLocked public onlyOwner {        name = newName;    }        function buyKitty() public payable {        require(msg.value == kittyValue);        super.completeSale();    }}
```

我们有一份代表一只隐猫的合同？。主人可以把它拿出来出售。小猫每被买一次，它的价值就增加 1 英镑。这只小猫的主人可以通过在 r `enameKitty` 中实现 i `fNotLocked` 修饰符来改变它的名字，只要它目前没有被出售。

#### **就是这样！**

如果你有进一步的建议来改进这个可销售的协议，请在我创建的 EIP 中添加你的评论、错误或建议[。](https://github.com/ethereum/EIPs/issues/798)

![4T98AkZfQPTp94yyrhpXDLhgsJ76RIWbt1FH](img/4143f0b4a6e029602e4f5f53838ebafe.png)

Photo by [Jonas Vincent](https://unsplash.com/photos/xulIYVIbYIc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)