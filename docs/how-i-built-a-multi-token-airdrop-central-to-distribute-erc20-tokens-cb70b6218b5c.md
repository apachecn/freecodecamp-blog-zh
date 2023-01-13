# 我如何构建多令牌空投中心来分发 ERC20 令牌

> 原文：<https://www.freecodecamp.org/news/how-i-built-a-multi-token-airdrop-central-to-distribute-erc20-tokens-cb70b6218b5c/>

巴勃罗·鲁伊斯

![Y5TVJUmcR7DVKVhsM7aTL7PZnByHQxpqYLXn](img/8d68ab35e09d7d3284589d6805b7c893.png)

Image [source](https://commons.wikimedia.org/wiki/File:Supply_air_drop_outside_of_Forward_Operating_Base_Boris_110922-A-AR883-003.jpg)

# 我如何构建多令牌空投中心来分发 ERC20 令牌

时不时地，在浏览[以太坊栈交易所](https://ethereum.stackexchange.com/)上的问题时，我会看到以下问题，以太坊栈交易所是 Solidity 开发相关问题的首选网站，也是我向开发社区投稿的首选网站:

"如何空投我的代币."

在代币销售活动中，空投是指免费向多个账户发送代币。这是最近流行的一种趋势，以促进即将到来的 ICOs / Token 众筹销售。

其中一些空投是以时间和/或数量为基础的活动，人们被告知，如果他们在某个日期拥有一定数量的代币，他们将收到更多的代币。

其他一些空投甚至是以主动的方式进行的。团队将只是从一个列表中随机发送令牌到帐户。如果你在那个列表上，并且你碰巧检查了你的令牌余额，你会看到他们。

也有一些网站允许用户订阅，以了解如何心甘情愿地参与这些空投。他们通常会要求你订阅一些邮件列表，或者给你推荐链接，让你参与代币销售。

### 令牌空投通常是如何完成的

团队有几种方式来处理这些象征性空投。

*   有些是手动的。他们只是在电子表格上建立一个列表，然后手动将令牌转移到每个帐户。
*   其他人构建了一个非常简单的智能契约，它接收一个地址数组，然后向每个地址传输一定数量的令牌。
*   其他人还使用智能合同，允许人们主动撤回事先分配给他们的令牌。

我还没有看到一个解决方案，允许人们注册，然后接收多个团队发送的任何令牌。

### 令牌空投中心

在这篇文章中，我将描述我如何建立一个智能合同，作为空投的中心。基本上，人们可以订阅这个空投中心，从那时起，当一个团队执行向中心的空投时，订阅的用户可以免费提取他们在空投中的份额。

另一方面，进行空投的团队可以将代币发送到这个中心，它将被平均分配给当时订阅的所有用户。Airdrop Central 保留 2%的代币作为服务费。

重要的是要注意，这个空投中心允许任何团队放弃他们的令牌，让现有的社区退出。所有团队共享用户列表。因此，每个团队接触的人越多，从其他团队空投的代币中受益的人就越多。

顺便提一下，值得一提的是，这个解决方案并不是完全分散的，因为它依赖于所有者来审查和批准提交。此机制旨在防止与必须信任未知第三方合同(团队提交的 ERC20 令牌)相关的潜在安全问题。我不能允许任何人提交任何合同地址，这可能包含恶意代码，而不是典型的 ERC20 令牌。

Airdrop Central 尚未在任何网络上部署，因为我想先彻底测试一下。与此同时，您可以在我的 Github 库上检查代码(甚至提交任何错误或建议):

*   如果你是一个想要收到代币的用户，请将你的地址添加到这个列表中。当我将 Airdrop Central 部署到 mainnet 时，我会将您的帐户添加到其中，这样您就可以从一开始就接收令牌。一旦队伍开始空投，你将可以免费提取代币。
*   如果您所在的团队正在寻找发送代币的简单方法来促进代币销售，请按照下面的说明开始验证过程。

一旦我将 Airdrop Central 部署到 mainnet，我将接受/拒绝提交。[在 Airdrop Central 的 Github 仓库](https://github.com/pabloruiz55/AirdropCentral/issues)上打开一个标签为“提交”的问题，标题如下:**【令牌符号】—【令牌名称】—【令牌地址】—【令牌所有者地址】**。令牌应该已经部署在 mainnet 上，以便可以查看智能合同。管理员会在你创建的 Github 问题上给你留言来通知你。

#### 它是如何工作的

**对于最终用户:**用户在令牌中心注册。然后，当一个团队空投代币时，用户可以检查他们获得了多少代币(根据当时发送了多少以及有多少用户签约到中心)并收回它们。他们只需要知道代币合同的地址就可以提取代币份额。

**对于团队:**提交的材料首先必须获得批准。鉴于 Airdrop Central 合同会与未知且可能有害的第三方合同进行交互，因此在接受令牌之前，必须获得 Central 所有者或指定管理员的批准。管理员基本上必须检查提交的地址是否符合 ERC20 兼容令牌合同，并且不包含任何恶意代码。

一旦团队和令牌被批准，他们可以使用相同的帐户和令牌地址进行任意多次空投。中心的所有者保留提交的令牌的 2%作为使用服务的费用，其余的存储在合同中，可供用户提取。每次空投都有截止日期。用户在该日期之前未收回的令牌可由团队收回。

#### 使用 Airdrop Central 合同

**对于最终用户:**

1.  通过执行`signUpForAirdrops()`功能注册 Airdrop Central。这将为您订阅未来的空投。
2.  根据相应的空投是否过期以及您已经收回了多少代币，调用`getTokensAvailableToMe(address _tokenAddress)`来检查您有权获得多少代币。
3.  如果您想撤回您的代币，调用`withdrawTokens(address _tokenAddress)`，它将使用与上述相同的逻辑检查可用的代币并转移它们。
4.  您现在应该能够调用代币合约上的`balanceOf(address _owner)`来查看添加到您的余额中的代币。

**对于团队:**

1.  如上所述提交您的令牌信息。
2.  一旦提交被批准，你就可以进行空投了。首先，您需要在您的 ERC20 令牌上向 Airdrop Central 提供令牌津贴。您可以通过调用令牌上的 approve()并传递 Airdrop Central 的地址和允许的数量来实现这一点。在您的提交获得批准之前，请不要这样做。此外，只调用 approve() —不要调用 transfer()，否则您会丢失令牌。
3.  一旦您向 Airdrop Central 提供了您所拥有的令牌的限额，您就可以通过调用函数`airdropTokens(address _tokenAddress, uint _totalTokensToDistribute, uint _expirationTime)`
    来启动 Airdrop，其中:
    `address _tokenAddress`是您提交的令牌的地址。
    `uint _totalTokensToDistribute`是总代币的分发地。
    `uint _expirationTime`是空投持续的时间(秒)。
4.  *可选步骤:*一旦到期，您可以执行`returnTokensToAirdropper(address _tokenAddress)`来取回未收集的令牌。

**关于令牌分发:**

*   `_totalTokensToDistribute`是您要分发的代币总数。该函数将负责添加从令牌合约中获得的必要小数。例如:如果你想空投 100 个代币，只要输入 100，不管你的代币有多少位小数。
*   您发送的令牌将在当前注册的所有用户之间平均分配。您可以通过调用`userSignupCount()`来检查中心当前有多少注册用户，以便大致计算出您想要向每个用户分发多少令牌。
*   在**airdrop 提交后**注册的用户不会收到该提交的代币。****

### **建设空投中心**

**完整的注释代码可以在我的 Github 库中找到。
以下是对代码最重要部分的详细解释。**

#### **管理提交**

**如上所述，我们需要建立一些机制来防止任何人提交任何合同。由于 Airdrop Central 合同会与可能包含有害代码的第三方令牌合同进行交互，因此我们首先需要审查每个提交，以防止出现问题。**

**为了做到这一点，我们将接受提交链外，手动审查他们，然后，如果一切正常，批准他们。**

```
`function approveSubmission(address _airdropper, address _tokenAddress) public onlyAdmin {        require(!airdropperBlacklist[_airdropper]);        require(!tokenBlacklist[_tokenAddress]);                tokenWhitelist[_tokenAddress] = true;    }`
```

**在任何时候，如果我们检测到提交的合同或提交恶意合同的帐户有问题，我们可以撤销对相关令牌的访问，并将它们放入黑名单以防止新的提交。**这样做还会导致代币无法提取。****

**这可能会引起争议，因为它允许所有者/管理员随意冻结合同中的令牌。但另一方面，这是我们对抗恶意代码的唯一机制，这些恶意代码可能在令牌合同最初被批准时未被检测到。**

```
`function revokeSubmission(address _airdropper, address _tokenAddress) public onlyAdmin {        if(_tokenAddress != address(0)){            tokenWhitelist[_tokenAddress] = false;            tokenBlacklist[_tokenAddress] = true;        }                if(_airdropper != address(0)){            airdropperBlacklist[_airdropper] = true;        }            }`
```

**如果由于某种原因，我们将错误的令牌/帐户列入黑名单，或者他们毕竟是好公民，所有者可以将他们从黑名单中删除。这也使得冻结的代币能够被提取。**

#### **用户注册流程**

**一旦部署了 Airdrop Central 合同，用户就可以开始注册了。注册用户有两种方式:**

**他们可以通过调用以下函数自己完成:**

```
`function signUpForAirdrops() public ifNotPaused{        require(signups[msg.sender].userAddress == address(0));        signups[msg.sender] = User(msg.sender,now);        userSignupCount++;                E_Signup(msg.sender,now);    }`
```

**或者管理员可以通过打电话`signupUsersManually().`来注册。请注意，与“常规”空投相反，团队不能手动添加用户，因为我们希望避免“垃圾邮件”和未经他们同意添加用户。**

```
`function signupUsersManually(address _user) public onlyAdmin {        require(signups[_user].userAddress == address(0));        signups[_user] = User(_user,now);        userSignupCount++;                E_Signup(msg.sender,now);    }`
```

**此外，用户可以将自己从 Airdrop Central 中移除，以停止接收令牌。这样做还会阻止他们撤回待定令牌。事实上，他们将失去他们，所以他们应该三思而后行。**

#### **象征性空投**

**一旦团队的令牌获得 Airdrop 管理中心的批准，他们就可以为该令牌执行任意数量的空投。**

**首先，他们需要向 Airdrop Central contract 提供他们想要分发的令牌的限额。为此，他们需要对令牌调用 approve()函数，指定他们希望允许 Airdrop Central 使用多少令牌。**

**一旦解决了这个问题，他们就可以通过调用以下函数来执行空投:**

```
`function airdropTokens(address _tokenAddress, uint _totalTokensToDistribute, uint _expirationTime) public ifNotPaused {        require(tokenWhitelist[_tokenAddress]);        require(!airdropperBlacklist[msg.sender]);                ERC20Basic token = ERC20Basic(_tokenAddress);        require(token.balanceOf(msg.sender) >= _totalTokensToDistribute);                //Multiply number entered by token decimals.        _totalTokensToDistribute = _totalTokensToDistribute.mul(10 ** uint256(token.decimals()));                // Calculate owner's tokens and tokens to airdrop        uint tokensForOwner = _totalTokensToDistribute.mul(ownersCut).div(100);        _totalTokensToDistribute = _totalTokensToDistribute.sub(tokensForOwner);                // Store the airdrop unique id in array (token address + id)        TokenAirdropID memory taid = TokenAirdropID(_tokenAddress,airdroppedTokens[_tokenAddress].length);        TokenAirdrop memory ta = TokenAirdrop(_tokenAddress,airdroppedTokens[_tokenAddress].length,msg.sender,now,now+_expirationTime,_totalTokensToDistribute,_totalTokensToDistribute,userSignupCount);        airdroppedTokens[_tokenAddress].push(ta);        airdrops.push(taid);                // Transfer the tokens        require(token.transferFrom(msg.sender,this,_totalTokensToDistribute));        require(token.transferFrom(msg.sender,owner,tokensForOwner));                E_AirdropSubmitted(_tokenAddress,ta.tokenOwner,ta.totalDropped,ta.airdropDate,ta.airdropExpirationDate);`
```

```
`}`
```

**airdropTokens()函数存储合同在其内部余额中允许使用的令牌。其中 2%转让给合同业主，其余转让给合同。然后，它可以将它分发给当时已经订阅的用户。**

**执行空投的团队还可以通过调用此函数来收回在每次空投的到期日期后仍无人认领的令牌:**

```
`function returnTokensToAirdropper(address _tokenAddress) public ifNotPaused {        require(tokenWhitelist[_tokenAddress]); // Token must be whitelisted first                // Get the token        ERC20Basic token = ERC20Basic(_tokenAddress);                 uint tokensToReturn = 0;                for (uint i =0; i<airdroppedTokens[_tokenAddress].length; i++){            TokenAirdrop storage ta = airdroppedTokens[_tokenAddress][i];            if(msg.sender == ta.tokenOwner &&                airdropHasExpired(_tokenAddress,i)){                                tokensToReturn = tokensToReturn.add(ta.tokenBalance);                ta.tokenBalance = 0;            }        }        require(token.transfer(msg.sender,tokensToReturn));        E_TokensWithdrawn(_tokenAddress,msg.sender,tokensToReturn,now);`
```

```
`}`
```

#### **象征性提款**

**我们需要检查的最后一件事是用户撤回发送给他们的令牌的过程。为了做到这一点，他们需要调用`withdrawTokens(address _tokenAddress)` 函数。该功能将检查指定令牌的所有活动(尚未过期或冻结)空投，并传输它们。**

```
`function withdrawTokens(address _tokenAddress) ifNotPaused public {        require(tokenWhitelist[_tokenAddress]); // Token must be whitelisted first                // Get User instance, given the sender account        User storage user = signups[msg.sender];        require(user.userAddress != address(0));                uint totalTokensToTransfer = 0;        // For each airdrop made for this token (token owner may have done several airdrops at any given point)        for (uint i =0; i<airdroppedTokens[_tokenAddress].length; i++){            TokenAirdrop storage ta = airdroppedTokens[_tokenAddress][i];                        uint _withdrawnBalance = user.withdrawnBalances[_tokenAddress][i];                        //Check that user signed up before the airdrop was done. If so, he is entitled to the tokens            //And the airdrop must not have expired            if(ta.airdropDate >= user.signupDate &&                now <= ta.airdropExpirationDate){                                // The user will get a portion of the total tokens airdroped,                // divided by the users at the moment the airdrop was created                uint tokensToTransfer = ta.totalDropped.div(ta.usersAtDate);                                // if the user has not alreay withdrawn the tokens                if(_withdrawnBalance < tokensToTransfer){                    // Register the tokens withdrawn by the user and total tokens withdrawn                    user.withdrawnBalances[_tokenAddress][i] = tokensToTransfer;                    ta.tokenBalance = ta.tokenBalance.sub(tokensToTransfer);                    totalTokensToTransfer = totalTokensToTransfer.add(tokensToTransfer);                                    }            }        }        // Get the token        ERC20Basic token = ERC20Basic(_tokenAddress);         // Transfer tokens from all airdrops that correspond to this user        require(token.transfer(msg.sender,totalTokensToTransfer));                E_TokensWithdrawn(_tokenAddress,msg.sender,totalTokensToTransfer,now);    }`
```

#### **后续步骤**

**我接下来想做的许多事情之一是建立一个 web 界面(dapp ),让人们可以看到最新和即将到来的空投，并订阅它们。**

**如果你是一个拥有有效 ERC20 令牌的团队的一员，或者你正计划推出一个令牌，并且你想使用 Airdrop Central 进行空投，请给我写信。否则，非常感谢任何反馈和建议。**

**我希望你喜欢读这篇文章，就像我喜欢写它一样。我目前正在从事与智能合同开发相关的咨询工作。如果你打算通过 ICO 筹集资金，或者开发基于区块链的产品，请随时与我联系。**