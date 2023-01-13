# TypeScript 静态成员| TypeScript OOP

> 原文：<https://www.freecodecamp.org/news/all-about-typescript-static-members-typescript-oop/>

在面向对象编程中，我们写了很多*类* 。

类包含保存变量和操作的*属性* ( **方法**和**属性**)。

每当我们定义一个类的属性时，它们被认为属于以下两者之一:

*   类的*实例*(通过构造函数创建的对象)或
*   类*本身*(我们称之为类成员)

我们这么说是什么意思？

属性怎么可能只属于*实例*而只属于*类*？

当我们选择使用或省略`static`关键字时，它会改变属性属于谁。

让我们看看不带`static`关键字的常规用法。

## 常规用法(属性属于实例)

通常，当我们在一个类上定义属性时，它们唯一可以被*访问*的时间是在我们创建了该类的一个实例之后的**，或者如果我们使用`this`来引用最终将驻留在对象实例上的属性。**

以[白色标签](https://github.com/stemmlerjs/white-label)中的早期示例为例。

```
type Genre = 'rock' | 'pop' | 'electronic' | 'rap'

class Vinyl {
  public title: string;
  public artist: string;
  public genres: Genre[];

  constructor (title: string, artist: string, genres: Genre[]) {
    this.title = title;
    this.artist = artist;
    this.genres = genres;
  } 

  public printSummary (): void {
    console.log(`${this.title} is an album by ${this.artist}`);
  }
}

const vinyl = new Vinyl('Goo', 'Sonic Youth', ['rock']);
console.log(vinyl.title)    // 'Goo'
console.log(vinyl.artist)   // 'Sonic Youth'
console.log(vinyl.genres)   // ['rock']
vinyl.printSummary();	      // 'Goo is an album by Sonic Youth' 
```

`Vinyl`类上的每个方法(`printSummary(): void`)和属性(`title`、`artist`、`genres`)都属于该类的一个*实例*。

在这个例子中，在被创建后，我们只能直接从对象*中访问属性`title`、`artist`和`genres`。*

```
console.log(vinyl.title)    // This is valid! 
```

还要注意，当我们使用`printSummary(): void`时，我们可以使用`this`关键字访问`title`和`artist`:

```
class Vinyl {
  ...
  public printSummary (): void {
    console.log(`${this.title} is an album by ${this.artist}`);
  }
} 
```

这样做是因为此时，`Vinyl`的结果对象/实例拥有这些属性。

如果我们检查一下 [TypeScript Playground](http://www.typescriptlang.org/play/) ，我们可以看看这个代码样本的编译后的 JavaScript:

```
"use strict";
class Vinyl {
  constructor(title, artist, genres) {
    this.title = title;
    this.artist = artist;
    this.genres = genres;
  }
  printSummary() {
    console.log(`${this.title} is an album by ${this.artist}`);
  }
}

const vinyl = new Vinyl('Goo', 'Sonic Youth', ['rock']);
console.log(vinyl.title); // 'Goo'
console.log(vinyl.artist); // 'Sonic Youth'
console.log(vinyl.genres); // ['rock']
vinyl.printSummary(); // 'Goo is an album by Sonic Youth' 
```

最终的 JavaScript 看起来*几乎和*一模一样。

让我们谈一谈当属性归*类*所有时会发生什么。

## 静态属性(属性属于类)

当我们在类上定义的属性上使用`static`关键字时，它们属于*类本身*。

这意味着我们不能从类的实例中访问那些属性。

我们只能通过引用类本身来直接访问属性。

为了演示，让我们添加一个计数器`NUM_VINYL_CREATED`,它增加一个`Vinyl`被创建的次数。

```
type Genre = 'rock' | 'pop' | 'electronic' | 'rap'

class Vinyl {
  public title: string;
  public artist: string;
  public genres: Genre[];
  public static NUM_VINYL_CREATED: number = 0;

  constructor (title: string, artist: string, genres: Genre[]) {
    this.title = title;
    this.artist = artist;
    this.genres = genres;

	  Vinyl.NUM_VINYL_CREATED++;        // increment number of vinyl created
    console.log(Vinyl.NUM_VINYL_CREATED)  
  } 

  public printSummary (): void { 
    console.log(`${this.title} is an album by ${this.artist}`);
  }
}

let goo = new Vinyl ('Goo', 'Sonic Youth', ['rock']);
// prints out 0

let daydream = new Vinyl ('Daydream Nation', 'Sonic Youth', ['rock']);
// prints out 1 
```

因为属性只能通过类本身来访问，所以我们不能:

```
let goo = new Vinyl ('Goo', 'Sonic Youth', ['rock']);
goo.MAX_GENRES_PER_VINYL    // Error
goo.NUM_VINYL_CREATED       // Error 
```

你可能听说过一个术语叫做**类成员**。属性或方法是一个*类成员*，因为它们只能通过类本身来访问；因此，他们是这个班级的成员。

这很好，但是什么时候你会想要使用静态属性呢？

## 如何知道何时使用静态属性

在添加属性或方法之前，作为您自己:

> 在没有这个类的*实例的情况下，这个属性需要被另一个类使用吗？*

换句话说，我需要在这个类创建的**对象**上调用它吗？如果是，那么正常继续。

如果不是，那么你可能想成为一个`static`成员。

### 使用静态属性可能有意义的场景

*   检查另一个类的业务规则或约束
*   实现一个`factory method`到[封装的复杂性](https://khalilstemmler.com/articles/typescript-value-object/)是为了创建一个类的实例
*   使用一个`abstract factory`来让[创建一个特定类型的类](https://khalilstemmler.com/wiki/abstract-factory/)的实例
*   当属性不应该改变时

### it *看起来像*的场景可能有意义，但实际上导致了[贫血的领域模型](https://khalilstemmler.com/wiki/anemic-domain-model/):

*   对该类的属性执行验证逻辑(使用[值对象](https://khalilstemmler.com/articles/typescript-value-object/))

为了演示一个有价值的场景，让我们为“记录一个约束”添加一个`static` `MAX_GENRES_PER_VINYL`属性，一个`Vinyl`最多只能有 2 个不同类型的`Genres`。

```
type Genre = 'rock' | 'pop' | 'electronic' | 'rap'

class Vinyl {
  public title: string;
  public artist: string;
  public genres: Genre[];
  public static MAX_GENRES_PER_VINYL: number = 2;

  constructor (title: string, artist: string, genres: Genre[]) {
    this.title = title;
    this.artist = artist;
    this.genres = genres;
  }

  public printSummary (): void { 
    console.log(`${this.title} is an album by ${this.artist}`);
  }
} 
```

然后让我们添加一个`addGenre(genre: Genre): void`方法来执行这个业务规则。

```
type Genre = 'rock' | 'pop' | 'electronic' | 'rap'

class Vinyl {
  public title: string;
  public artist: string;
  public genres: Genre[];
  public static MAX_GENRES_PER_VINYL: number = 2;

  constructor (title: string, artist: string, genres: Genre[]) {
    this.title = title;
    this.artist = artist;
    this.genres = genres;
  }

  public addGenre (genre: Genre): void {
    // Notice that in order to reference the value, we have go through the class
    // itself (Vinyl), not through an instance of the class (this).
    const maxLengthExceeded = this.genres.length < Vinyl.MAX_GENRES_PER_VINYL;
    const alreadyAdded = this.genres.filter((g) => g === genre).length !== 0;

    if (!maxLengthExceeded && !alreadyAdded) {
      this.genres.push(genre);
    }
  }

  public printSummary (): void { 
    console.log(`${this.title} is an album by ${this.artist}`);
  }
} 
```

* * *

## 高级类型脚本& Node.js 博客

如果你喜欢这篇文章，你应该看看我的博客。我写了关于**高级类型脚本& Node.js 大型应用程序的最佳实践**，并教开发人员如何编写灵活、可维护的软件。