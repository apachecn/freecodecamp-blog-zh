# 使用 Node.js 和 Seneca 编写象棋微服务，第 2 部分

> 原文：<https://www.freecodecamp.org/news/follow-the-rules-with-seneca-ii-c22074debac/>

#### 无需重构即可处理新需求

本系列的第 1 部分讲述了使用 Seneca 定义和调用微服务。创建了一些服务来返回棋盘上一个单独棋子的所有合法移动。该系列在[第三部](https://medium.com/@jefflowery/writing-a-chess-microservice-using-node-js-and-seneca-part-3-ab38b8ef9b0a)中继续。

#### 快速回顾:

*   Seneca 服务通过由`role`和`cmd`属性组成的模式来识别。还可以向模式中添加其他属性。

```
this.add({
        role: "movement",
        cmd: "legalMoves"   //, otherProp: value, ...
    }, (msg, reply) => {...}
```

*   服务也有一个实现，它接受一个`msg` 对象和一个回复回调。除了发送给服务的所有其他数据之外，`msg` 对象还包含模式属性。
*   `Seneca.act()`用于间接调用服务。`act`方法接受一个对象和一个回调函数。该对象包含`role`、`cmd`和其他组成服务消息的属性。

```
seneca.act({
            role: "movement",
            cmd: "legalMoves",
            piece: p,
            board: board
        }, (err, msg) => {
```

*   当一个动作可以由多个匹配模式的服务处理时，将调用与[最匹配的服务。](http://senecajs.org/getting-started/#how-patterns-work)

在本系列的[第一部分](https://medium.freecodecamp.com/follow-the-rules-with-seneca-b3cf3d08fe5d)中定义了一些服务。三个`rawMoves`服务中的一个将一个棋子及其位置作为参数，并返回 15 x 15 的移动掩码。使用一个`legalSquares`服务，这些被截断成一个 8 x 8 的板。结果是，这些服务可以一起返回空棋盘上任何合法方格中任何棋子的所有合法移动。

### 微服务和技术债务

微服务的动机之一是[减少技术债务](http://www.infoworld.com/article/2878659/application-development/reducing-technical-debt-with-microservices.html)。每个项目都有截止日期，当截止日期越来越大时，权宜之计往往胜过质量。过一会儿，FIXME 和 TODO 注释会在源代码中留下痕迹。这些评论指出了“有一天”会被解决的技术债务。

#### 某一天永远不会到来

微服务侧重于功能分解和松散耦合。这两个都不是新的想法，但它是对如何实现这些概念的重新思考。微服务应该是小型的、单一用途的、可扩展的。扩展服务几乎没有副作用。新服务可以扩展现有服务，旧服务和调用它的客户机都不知道服务实现发生了变化。减少对类、方法、方法签名、流程流的重构……所有这些都使得对付可怕的 TD 变得更容易。

### 回到正在进行的游戏…

在一个孤独的棋盘上移动一颗棋子并不那么有趣。在真正的国际象棋游戏中，棋盘上有友好的和敌对的棋子，这些棋子会影响彼此的移动。

现在我有一个`legalSquares`服务，它可以成为一个更完整的`legalMoves`服务的基础。如果您还记得，`legalSquares` 服务将调用`rawMoves`服务，然后移除所有不属于棋盘的“坏”方格。

新的`legalMoves` 服务将考虑其他部分，这是`legalSquares`没有考虑的。这需要一个额外的参数，叫做`board`。`board` 只是一个由**棋子**实例组成的数组，并假设棋盘上的棋子已经被检查过有效性。例如，两个棋子不在同一个方格中，棋子不在第一排，国王不在一起，等等。

以下模式将识别服务:

```
'role: movement;cmd: legalMoves'
```

这个模式是 JSON 的一个字符串化版本，叫做**jsonic**；如果您愿意，可以使用常规的 JSON 对象。发送给服务的消息将包含该模式。它还将包含一个 ChessPiece 实例，该实例有一个棋子类型，如“K”ing、“Q'ueen”、“R'ook”和棋盘位置(参见代数符号)。稍后我会给这个类添加一块颜色(白色或黑色)，这样这个服务就可以区分朋友和敌人。但是现在，该服务将假设所有的部分都是友好的。

因为一个友方棋子不能被捕获，它将限制其他友方棋子的移动。确定这些限制有点麻烦。在实现`rawMoves`服务的过程中，我给自己增加了难度……这让我想到:

### 微服务不是万能的

如果你设计了一个检索或计算信息的服务，而**没有**将数据向上传递，那么上游的一些服务可能不得不在以后重做这项工作。在我的例子中，`rawMoves` 返回了一个 move 对象的数组(文件和棋盘上的排名位置)。让我们采用使用`rawMoves`服务生成棋子对角线移动的方法:

```
module.exports = function diagonal(position, range = 7) {
    var moves = [];
    const cFile = position.file.charCodeAt()
    const cRank = position.rank.charCodeAt();

for (var i = 1; i < range + 1; i++) {
        moves.push({
            file: String.fromCharCode(cFile - i),
            rank: String.fromCharCode(cRank - i)
        });
        moves.push({
            file: String.fromCharCode(cFile + i),
            rank: String.fromCharCode(cRank + i)
        });
        moves.push({
            file: String.fromCharCode(cFile - i),
            rank: String.fromCharCode(cRank + i)
        });
        moves.push({
            file: String.fromCharCode(cFile + i),
            rank: String.fromCharCode(cRank - i)
        });
    }
    return moves;
}
```

乍一看，这没什么不好。但是，那四个`move.push`操作实际上是沿着**运动矢量**运行的。我可以构造四个移动向量，然后通过连接它们返回一个移动列表，就像这样:

```
function diagonalMoves(position, range) {
    var vectors = [[], [], [], []];
    const cFile = position.file.charCodeAt()
    const cRank = position.rank.charCodeAt();

    for (var i = 1; i < range + 1; i++) {
        vectors[0].push({
            file: String.fromCharCode(cFile - i),
            rank: String.fromCharCode(cRank - i)
        });
        vectors[1].push({
            file: String.fromCharCode(cFile + i),
            rank: String.fromCharCode(cRank + i)
        });
        vectors[2].push({
            file: String.fromCharCode(cFile - i),
            rank: String.fromCharCode(cRank + i)
        });
        vectors[3].push({
            file: String.fromCharCode(cFile + i),
            rank: String.fromCharCode(cRank - i)
        });
    }

    const moves = Array.prototype.concat(...vectors)
    return moves;
}
```

照目前的情况来看，这样做毫无意义。但是后来当一个友军挡住去路的时候，这些向量会很方便的截断对角线上的移动。相反，我不得不沿着服务上游的向量分解移动列表——更多的工作和低效率将在后面看到。

然而，真正的缺陷是我返回了一个数组，而不是一个数据对象。数据对象有可扩展的属性，数组没有。结果，我所有的上游服务都依赖于接收一个移动数组，和**只有**一个移动数组。没有灵活性。我现在不能添加一个移动向量列表**除了** 到一个移动列表。但是如果我从这个方法和调用它的服务中返回一个对象，我就可以了。

吸取教训了？考虑从服务中返回数据对象。让您的上游服务处理部分数据，但是将它们收到的所有数据传递回上游。当然，这条规则也有很多例外。

### 有这样的朋友…

在第 1 部分中，该模式下有一个服务:

`role:"movement",cmd:"legalSquares"`

它返回一个无阻碍棋子的所有移动。由于这将是决定棋盘上合法移动的基础服务，我将把`cmd`重命名为`legalMoves`。现在我想扩展一下，把可能阻挡我所选棋子的友军棋子也考虑进去。

#### 延伸服务

延伸`role:"movement",cmd:"legalMoves"`的服务是… `role:"movement",cmd:"legalMoves"`！

是的，它有和它调用的服务相同的服务模式。您可能还记得服务是由模式识别的，那么它是如何工作的呢？当程序作用于`role:"movement",cmd:"legalMoves"`时，它将使用最近定义的服务。但是新服务必须调用以前的`legalMoves`服务。这很容易解决:

```
this.add({
        role: "movement",
        cmd: "legalMoves"
    }, (msg, reply) => {//returns unimpeded moves}

this.add('role:movement,cmd:legalMoves', function (msg, reply) {
        this.
prior(msg, function (err, moves) {
            if (msg.board) {
                const boardMoves = legalMovesWithBoard(msg, moves);
                reply(err, boardMoves);
                return;
            }
            reply(err, moves);
        });
    });
```

这个新服务能够通过使用 Seneca 中的`prior()`方法调用以前的服务。如果在传入的`msg` 对象中没有提供`board`参数，那么这个服务将只作为前一个服务的通道。但是如果有板子呢？

我不会在这里展示完整的代码清单(见下面的链接)，但它的要点是:

```
module.exports = function (msg, moves) {
    if (!msg.board) return moves;

const blockers = moves.filter(m => {
        return (msg.board.pieceAt(m))
    })

var newMoves = [];
    const pp = msg.piece.position;

const rangeChecks = {
        B: diagonalChecks,
        R: rankAndFileChecks,
        K: panopticonChecks,
        Q: panopticonChecks,
        P: pawnChecks,
        N: knightChecks
    };

var rangeCheck = rangeChecks[msg.piece.piece];
    // console.error(msg.piece.piece, rangeCheck.name)
    newMoves = moves.filter(m => {
        return rangeCheck(m, blockers, pp);
    })
    return newMoves;
}
```

还记得我们在`rawMoves`服务部的老朋友`diagonalMoves`吗？为了对没有方便矢量的对角线进行范围检查，新的`legalMoves` 服务称之为:

```
// m: proposed move
// blockers: blocking pieces
// pp: current piece position
function diagonalChecks(m, blockers, pp) {
    let isGood = true;
for (const b of blockers) {
        if (b.rank > pp.rank && b.file > pp.file) {
            if (m.rank > pp.rank && m.file > pp.file) {
                isGood = isGood && (m.rank < b.rank && m.file < b.file);
            }
        }
        if (b.rank > pp.rank && b.file < pp.file) {
            if (m.rank > pp.rank && m.file < pp.file) {
                isGood = isGood && (m.rank < b.rank && m.file > b.file)
            }
        }
        if (b.rank < pp.rank && b.file > pp.file) {
            if (m.rank < pp.rank && m.file > pp.file) {
                isGood = isGood && (m.rank > b.rank && m.file < b.file)
            }
        }
        if (b.rank < pp.rank && b.file < pp.file) {
            if (m.rank < pp.rank && m.file < pp.file) {
                isGood = isGood && (m.rank > b.rank && m.file > b.file)
            }
        }
    }
return isGood;
}
```

diagonalMoves.js

很丑，不是吗？如果有算法倾向的读者在评论部分把它减少到两行，我会很高兴。甚至三个。

这样就能处理好友好的部分。下一部分将处理敌对的棋子，这些棋子可以被捕获。

本文的完整源代码可以在 [GitHub](https://github.com/JeffML/ms-chess2) 找到。