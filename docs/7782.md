# 如何在基于以太坊的 dapps 上添加礼品卡支持

> 原文：<https://www.freecodecamp.org/news/how-to-add-support-for-gift-cards-on-your-ethereum-based-dapps-6389959265ba/>

巴勃罗·鲁伊斯

# 如何在基于以太坊的 dapps 上添加礼品卡支持

![y2pSCXiQn90NVJpJLky3GCTNMA7HePjtrHkh](img/0bb31c128cc5076e146441f2bf64108a.png)

Photo by [Ben White](https://unsplash.com/photos/vJz7tkHncFk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

啊，圣诞节…一年中那个神奇的时刻，你不得不为所有你爱的人买礼物，但你不知道该送他们什么…

对于那些不知道为别人的节日、生日或其他特殊场合买什么的人来说，礼品卡是完美的选择。

让你的商店接受礼品卡提供了一个促进销售的好方法，而且实际上允许“代表”其他人在网上购物。

不知道给心爱的人买哪些 Cryptokitties？为什么不给他们一张礼品卡，让他们买自己真正想要的呢？(*当然，这不会马上生效，因为 Cryptokitties 的智能合约必须修改才能接受礼品卡。*？)

在这篇文章中，我将介绍如何发行连锁礼品卡，以及如何在您自己的智能合约/dapp 中接受它们。为此，我们将构建一个 GiftCardIssuer 智能合约，您自己的智能合约可以继承它来处理礼品卡。

### 礼品卡是如何运作的

这个智能合约背后的想法是为您自己的接收付款的合约提供必要的逻辑，以便它们可以接受带有预付余额的礼品卡，而不是“现金”

任何人都可以发行新的礼品卡，该卡仅对智能合约上的购物有效，并且仅对发行账户选择的受益人有效。

在支持商店发放礼品卡将非常简单明了。想要赠送礼品卡作为礼物的帐户只需调用智能合约上的相应功能，提供卡的 ID，选择受益人帐户，然后支付即可。

我们的 GiftCardIssuer 智能合同将根据我们预定义的参数和业务规则生成礼品卡。例如，我们可以让我们的智能合约生成的礼品卡只接受最低金额的资金，或者我们可以使它们可充值。

### 开发礼品卡发行人智能合同

您可以在我的 [Github 库](https://github.com/pabloruiz55/GiftCardIssuer)上查看完整的、完全注释的代码和示例实现。

在下面的段落中，我将介绍 GiftCardIssuer 智能合同的最重要的方面。

#### 礼品卡的结构

这是我们的智能合约发行的礼品卡的外观:

```
struct Card {        uint value;        uint issueDate;        uint validThru;        address beneficiary;        address generatedBy;        bool rechargeable;        bool transfereable;    }
```

上面的结构定义了我们将要发行的礼品卡的基本属性，并帮助我们设置和执行我们稍后为它们定义的业务规则。

#### 为礼品卡设置业务规则

我们创建的礼品卡将包含一些商业规则。这些是我定义的规则，但可以根据商店的需求添加更多规则:

```
// Card business rules variablesuint public rule_Duration = 365 days;bool public rule_Rechargeable = false;uint public rule_MinValue = 1 wei;uint public rule_MaxValue = 100 ether;bool public rule_Transfereable = true;
```

*   `**rule_Duration**`定义礼品卡的到期日期
*   `**rule_Rechargeable**`定义是否可以向卡中添加资金
*   `**rule_Transfereable**`定义卡发行后是否可以给其他人
*   `**rule_MinValue**`和`**rule_MaxValue**`定义了发卡行可以添加到卡中的最小和最大资金

商店老板可以使用功能`**setGiftCardRules()**`随时修改这些商业规则，但是请记住，修改**仅适用于新发行的礼品卡的**。已经发行的卡将保留发行时的规则。

```
function setGiftCardRules(        bool _rechargeable,        bool _transfereable,        uint _duration,        uint _minValue,        uint _maxValue        ) public {        require(msg.sender == owner);        require(duration >= 1 days);        require(_minValue > 0);        require(_maxValue >= _minValue);                rule_Rechargeable = _rechargeable;        rule_Transfereable = _transfereable;        rule_Duration = _duration;        rule_MinValue = _minValue;        rule_MaxValue = _maxValue;    }
```

#### 发行新的购物卡

用户可以通过调用**应付**函数`**issueGiftCard()**` 来发放新的购物卡，该函数有两个参数:

*   `**_cardId**`:哪一个是发卡方选择的 ***唯一的*** 32 字节字符串(他们可以创建一张 id 为“约翰，生日快乐”的卡片)
*   `**_beneficiary**`:哪个账户可以使用已发放的购物卡。

当执行该功能时，一张新的礼品卡将被发放到受益人的名下(带有之前设置的业务规则),并且资金将与功能调用一起发送。

```
function issueGiftCard(bytes32 _cardId, address _beneficiary) public payable {        require(msg.value > 0);        require(cards[_cardId].issueDate == 0);        require(msg.value >= rule_MinValue);        require(msg.value <= rule_MaxValue);                cards[_cardId].value = msg.value;        cards[_cardId].beneficiary = _beneficiary;        cards[_cardId].generatedBy = msg.sender;        cards[_cardId].issueDate = now;        cards[_cardId].validThru = now + rule_Duration;        cards[_cardId].rechargeable = rule_Rechargeable;        cards[_cardId].transfereable = rule_Transfereable;                // add value to merchant balance        balance += msg.value;                E_GiftCardIssued(_cardId, now, msg.sender, _beneficiary,msg.value);    }
```

#### 处理礼品卡

为了接受礼品卡付款，商店智能合同必须实现一个函数来调用礼品卡发行者的`**useGiftCard()**` 函数:

```
function useGiftCard(bytes32 _cardId, uint _prodPrice) public returns (bool){                // Gift card can only be used by the account it was issued to        require(msg.sender == cards[_cardId].beneficiary);                // card must exist        require(cards[_cardId].issueDate > 0);                // Card must not have expired        require(now <= cards[_cardId].validThru);                // Card should have enough funds to cover the purchase        require(cards[_cardId].value >= _prodPrice);                // remove value from card balance        cards[_cardId].value -= _prodPrice;                E_GiftCardUsed(_cardId, now, cards[_cardId].beneficiary, _prodPrice);            return (true);    }
```

上面的函数将从商店智能契约(继承自 GiftCardIssuer)中调用，如下所示:

```
function buyWithGiftCard(bytes32 _cardId) public {        // Try to buy the product with the gift card provided        require(useGiftCard(_cardId, itemPrice));                itemsBought[msg.sender]++;    }
```

`**buyWithGiftCard()**`接受一个`_cardId`并通过传递`_cardId`和`itemPrice`调用其`useGiftCard`函数。这是将要购买的产品的价格(代表我们将从礼品卡的余额中减去多少乙醚)。

`**useGiftCard()**`继续确认礼品卡是否有效，如果有效，则扣除余额并接受付款。如果礼品卡无效，或者它没有足够的资金，该函数将抛出，整个交易将失败。

#### 接受购物卡付款

下面是一个极其简单的商店智能合约的实现示例，该合约接受以太卡或礼品卡:

```
contract Store is GiftCardIssuer {        uint itemPrice = 1 ether;        mapping(address => uint) public itemsBought;        function buyWithGiftCard(bytes32 _cardId) public {        // Try to buy the product with the gift card provided        require(useGiftCard(_cardId, itemPrice));                itemsBought[msg.sender]++;    }        function buyWithEther() public payable {        require(msg.value == itemPrice);                itemsBought[msg.sender]++;    }}
```

想要发行礼品卡并接受它们而不是以太卡的智能契约只需要从 GiftCardIssuer 基础契约继承并正确调用`**require(useGiftCard(_cardId, itemPrice));**`。

#### 礼品卡充值和转账

正如礼品卡的业务规则部分所定义的，我们可以将礼品卡设置为“可充值”和/或“可转让”

**可转让礼品卡**可以由当前设置为受益人的账户赠送给另一个账户。他们所要做的就是调用下面的函数(只要礼品卡的规则允许):

```
function transferGiftCardTo(bytes32 _cardId, address _newBeneficiary) public {        require(msg.sender == cards[_cardId].beneficiary);        require(cards[_cardId].transfereable);        require(cards[_cardId].issueDate > 0);        require(_newBeneficiary != address(0));                cards[_cardId].beneficiary = _newBeneficiary;    }
```

可充值的礼品卡可以添加资金。任何人都可以调用以下函数向现有礼品卡添加资金(只要礼品卡的规则允许):

```
function addFundsToGiftCard(bytes32 _cardId) public payable {        require(cards[_cardId].rechargeable);        require(msg.value > 0);        require(cards[_cardId].issueDate > 0);        require(msg.value >= rule_MinValue);        require(msg.value <= rule_MaxValue);                cards[_cardId].value += msg.value;        cards[_cardId].validThru = now + rule_Duration; //Extend duration                // add value to merchant balance        balance += msg.value;    }
```

### 送礼快乐！

有许多新的规则和属性可以添加到礼品卡中。也有可能使通用礼品卡发行商不仅在一个商店有效，而且在发行商网络的任何商店部分有效。

我希望你喜欢读这篇文章，就像我喜欢写这篇文章一样。我目前正在从事与智能合同开发相关的咨询工作。如果你打算通过 ICO 筹集资金，或者开发基于区块链的产品，请随时与我联系。