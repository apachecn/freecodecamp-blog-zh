# 如何用 p5js 搭建画图 app

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-drawing-app-with-p5js-9b8d16e9364a/>

[每周编码挑战](https://www.florin-pop.com/blog/2019/03/weekly-coding-challenge/)第 5 周的**主题**是:

### 创建绘图应用程序

这是我们在#weeklyCodingChallenge 计划中构建的第一个应用程序。到目前为止，我们已经建立了较小的项目，所以如果你问我，这是非常令人兴奋的！？

在本文中，我们将使用绘图库 p5js 来构建一个[绘图应用程序](https://codepen.io/FlorinPop17/full/VNYyZQ):

点击这里查看代码笔:

[https://codepen.io/FlorinPop17/embed/preview/VNYyZQ?height=300&slug-hash=VNYyZQ&default-tabs=css,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/VNYyZQ?height=300&slug-hash=VNYyZQ&default-tabs=css,result&host=https://codepen.io)

如果你想了解更多关于 p5js 和它的作用，可以访问他们的[官网](http://p5js.org/)。基本上，我使用它是因为它通过提供清晰的 API 在浏览器的[画布](https://www.w3schools.com/html/html5_canvas.asp)元素上工作得很好。

### HTML

正如你在上面的例子中注意到的，在屏幕的左侧有一个`.sidebar`。我们将在里面放置我们的“工具”——一个`color`选择器、一个`weight`选择器和`clear`按钮(垃圾桶图标):

```
<div class="sidebar">
    <ul>
        <li>
            <label for="color">Color:</label>
            <input type="color" id="color" />
        </li>
        <li>
            <label for="weight">Stroke:</label>
            <input type="number" id="weight" min="2" max="200" value="3" />
        </li>
        <li>
            <button id="clear"><i class="fa fa-trash"></i></button>
        </li>
    </ul>
</div>
```

### CSS

使用 CSS 我们将把`.sidebar`和它里面的所有东西移到左边。我们将样式化一点，使它看起来更好(没有花哨，基本的 CSS):

```
.sidebar {
    background-color: #333;
    box-shadow: 0px 0px 10px rgba(30, 30, 30, 0.7);
    color: #fff;
    position: absolute;
    left: 0;
    top: 0;
    height: 100vh;
    padding: 5px;
    z-index: 1000;
}

.sidebar ul {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    flex-direction: column;
    list-style-type: none;
    padding: 0;
    margin: 0;
    height: 100%;
}

.sidebar ul li {
    padding: 5px 0;
}

.sidebar input,
.sidebar button {
    text-align: center;
    width: 45px;
}

.sidebar li:last-of-type {
    margin-top: auto;
}

.sidebar button {
    background-color: transparent;
    border: none;
    color: #fff;
    font-size: 20px;
}

.sidebar label {
    display: block;
    font-size: 12px;
    margin-bottom: 3px;
}
```

现在是**重要的**部分…

### JS / P5JS

您可能已经注意到，我们还没有在 HTML 中添加`canvas`元素，因为 p5js 会为我们创建它。

我们将从 [p5js](http://p5js.org/) 库中使用两个重要的函数:

*   [setup](http://p5js.org/reference/#/p5/setup) —程序启动时调用一次。它用于定义初始环境属性，如屏幕大小和背景颜色。
*   [绘制](http://p5js.org/reference/#/p5/draw)——在`setup()`之后直接调用。`draw()`函数持续执行包含在其块中的代码行。

```
function setup() {
    // create a canvas which is full width and height
    createCanvas(window.innerWidth, window.innerHeight);

    // Add a white background to the canvas
    background(255);
}

function draw() {}
```

在继续前进之前，让我们停下来，看看我们想要实现什么。

所以，基本上，我们想给`canvas`添加一个`mousepressed`事件监听器，只要`mouseIsPressed`在里面，它就会开始‘画’一个形状。

我们将创建一个点数组，使用 [beginShape](http://p5js.org/reference/#/p5/beginShape) 和 [endShape](http://p5js.org/reference/#/p5/endShape) 方法在画布中绘制这个形状，我们将用它来创建一个`path`(或一个形状)。该形状将通过连接一系列顶点来构建(更多信息见[顶点](http://p5js.org/reference/#/p5/vertex))。

因为我们希望这个形状每次都被*重新绘制*，我们将把这段代码放在`draw`方法中:

```
const path = [];

function draw() {
    // disabled filling geometry - p5js function
    noFill();

    if (mouseIsPressed) {
        // Store the location of the mouse
        const point = {
            x: mouseX,
            y: mouseY
        };
        path.push(point);
    }

    beginShape();
    path.forEach(point => {
        // create a vertex at the specified location
        vertex(point.x, point.y);
    });
    endShape();
}
```

如您所见，p5js 有一个[mouse pressed](http://p5js.org/reference/#/p5/mouseIsPressed)标志，我们可以用它来检测鼠标按钮何时被按下。

到目前为止，一切看起来都不错，但是有一个大问题。一旦释放鼠标按钮，我们试图绘制另一个形状，前一个形状的最后一个点将连接到新形状的第一个点。这肯定不是我们想要的，所以我们需要稍微改变一下我们的方法。

我们将创建一个`pathsarray`并将所有的`paths`存储在其中，而不是一个点数组(路径数组)。基本上，我们会有一个包含点的双数组。此外，为此，我们将需要在鼠标仍被按下时保持对`currentPath`的跟踪。一旦再次按下鼠标按钮，我们将重置该数组。迷惑？？让我们看看代码，我敢打赌它会变得更加清晰:

```
const paths = [];
let currentPath = [];

function draw() {
    noFill();

    if (mouseIsPressed) {
        const point = {
            x: mouseX,
            y: mouseY
        };
        // Adding the point to the `currentPath` array
        currentPath.push(point);
    }

    // Looping over all the paths and drawing all the points inside them
    paths.forEach(path => {
        beginShape();
        path.forEach(point => {
            stroke(point.color);
            strokeWeight(point.weight);
            vertex(point.x, point.y);
        });
        endShape();
    });
}

// When the mouse is pressed, this even will fire
function mousePressed() {
    // Clean up the currentPath
    currentPath = [];

    // Push the path inside the `paths` array
    paths.push(currentPath);
}
```

我还在上面的代码中添加了一些注释，请务必查看。

每按一次鼠标键就调用一次 [mousePressed](http://p5js.org/reference/#/p5/mousePressed) *函数* — p5js stuff！？

太好了！现在我们可以在画布上绘制单独的形状了！？

最后要做的事情是*将我们在 HTML 中创建的那些按钮*连接起来，并使用其中的值来设置形状的样式:

```
const colorInput = document.getElementById('color');
const weight = document.getElementById('weight');
const clear = document.getElementById('clear');

function draw() {
    noFill();

    if (mouseIsPressed) {
        const point = {
            x: mouseX,
            y: mouseY,
            // storing the color and weights provided by the inputs for each point
            color: colorInput.value,
            weight: weight.value
        };
        currentPath.push(point);
    }

    paths.forEach(path => {
        beginShape();
        path.forEach(point => {
            // using the color and the weight to style the stroke
            stroke(point.color);
            strokeWeight(point.weight);
            vertex(point.x, point.y);
        });
        endShape();
    });
}

clear.addEventListener('click', () => {
    // Remove all the paths
    paths.splice(0);

    // Clear the background
    background(255);
});
```

这样，我们就完成了我们的小应用程序！耶！？

### 整个 JS 代码

```
const colorInput = document.getElementById('color');
const weight = document.getElementById('weight');
const clear = document.getElementById('clear');
const paths = [];
let currentPath = [];

function setup() {
    createCanvas(window.innerWidth, window.innerHeight);
    background(255);
}

function draw() {
    noFill();

    if (mouseIsPressed) {
        const point = {
            x: mouseX,
            y: mouseY,
            color: colorInput.value,
            weight: weight.value
        };
        currentPath.push(point);
    }

    paths.forEach(path => {
        beginShape();
        path.forEach(point => {
            stroke(point.color);
            strokeWeight(point.weight);
            vertex(point.x, point.y);
        });
        endShape();
    });
}

function mousePressed() {
    currentPath = [];
    paths.push(currentPath);
}

clear.addEventListener('click', () => {
    paths.splice(0);
    background(255);
});
```

另外，确保在导入这个`js`文件之前，也在 html 中导入了`p5js`文件。

### 结论

我希望你喜欢我们制作的这个绘图应用程序。有很多功能可以添加到这个应用程序中，我挑战你，让你的创造性思维提出新的想法！？

如果您可以将绘图保存为图像(`.png`或`.jpg`)会怎么样？？(可以用 p5js 库来实现)。

截至目前，我们只检查了`mouse`事件。也许你可以通过计算出`touch`事件，让它在移动设备上也能工作？天空是这个应用程序可以添加的功能数量的极限！

我很想看看你要建什么！给我发推特 [@florinpop1705](https://twitter.com/florinpop1705) 分享你的创作！

你可能也喜欢每周编码挑战计划中的其他挑战。点击查看[。](https://www.florin-pop.com/blog/2019/03/weekly-coding-challenge/)

下次见！编码快乐！？

*最初发表于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/04/drawing-app-built-with-p5js/)。*