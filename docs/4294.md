# 如何使用 Python 创建自己的加密货币

> 原文：<https://www.freecodecamp.org/news/create-cryptocurrency-using-python/>

> 随着加密货币的兴起，区块链正在科技界引起轰动。这项技术之所以吸引了如此多的关注，主要是因为它能够保证安全性、实施去中心化，并加快几个行业的流程，尤其是金融业。

从本质上讲，区块链是一个公共数据库，它不可逆转地记录和验证数字资产的拥有和传输。数字货币，像比特币、以太坊都是基于这个概念。区块链是一项令人兴奋的技术，可以用来转变应用程序的功能。

最近，我们已经看到政府、组织和个人使用区块链技术来创建他们自己的加密货币，以避免落后。值得注意的是，当脸书提出自己的名为 [Libra](https://libra.org/en-US/white-paper/#introduction) 的加密货币时，这一声明在世界范围内激起了轩然大波。

如果你也可以效仿，创建自己版本的加密货币，会怎么样？

我考虑过这个问题，并决定开发一种算法来创建一个密码。

我决定把加密货币**叫做 fcccoun**。

在本教程中，我将说明我用来构建数字货币的一步一步的过程(我使用了 [Python](https://www.freecodecamp.org/news/best-python-tutorial/) 编程语言的面向对象概念)。

下面是创建 **fccCoin** 的区块链算法的基本蓝图:

```
class Block:

    def __init__():

    #first block class

        pass

    def calculate_hash():

    #calculates the cryptographic hash of every block

class BlockChain:

    def __init__(self):
     # constructor method
    pass

    def construct_genesis(self):
        # constructs the initial block
        pass

    def construct_block(self, proof_no, prev_hash):
        # constructs a new block and adds it to the chain
        pass

    @staticmethod
    def check_validity():
        # checks whether the blockchain is valid
        pass

    def new_data(self, sender, recipient, quantity):
        # adds a new transaction to the data of the transactions
        pass

    @staticmethod
    def construct_proof_of_work(prev_proof):
        # protects the blockchain from attack
        pass

    @property
    def last_block(self):
        # returns the last block in the chain
        return self.chain[-1] 
```

现在，让我来解释发生了什么…

## 1.构建第一个块类

区块链由几个相互连接的积木组成(听起来很熟悉，对吗？).

块的链接是这样发生的，如果一个块被篡改，则链的其余部分变得无效。

在应用上述概念时，我创建了以下初始块类:

```
import hashlib
import time

class Block:

    def __init__(self, index, proof_no, prev_hash, data, timestamp=None):
        self.index = index
        self.proof_no = proof_no
        self.prev_hash = prev_hash
        self.data = data
        self.timestamp = timestamp or time.time()

    @property
    def calculate_hash(self):
        block_of_string = "{}{}{}{}{}".format(self.index, self.proof_no,
                                              self.prev_hash, self.data,
                                              self.timestamp)

        return hashlib.sha256(block_of_string.encode()).hexdigest()

    def __repr__(self):
        return "{} - {} - {} - {} - {}".format(self.index, self.proof_no,
                                               self.prev_hash, self.data,
                                               self.timestamp) 
```

从上面的代码中可以看出，我定义了 **__init__()** 函数，它将在 **Block** 类被初始化时执行，就像在任何其他 Python 类中一样。

我向初始化函数提供了以下参数:

*   **self**—这是指**块**类的实例，使得访问与该类相关的方法和属性成为可能；
*   **索引**—这跟踪块在区块链内的位置；
*   **proof _ no**—这是在创建新区块(称为采矿)期间产生的数字；
*   **prev _ hash**—这是指链内的前一个块的散列；
*   **数据**—这给出了完成的所有交易的记录，例如购买的数量；
*   **时间戳**—为交易添加时间戳。

类中的第二个方法， **calculate_hash** ，将使用上述值生成块的散列。SHA-256 模块被导入到项目中，以帮助获得块的散列。

将值输入加密哈希算法后，该函数将返回一个代表块内容的 256 位字符串。

这就是在区块链中实现安全性的方式——每个数据块都有一个哈希，而这个哈希将依赖于前一个数据块的哈希。

因此，如果有人试图破坏链中的任何块，其他块将具有无效的哈希，从而导致整个区块链网络的中断。

最终，块将看起来像这样:

```
{
    "index": 2,
    "proof": 21,
    "prev_hash": "6e27587e8a27d6fe376d4fd9b4edc96c8890346579e5cbf558252b24a8257823",
    "transactions": [
        {'sender': '0', 'recipient': 'Quincy Larson', 'quantity': 1}
    ],
    "timestamp": 1521646442.4096143
} 
```

## 2.构建区块链类

顾名思义，区块链的主要思想是将几个模块“链接”在一起。

因此，我将构建一个**区块链**类，它将有助于管理整个链的工作。这是大部分行动将要发生的地方。

**区块链**类将有各种帮助器方法，用于完成区块链中的各种任务。

让我解释一下类中每个方法的作用。

### a.构造函数方法

此方法确保区块链被实例化。

```
class BlockChain:

    def __init__(self):
        self.chain = []
        self.current_data = []
        self.nodes = set()
        self.construct_genesis() 
```

其属性的作用如下:

*   **self . chain**—该变量保留所有块；
*   **self . current _ data**—该变量保存块中所有已完成的事务；
*   **self . construct _ genesis()**—该方法将负责构建初始块。

### b.构建创世区块

区块链需要一个***construct _ genesis***方法来构建链中的初始块。在《区块链公约》中，这个街区很特别，因为它象征着区块链的开始。

在这种情况下，让我们通过简单地将一些默认值传递给 ***construct_block*** 方法来构造它。

我给 ***proof_no*** 和 ***prev_hash*** 的值都是零，虽然你可以提供你想要的任何值。

```
def construct_genesis(self):
    self.construct_block(proof_no=0, prev_hash=0)

def construct_block(self, proof_no, prev_hash):
    block = Block(
        index=len(self.chain),
        proof_no=proof_no,
        prev_hash=prev_hash,
        data=self.current_data)
    self.current_data = []

    self.chain.append(block)
    return block 
```

### 建造新的街区

***构造 _ 块*** 方法用于在区块链创建新块。

下面是这个方法的各种属性所发生的情况:

*   **指数**—代表区块链的长度；
*   **proof _ nor&prev _ hash**—调用者方法传递它们；
*   **数据**—这包含不包括在节点上的任何块中的所有事务的记录；
*   **self . current _ data**—用于重置节点上的交易列表。如果已经构造了一个块，并且事务被分配给该块，则重置该列表以确保将来的事务被添加到该列表中。而且，这个过程会不断发生；
*   **self.chain.append()—** 该方法将新构建的块加入到链中；
*   **返回**—最后，返回一个构造好的块对象。

### d.检查有效性

***check _ validity***方法对于评估区块链的完整性并确保不存在异常非常重要。

如前所述，哈希对于区块链的安全性至关重要，因为对象中哪怕是最轻微的变化都会导致生成全新的哈希。

因此，这种 ***check_validity*** 方法使用 ***if*** 语句来检查每个块的 hash 是否正确。

它还通过比较每个块的哈希值来验证每个块是否都指向正确的前一个块。如果一切都正确，则返回 true 否则，它返回 false。

```
@staticmethod
def check_validity(block, prev_block):
    if prev_block.index + 1 != block.index:
        return False

    elif prev_block.calculate_hash != block.prev_hash:
        return False

    elif not BlockChain.verifying_proof(block.proof_no, prev_block.proof_no):
        return False

    elif block.timestamp <= prev_block.timestamp:
        return False

    return True 
```

### e.添加交易数据

***new _ data***方法用于将交易数据添加到块中。这是一个非常简单的方法:它接受三个参数(发送者的详细信息、接收者的详细信息和数量)并将交易数据附加到***self . current _ data***列表中。

每当创建一个新的块时，该列表被分配给该块，并再次重置，如 ***construct_block*** 方法中所述。

将事务数据添加到列表中后，将返回下一个要创建的块的索引。

该索引是通过将当前块(区块链中的最后一个)的索引加 1 来计算的。这些数据将有助于用户将来提交交易。

```
def new_data(self, sender, recipient, quantity):
    self.current_data.append({
        'sender': sender,
        'recipient': recipient,
        'quantity': quantity
    })
    return True 
```

### 
f .添加工作证明

[工作证明](https://en.bitcoin.it/wiki/Proof_of_work)是防止区块链被滥用的概念。简单地说，它的目标是在完成一定量的计算工作后，确定一个解决问题的数字。

如果识别该号码的难度很高，它会阻止垃圾邮件和篡改区块链。

在这种情况下，我们将使用一个简单的算法，阻止人们轻易挖掘块或创建块。

```
@staticmethod
def proof_of_work(last_proof):
    '''this simple algorithm identifies a number f' such that hash(ff') contain 4 leading zeroes
         f is the previous f'
         f' is the new proof
        '''
    proof_no = 0
    while BlockChain.verifying_proof(proof_no, last_proof) is False:
        proof_no += 1

    return proof_no

@staticmethod
def verifying_proof(last_proof, proof):
    #verifying the proof: does hash(last_proof, proof) contain 4 leading zeroes?

    guess = f'{last_proof}{proof}'.encode()
    guess_hash = hashlib.sha256(guess).hexdigest()
    return guess_hash[:4] == "0000" 
```

### g.得到最后一个街区

最后， ***latest_block*** 方法是一个帮助器方法，帮助获得区块链中的最后一个块。记住，最后一个块实际上是链中的当前块。

```
@property
    def latest_block(self):
        return self.chain[-1] 
```

## 让我们把一切加在一起

这里是创建 **fccCoin** 加密货币的完整代码。

你也可以在这个 GitHub 库上获得代码。

```
import hashlib
import time

class Block:

    def __init__(self, index, proof_no, prev_hash, data, timestamp=None):
        self.index = index
        self.proof_no = proof_no
        self.prev_hash = prev_hash
        self.data = data
        self.timestamp = timestamp or time.time()

    @property
    def calculate_hash(self):
        block_of_string = "{}{}{}{}{}".format(self.index, self.proof_no,
                                              self.prev_hash, self.data,
                                              self.timestamp)

        return hashlib.sha256(block_of_string.encode()).hexdigest()

    def __repr__(self):
        return "{} - {} - {} - {} - {}".format(self.index, self.proof_no,
                                               self.prev_hash, self.data,
                                               self.timestamp)

class BlockChain:

    def __init__(self):
        self.chain = []
        self.current_data = []
        self.nodes = set()
        self.construct_genesis()

    def construct_genesis(self):
        self.construct_block(proof_no=0, prev_hash=0)

    def construct_block(self, proof_no, prev_hash):
        block = Block(
            index=len(self.chain),
            proof_no=proof_no,
            prev_hash=prev_hash,
            data=self.current_data)
        self.current_data = []

        self.chain.append(block)
        return block

    @staticmethod
    def check_validity(block, prev_block):
        if prev_block.index + 1 != block.index:
            return False

        elif prev_block.calculate_hash != block.prev_hash:
            return False

        elif not BlockChain.verifying_proof(block.proof_no,
                                            prev_block.proof_no):
            return False

        elif block.timestamp <= prev_block.timestamp:
            return False

        return True

    def new_data(self, sender, recipient, quantity):
        self.current_data.append({
            'sender': sender,
            'recipient': recipient,
            'quantity': quantity
        })
        return True

    @staticmethod
    def proof_of_work(last_proof):
        '''this simple algorithm identifies a number f' such that hash(ff') contain 4 leading zeroes
         f is the previous f'
         f' is the new proof
        '''
        proof_no = 0
        while BlockChain.verifying_proof(proof_no, last_proof) is False:
            proof_no += 1

        return proof_no

    @staticmethod
    def verifying_proof(last_proof, proof):
        #verifying the proof: does hash(last_proof, proof) contain 4 leading zeroes?

        guess = f'{last_proof}{proof}'.encode()
        guess_hash = hashlib.sha256(guess).hexdigest()
        return guess_hash[:4] == "0000"

    @property
    def latest_block(self):
        return self.chain[-1]

    def block_mining(self, details_miner):

        self.new_data(
            sender="0",  #it implies that this node has created a new block
            receiver=details_miner,
            quantity=
            1,  #creating a new block (or identifying the proof number) is awarded with 1
        )

        last_block = self.latest_block

        last_proof_no = last_block.proof_no
        proof_no = self.proof_of_work(last_proof_no)

        last_hash = last_block.calculate_hash
        block = self.construct_block(proof_no, last_hash)

        return vars(block)

    def create_node(self, address):
        self.nodes.add(address)
        return True

    @staticmethod
    def obtain_block_object(block_data):
        #obtains block object from the block data

        return Block(
            block_data['index'],
            block_data['proof_no'],
            block_data['prev_hash'],
            block_data['data'],
            timestamp=block_data['timestamp']) 
```

现在，让我们测试一下我们的代码，看看它是否有效。

```
blockchain = BlockChain()

print("***Mining fccCoin about to start***")
print(blockchain.chain)

last_block = blockchain.latest_block
last_proof_no = last_block.proof_no
proof_no = blockchain.proof_of_work(last_proof_no)

blockchain.new_data(
    sender="0",  #it implies that this node has created a new block
    recipient="Quincy Larson",  #let's send Quincy some coins!
    quantity=
    1,  #creating a new block (or identifying the proof number) is awarded with 1
)

last_hash = last_block.calculate_hash
block = blockchain.construct_block(proof_no, last_hash)

print("***Mining fccCoin has been successful***")
print(blockchain.chain) 
```

成功了！

以下是挖掘过程的输出:

```
***Mining fccCoin about to start***
[0 - 0 - 0 - [] - 1566930640.2707076]
***Mining fccCoin has been successful***
[0 - 0 - 0 - [] - 1566930640.2707076, 1 - 88914 - a8d45cb77cddeac750a9439d629f394da442672e56edfe05827b5e41f4ba0138 - [{'sender': '0', 'recipient': 'Quincy Larson', 'quantity': 1}] - 1566930640.5363243] 
```

## 
结论

你知道了吧！

这就是如何使用 Python 创建自己的区块链。

让我说，本教程只是演示了让你的脚湿在创新的区块链技术的基本概念。

如果这种硬币按原样部署，它无法满足目前市场对稳定、安全和易用的加密货币的需求。

因此，它仍然可以通过添加额外的功能来提高其挖掘和发送[金融交易](https://www.forextradingbig.com/)的能力。

尽管如此，如果你决定让你的名字在神奇的密码世界中为人所知，这是一个很好的起点。

如果您有任何意见或问题，请在下面发表。

快乐(密码)编码！