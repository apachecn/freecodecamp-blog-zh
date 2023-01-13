# TypeScript 实用指南——如何使用 HTML、CSS 和 TypeScript 构建 Pokedex 应用程序

> 原文：<https://www.freecodecamp.org/news/a-practical-guide-to-typescript-how-to-build-a-pokedex-app-using-html-css-and-typescript/>

TypeScript 是一个需要编译成普通 JavaScript 的超集。它对您的代码提供了更多的控制，因为它使用类型注释、接口、类和静态类型检查来在编译时抛出错误。

TypeScript 有助于提高代码质量和可理解性，尤其是对于大型代码库。

在本指南中，我将带领您学习 TypeScript，首先学习开始使用这种伟大语言所需的所有基础知识。然后，我们将使用 HTML、CSS 和 TypeScript 从头开始构建一个应用程序。

让我们开始吧

*   [什么是 TypeScript？](#what-is-typescript)
*   [设置打字稿](#setting-up-typescript)
*   [用 tsconfig 配置 TypeScript】](#configuring-typescript-with-tsconfig)
*   [打字稿类型](#typescript-types)
*   [基本打字稿类型](#basic-typescript-types)
*   [接口和类型别名](#interfaces-and-type-aliases)
*   [使用 TypeScript 构建 Pokedex 应用程序](#build-a-pokedex-app-using-typescript)
*   [Markup](#markup)
*   [使用类型脚本](#fetch-and-display-data-using-typescript)获取并显示数据
*   [将类型脚本编译成 JavaScript](#compile-typescript-to-javascript)
*   [资源](#resources)

## 什么是 TypeScript？

TypeScript 是一种面向对象的编程语言，由微软开发和维护。它是 JavaScript 的超集，这意味着任何有效的 JavaScript 代码也将按预期在 TypeScript 中运行。

TypeScript 具有 JavaScript 的所有功能以及一些附加特性。它需要在运行时被编译成普通的 JavaScript，因此你需要一个编译器来取回 JS 代码。

TypeScript 使用静态类型，这意味着您可以在声明期间为变量赋予一个类型。这是 JavaScript 无法做到的，因为它是一种动态类型的语言——它不知道变量的数据类型，直到它在运行时给该变量赋值。

静态类型检查使 TypeScript 变得很棒，因为如果变量未被使用或被重新分配了不同的类型注释，它有助于在编译时抛出错误。但是，该错误不会阻止代码执行(JavaScript 代码仍然会生成)。

静态类型在 TypeScript 中是可选的。如果没有定义类型，但变量有一个值，TypeScript 会将该值推断为类型。如果变量没有值，默认情况下，类型将被设置为 any。

现在，让我们在下一节开始使用 TypeScript 来看看它的运行情况。

## 设置类型脚本

如前所述，TypeScript 需要编译成普通的 JavaScript。所以我们需要用一个工具来做编译。要访问该工具，您需要通过在终端上运行该命令来安装 TypeScript。

```
 yarn add -g typescript 
```

或者，如果您正在使用 npm:

```
 npm install -g typescript 
```

注意，这里我使用了`-g`标志来全局安装 TypeScript，这样我就可以从任何地方访问它。

通过安装 TypeScript，我们现在可以访问编译器，并且可以将我们的代码编译成 JavaScript。

稍后我们将深入研究它以及它的作用，但是现在让我们向我们的项目添加一个配置文件。添加一个配置文件并不是强制性的——但是在很多情况下，拥有它是很有用的，因为它允许我们为编译器定义规则集。

## 使用 tsconfig 配置 TypeScript

`tsconfig`是一个帮助配置 TypeScript 的 JSON 文件。有一个配置文件更好，因为它有助于控制编译器的行为。

要创建配置文件，首先需要创建一个名为`Pokedex`的新目录，并浏览到文件夹的根目录。然后，在终端或 IDE 上打开它，并运行此命令来生成新的 TypeScript 配置文件。

```
 tsc --init 
```

文件生成后，我们现在可以在 IDE 上研究它。

*   `tsconfig.json`

```
{
    "compilerOptions": {
        "target": "es5",
        "module": "commonjs",
        "outDir": "public/js"
        "rootDir": "src",
        "strict": true,
        "esModuleInterop": true
        "forceConsistentCasingInFileNames": true
    },
    "include": ["src"]
} 
```

这个配置文件比你在上面看到的要冗长得多——为了更容易阅读，我删除了注释和未使用的值。也就是说，我们现在可以分解这些值，解释每一个，并看看它做什么。

target:它在编译 TypeScript 代码时指定 ECMAScript 目标版本。这里，我们的目标是`es5`支持所有的浏览器，你可以把它改成 ES6、ES3(如果没有指定目标，这是默认的)、ES2020 等等。

模块:它定义了编译代码的模块。模块可以是常见的 JS、ES2015、ES2020 等。

outDir:它为编译成 JavaScript 的代码指定输出目录。

rootDir:它定义了需要编译的 TypeScript 文件所在的位置。

include:它有助于定义需要编译哪个目录。如果没有这个值，编译器将获取每个`.ts`文件，并将其编译成 JavaScript，即使定义了输出目录。

有了这些，我们现在可以深入到 TypeScript 最重要的部分之一:类型。

## 类型脚本类型

类型提供了一种提高代码质量的方法，而且它们也使代码更容易理解，因为它定义了变量类型。它们是可选的，有助于定义给定变量的值应该是什么。它们还允许编译器在运行前捕捉错误。

TypeScript 有几种类型，如 number、string、boolean、enum、void、null、undefined、any、never、array 和 tuple。我们不会在本指南中看到所有类型，但请记住它们是存在的。

现在，让我们看一些基本类型的例子。

### 基本类型脚本类型

```
let foo: string = "test"
let bar: number = 1
let baz: string[] = ["This", "is", "a", "Test"] 
```

如你所见，我们有三个不同类型的变量。`foo`需要一个字符串，`bar`需要一个数字，`baz`需要一个字符串数组。如果它们接收到声明的类型之外的任何内容，TypeScript 将会抛出一个错误。

也可以这样声明`baz`:`let baz: Array<string> = ["This", "is", "a", "Test"]`。

现在，让我们尝试重新分配这些变量中的一个，并看看 TypeScript 的行为。

```
let foo: string = "test"
foo = 1 
```

```
Type '1' is not assignable to type 'string' 
```

TypeScript 将抛出一个错误，因为我们已经声明了`foo`期望一个字符串作为值。这个错误是在编译时捕获的，这使得 TypeScript 非常有用。

使用 TypeScript，类型可以像上面一样是显式的，但也可以是隐式的。最好显式定义给定值的类型，因为这有助于编译器和下一个继承代码的开发人员。但是也可以用隐式类型注释来声明变量。

```
let foo = "test"
let bar = 1
let baz = ["This", "is", "a", "Test"] 
```

TypeScript 将在这里尝试尽可能多地进行推断，以较少的代码为您提供类型安全。它将获取值，并将其定义为变量的类型。关于错误什么都不会改变。

让我们尝试重新分配这些变量，看看会发生什么。

```
foo = 7
bar = "updated"
baz = [2, true, "a", 10] 
```

TypeScript 将像以前一样捕捉错误，即使变量类型是隐式声明的。

```
Type '7' is not assignable to type 'string'.
Type '"updated"' is not assignable to type 'number'.
Type 'true' is not assignable to type 'string'. 
```

当处理一个具有多种属性的对象时，定义类型会很棘手，也很烦人。但是幸运的是，TypeScript 有助于您理解这个用例。因此，让我们在下一节深入研究 TypeScript 接口和类型别名。

### 接口和类型别名

接口和类型别名帮助我们定义一个类似对象的数据结构。就结构而言，它们看起来是一样的，但请记住它们是不同的。

然而，开发人员的共识是尽可能使用`interface`，因为它在默认的`tslint`规则集中。

现在，让我们在下一节中创建一个接口和一个类型别名来看看它们的作用。

```
interface ITest {
  id: number;
  name?: string;
}

type TestType = {
  id: number,
  name?: string,
}

function myTest(args: ITest): string {
  if (args.name) {
    return `Hello ${args.name}`
  }
  return "Hello Word"
}

myTest({ id: 1 }) 
```

如您所见，接口和类型别名的结构看起来像 JavaScript 对象。他们必须用 TypeScript 定义给定数据的形式。

注意，在这里，我通过添加一个问号(`?`)来使用可选字段`name`。它让我们将属性`name`设置为可选。这意味着如果没有值传递给属性`name`，它将返回`undefined`作为它的值。

接下来，我们使用接口`ITest`作为函数`myTest`接收的参数的类型。和变量一样，函数也可以被定义为返回特定的类型。这里，返回值必须是一个字符串，否则 TypeScript 会抛出一个错误。

到目前为止，我们已经介绍了开始使用 TypeScript 所需的所有基础知识。现在，让我们用它来构建一个包含 HTML 和 CSS 的 Pokedex。

让我们开始吧。

![excited](img/2310c451873272fca68f754df10fa110.png)

## 使用 TypeScript 构建 Pokedex 应用程序

我们将要构建的项目将从 [Pokemon API](https://pokeapi.co/) 获取远程数据，并用 TypeScript 显示每个 Pokemon。

所以，让我们从在文件夹`Pokedex`的根目录下创建三个新文件开始:`index.html`、`style.css`和`src/app.ts`。对于 TypeScript 的配置，我们将使用前面创建的同一个`tsconfig.json`文件。

现在，让我们转到标记部分，向 HTML 文件添加一些内容。

### 利润

*   `index.html`

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />
    <title>TypeScript Pokedex</title>
  </head>
  <body>
    <main>
      <h1>Typed Pokedex</h1>
      <div id="app"></div>
    </main>
    <script src="public/js/app.js"></script>
  </body>
</html> 
```

如你所见，我们有一个相对简单的标记。不过，有两件重要的事情需要保留:

*   将用于使用 TypeScript 追加内容的`div`标记的 id `app`,以及
*   指向`public`文件夹的`script`标签，确切地说是 TypeScript 将在编译时为我们创建的 JavaScript 文件。

此外，CSS 文件有点长，所以我不会涉及它——我不想浪费你的时间，并希望专注于 TypeScript。也就是说，我们现在可以深入其中，开始从 API 获取数据。

### 使用 TypeScript 获取和显示数据

我们通过选择 id `app`开始 TS 部分，它是 div `tag`的 id。

*   `src/app.ts`

```
const container: HTMLElement | any = document.getElementById("app")
const pokemons: number = 100

interface IPokemon {
  id: number;
  name: string;
  image: string;
  type: string;
} 
```

这里，我们有一个还没有涉及的类型注释。这是一种联合类型，它允许给定变量有替代类型。这意味着如果`container`不是类型`HTMLElement`，TypeScript 将再次检查该值是否等于管道(`|`)符号之后的类型，以此类推，因为您可以有多个类型。

接下来，我们有一个接口`IPokemon`,它定义了一个 pokemon 对象的形状，这个对象将在负责显示内容的函数中使用。

*   `src/app.ts`

```
const fetchData = (): void => {
  for (let i = 1; i <= pokemons; i++) {
    getPokemon(i)
  }
}

const getPokemon = async (id: number): Promise<void> => {
  const data: Response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`)
  const pokemon: any = await data.json()
  const pokemonType: string = pokemon.types
    .map((poke: any) => poke.type.name)
    .join(", ")

  const transformedPokemon = {
    id: pokemon.id,
    name: pokemon.name,
    image: `${pokemon.sprites.front_default}`,
    type: pokemonType,
  }

  showPokemon(transformedPokemon)
} 
```

函数`fetchData`允许我们遍历要检索的口袋妖怪的编号，并为每个对象调用带有口袋妖怪编号的`getPokemon`。

获取数据可能需要一些时间，所以我们将使用一个异步函数来返回类型为`void`的`Promise`。最后一点意味着函数不会返回值。

一旦获取了数据，我们现在可以创建一个镜像接口`IPokemon`的新对象`transformedPokemon`，然后将它作为参数传递给`showPokemon()`。

*   `src/app.ts`

```
const showPokemon = (pokemon: IPokemon): void => {
  let output: string = `
        <div class="card">
            <span class="card--id">#${pokemon.id}</span>
            <img class="card--image" src=${pokemon.image} alt=${pokemon.name} />
            <h1 class="card--name">${pokemon.name}</h1>
            <span class="card--details">${pokemon.type}</span>
        </div>
    `
  container.innerHTML += output
}

fetchData() 
```

如你所见，函数`showPokemon`接收类型为`IPokemon`的口袋妖怪对象作为参数，并返回`void`或者准确地说没有值。它只是借助 id `container`(记住，它是`div`标签)将内容添加到 HTML 文件中。

太好了！我们现在已经做了很多，但是仍然缺少一些东西，因为如果你试图在浏览器中启动`index.html`文件，它将什么也不显示。这是因为 TypeScript 需要编译成普通的 JavaScript。那么，让我们在下一节中开始吧。

## 将 TypeScript 编译成 JavaScript

在本教程的前面，我们安装了 TypeScript 编译器，它允许将 TS 代码编译成 JavaScript。为此，您需要浏览到项目的根目录并运行以下命令。

```
 tsc 
```

这个命令将编译每一个带有 JavaScript 扩展名的文件。由于我们有一个`tsconfig`文件，编译器将遵循定义的规则，只编译位于`src`文件夹中的 TS 文件，并将 JS 代码放入`public`目录。

编译器还允许只编译一个文件。

```
 tsc myFile.ts 
```

如果您没有在 TS 文件(`myFile.ts`)后指定名称，编译后的 JS 文件将与 TS 文件同名。

如果您不想在每次更改时都执行该命令，只需添加一个`-w`标志，让编译器继续监视更改，并在需要时重新编译代码。

```
 tsc -w 
```

现在，如果您启动`index.html`文件，您将看到您的 Pokedex 在浏览器中成功呈现。

![Pokedex app preview image](img/8eb4cd4c364b3730a65bae5b5b0a7cd9.png)

太好了！我们现在已经通过使用 HTML 和 CSS 构建 Pokedex 应用程序学习了 TypeScript 的基础知识。

在这里预览完成的项目[或者在这里](https://codesandbox.io/s/typescript-pokedex-yluzs?file=/src/index.ts)找到源代码[。](https://github.com/ibrahima92/pokedex-typescript)

你也可以在我的博客上找到类似的精彩内容，或者在推特上关注我，当我写了新的东西时，你会得到通知。

感谢阅读。

## 资源

下面是一些有助于深入研究 TypeScript 的有用资源。

[打字稿类型](https://www.typescriptlang.org/docs/handbook/basic-types.html)

[TypeScript 编译器选项](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

[打字手册](https://www.typescriptlang.org/docs/handbook/basic-types.html)