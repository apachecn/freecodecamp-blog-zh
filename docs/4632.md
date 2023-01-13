# 不，TypeScript & JavaScript 中的 Getters 和 Setters 并不是无用的

> 原文：<https://www.freecodecamp.org/news/typescript-javascript-getters-and-setters-are-they-useless/>

> 在这篇博文中，我们将讨论 getters 和 setters 在现代 web 开发中的效用。他们没用吗？什么时候使用它们有意义？

当 ECMAScript 5 (2009)发布时，Getters 和 setters(也称为访问器)被引入到 JavaScript 中。

问题是，关于它们的效用以及为什么你会想要使用它们，有很多困惑。

我偶然发现了这个 reddit 帖子,讨论的是它们是否是反模式。

不幸的是，该主题的普遍共识是“是”。我认为这是因为大多数日常的前端编程并不需要 getters 和 setters 提供的工具。

尽管我不同意 getters 和 setters 作为反模式的观点。它们在几种不同的情况下有很大的用处。

## 分别是什么*？*

Getters 和 setters 是访问对象属性的另一种方式。

琐碎的用法可能如下所示:

```
interface ITrackProps {
  name: string;
  artist: string;
}

class Track {  
  private props: ITrackProps;

  get name (): string {
    return this.props.name;
  }

  set name (name: string) {
	  this.props.name = name;
  }

  get artist (): string {
    return this.props.artist;
  }

  set artist (artist: string) {
	  this.props.artist = artist;
  }

  constructor (props: ITrackProps) {
    this.props = props;
  } 

  public play (): void {	
	  console.log(`Playing ${this.name} by ${this.artist}`);
  }
} 
```

问题变成了:“为什么不直接使用常规的类属性？”

嗯，在这种情况下，*我们可以*。

```
interface ITrackProps {
  name: string;
  artist: string;
}

class Track {  
  public name: string;
  public artist: string;

  constructor (name: string, artist: string;) {
    this.name = name;
    this.artist = artist;
  } 

  public play (): void {	
	  console.log(`Playing ${this.name} by ${this.artist}`);
  }
} 
```

那就简单多了。这也是一个非常简单的使用案例。让我们来看一些场景，它们更好地描述了为什么我们会关心使用 getter 和 better 而不是常规的类属性。

## 防止贫血的领域模型

你还记得什么是[贫血领域模型](https://khalilstemmler.com/wiki/anemic-domain-model/)吗？找出一个缺乏活力的领域模型的最早方法之一是，如果你的领域实体的每一个属性都有**的 getters 和 setters(即:对特定领域语言没有意义的 *set* 操作被暴露出来)。**

如果你不明确地使用`get`或`set`关键词，让一切`public`也有同样的负面影响。

考虑这个例子:

```
class User {
  // Bad. You can now `set` the user id.
  // When would you ever need to mutate a user's id to a
  // different identifier? Is that safe? Should you be able to?
  public id: UserId;

  constuctor (id: UserId) {
    this.id = id;
  }
} 
```

在领域驱动的设计中，为了防止一个缺乏活力的领域模型并推进特定领域语言的创建，对我们来说真正重要的是*只公开对领域有效的操作*。

这意味着[了解你在](https://khalilstemmler.com/articles/solid-principles/single-responsibility/)工作的领域。

我会让自己接受审查。让我们来看看来自 [White Label](https://github.com/stemmlerjs/white-label) 的`Vinyl`类，这是一个开源的乙烯交易应用程序，使用域驱动设计用 TypeScript 构建。

```
import { AggregateRoot } from "../../core/domain/AggregateRoot";
import { UniqueEntityID } from "../../core/domain/UniqueEntityID";
import { Result } from "../../core/Result";
import { Artist } from "./artist";
import { Genre } from "./genre";
import { TraderId } from "../../trading/domain/traderId";
import { Guard } from "../../core/Guard";
import { VinylCreatedEvent } from "./events/vinylCreatedEvent";
import { VinylId } from "./vinylId";

interface VinylProps {
  traderId: TraderId;
  title: string;
  artist: Artist;
  genres: Genre[];
  dateAdded?: Date;
}

export type VinylCollection = Vinyl[];

export class Vinyl extends AggregateRoot<VinylProps> {

  public static MAX_NUMBER_GENRES_PER_VINYL = 3;

  // ? 1\. Facade. The VinylId key doesn't actually exist
  // as a property of VinylProps, yet- we still need
  // to provide access to it.

  get vinylId(): VinylId {
    return VinylId.create(this.id)
  }

  get title (): string {
    return this.props.title;
  }

  // ? 2\. All of these properties are nested one layer
  // deep as props so that we can control access 
  // and mutations to the ACTUAL values.

  get artist (): Artist {
    return this.props.artist
  }

  get genres (): Genre[] {
    return this.props.genres;
  }

  get dateAdded (): Date {
    return this.props.dateAdded;
  }

  // ? 3\. You'll notice that there are no setters so far because 
  // it doesn't make sense for us to be able to change any of these
  // things after it has been created

  get traderId (): TraderId {
    return this.props.traderId;
  }

  // ? 4\. This approach is called "Encapsulate Collection". We
  // will need to add genres, yes. But we still don't expose the
  // setter because there's some invariant logic here that we want to
  // ensure is enforced.
  // Invariants: 
  // https://khalilstemmler.com/wiki/invariant/

  public addGenre (genre: Genre): void {
    const maxLengthExceeded = this.props.genres
      .length >= Vinyl.MAX_NUMBER_GENRES_PER_VINYL;

    const alreadyAdded = this.props.genres
      .find((g) => g.id.equals(genre.id));

    if (!alreadyAdded && !maxLengthExceeded) {
      this.props.genres.push(genre);
    }
  }

  // ? 5\. Provide a way to remove as well.

  public removeGenre (genre: Genre): void {
    this.props.genres = this.props.genres
      .filter((g) => !g.id.equals(genre.id));
  }

  private constructor (props: VinylProps, id?: UniqueEntityID) {
    super(props, id);
  }

  // ? 6\. This is how we create Vinyl. After it's created, all properties 
  // effectively become "read only", except for Genre because that's all that
  // makes sense to enabled to be mutated.

  public static create (props: VinylProps, id?: UniqueEntityID): Result<Vinyl> {
    const propsResult = Guard.againstNullOrUndefinedBulk([
      { argument: props.title, argumentName: 'title' },
      { argument: props.artist, argumentName: 'artist' },
      { argument: props.genres, argumentName: 'genres' },
      { argument: props.traderId, argumentName: 'traderId' }
    ]);

    if (!propsResult.succeeded) {
      return Result.fail<Vinyl>(propsResult.message)
    } 

    const vinyl = new Vinyl({
      ...props,
      dateAdded: props.dateAdded ? props.dateAdded : new Date(),
      genres: Array.isArray(props.genres) ? props.genres : [],
    }, id);
    const isNewlyCreated = !!id === false;

    if (isNewlyCreated) {
      // ? 7\. This is why we need VinylId. To provide the identifier
      // for any subscribers to this domain event.
      vinyl.addDomainEvent(new VinylCreatedEvent(vinyl.vinylId))
    }

    return Result.ok<Vinyl>(vinyl);
  }
} 
```

在[领域驱动设计](https://khalilstemmler.com/articles/domain-driven-design-intro/)中，充当门面、维护只读值、增强模型表现力、封装集合以及[创建领域事件](https://khalilstemmler.com/blogs/domain-driven-design/where-do-domain-events-get-dispatched/)是获取者和设置者的一些非常可靠的用例。

## Vue.js 中的更改检测

Vue.js ，一个较新的前端框架，以非常快速和反应灵敏而自豪。

Vue.js 如此高效地检测变化的原因是因为他们使用`Object.defineProperty()` [API](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 来*观察*对视图模型的变化！

从 Vue.js 关于反应性的文档中，

> 当您将普通 JavaScript 对象作为其数据选项传递给 Vue 实例时，Vue 将遍历其所有属性，并使用 Object.defineProperty 将它们转换为 getter/setter。getter/setter 对用户是不可见的，但在幕后，它们使 Vue 能够在属性被访问或修改时执行依赖跟踪和更改通知。- [Vue.js 文档:反应性](https://vuejs.org/v2/guide/reactivity.html)

* * *

总之，getters 和 setter*do*对于许多不同的问题都有很大的用处。这些问题在现代前端 web 开发中并不经常出现。

-

## 高级类型脚本& Node.js 博客

如果你喜欢这篇文章，你应该看看我的博客。我写了关于**高级类型脚本& Node.js 大型应用程序的最佳实践**，并教开发人员如何编写灵活、可维护的软件。