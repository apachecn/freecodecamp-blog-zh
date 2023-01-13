# 如何创建自己的 Chip-8 仿真器

> 原文：<https://www.freecodecamp.org/news/creating-your-very-own-chip-8-emulator/>

在深入本文之前，我想快速介绍一下什么是模拟器。最简单地说，仿真器是一种软件，它允许一个系统像另一个系统一样运行。

现在模拟器的一个非常流行的用途是模拟旧的视频游戏系统，如任天堂 64、Gamecube 等等。

例如，通过任天堂 64 模拟器，我们可以直接在 Windows 10 电脑上运行任天堂 64 游戏，而不需要实际的主机。在我们的例子中，我们通过使用我们将在本文中创建的仿真器在我们的主机系统上仿真 Chip-8。

学习如何制作自己的仿真器的最简单的方法之一是从一个 Chip-8 仿真器开始。只有 4KB 的内存和 36 条指令，您可以在不到一天的时间内启动并运行自己的 Chip-8 仿真器。您还将获得转到更大、更深入的仿真器所需的知识。

这将是一篇非常深入的长文，希望能弄懂一切。对十六进制、二进制和位运算有基本的了解将是有益的。

每一部分都被我们正在处理的文件分割，并被我们正在处理的函数再次分割，希望这样更容易理解。一旦我们完成了每个文件，我会提供一个完整代码的链接，并附上注释。

对于整篇文章，我们将参考 Cowgod 的 [Chip-8 技术参考文献](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#2.2)，它解释了 Chip-8 的每个细节。

您可以使用任何您想要的语言来制作模拟器，尽管本文将使用 JavaScript。我觉得这是第一次创建仿真器时使用的最简单的语言，因为它提供了对渲染、键盘和声音的开箱即用支持。

最重要的是你了解仿真的过程，所以使用你最熟悉的语言。

如果您决定使用 JavaScript，您将需要运行一个本地 web 服务器进行测试。为此，我使用 Python，它允许您通过运行`python3 -m http.server`在当前文件夹中启动一个 web 服务器。

我们将从创建`index.html`和`style.css`文件开始，然后继续到渲染器、键盘、扬声器，最后是实际的 CPU。我们的项目结构将如下所示:

```
- roms
- scripts
    chip8.js
    cpu.js
    keyboard.js
    renderer.js
    speaker.js
index.html
style.css 
```

## 索引和样式

这两个文件没什么疯狂的，很基础。`index.html`文件只是加载样式，创建一个画布元素，然后加载`chip8.js`文件。

```
<!DOCTYPE html>
<html>
    <head>
        <link rel="stylesheet" href="style.css">
    </head>
    <body>
        <canvas></canvas>

        <script type="module" src="scripts/chip8.js"></script>
    </body>
</html> 
```

`style.css`文件甚至更简单，因为唯一被样式化的是画布，以便更容易识别。

```
canvas {
    border: 2px solid black;
} 
```

在整篇文章中，您不必再接触这两个文件，但是可以随意地以您喜欢的任何方式设计页面。

## renderer.js

我们的渲染器将处理所有与图形相关的事情。它将初始化我们的 canvas 元素，切换显示中的像素，并在画布上呈现这些像素。

```
class Renderer {

}

export default Renderer; 
```

### 建造者(规模)

首要任务是构建我们的渲染器。这个构造函数将接受一个参数`scale`，它将允许我们放大或缩小显示，使像素变大或变小。

```
class Renderer {
    constructor(scale) {

    }
}

export default Renderer; 
```

我们需要在这个构造函数中初始化一些东西。首先是显示器尺寸，Chip-8 的尺寸是 64x32 像素。

```
this.cols = 64;
this.rows = 32; 
```

在现代系统中，这是难以置信的小，很难看到，这就是为什么我们要扩大显示，使其更加用户友好。在我们的构造函数中，我们想要设置比例，抓取画布，获取上下文，并设置画布的宽度和高度。

```
this.scale = scale;

this.canvas = document.querySelector('canvas');
this.ctx = this.canvas.getContext('2d');

this.canvas.width = this.cols * this.scale;
this.canvas.height = this.rows * this.scale; 
```

如你所见，我们使用了`scale`变量来增加画布的宽度和高度。当我们开始渲染屏幕上的像素时，我们将再次使用`scale`。

我们需要添加到构造函数中的最后一项是一个数组，它将作为我们的显示。因为一个 Chip-8 显示器是 64x32 像素，所以我们的数组的大小就是 64 * 32(列*行)，或者 2048。基本上，我们用这个阵列来表示 Chip-8 显示器上的每个像素，开(1)或关(0)。

```
this.display = new Array(this.cols * this.rows); 
```

这将在稍后用于在我们的画布中正确的位置渲染像素。

### 设定像素(x，y)

每当我们的仿真器打开或关闭一个像素时，显示数组将被修改来表示它。

说到打开或关闭像素，让我们创建一个负责这个的函数。我们将调用函数`setPixel`，它将接受一个`x`和`y`位置作为参数。

```
setPixel(x, y) {

} 
```

根据技术参考，如果一个像素位于显示器的边界之外，它应该绕到对面，所以我们需要考虑这一点。

```
if (x > this.cols) {
    x -= this.cols;
} else if (x < 0) {
    x += this.cols;
}

if (y > this.rows) {
    y -= this.rows;
} else if (y < 0) {
    y += this.rows;
} 
```

考虑到这一点，我们可以正确地计算像素在显示器上的位置。

```
let pixelLoc = x + (y * this.cols); 
```

如果您不熟悉位运算，下面这段代码可能会令人困惑。根据技术参考，子画面被 xor 到显示器上:

```
this.display[pixelLoc] ^= 1; 
```

这一行所做的就是切换`pixelLoc`处的值(0 到 1 或 1 到 0)。值 1 表示应该绘制像素，值 0 表示应该擦除像素。从这里开始，我们只返回一个值来表示一个像素是否被擦除。

这一部分，尤其重要，当我们到了 CPU，写不同的指令的时候。

```
return !this.display[pixelLoc]; 
```

如果返回 true，则像素被擦除。如果返回 false，则没有删除任何内容。当我们看到使用这个函数的指令时，它会更有意义。

### 清除()

这个函数通过重新初始化来完全清除我们的`display`数组。

```
clear() {
    this.display = new Array(this.cols * this.rows);
} 
```

### 渲染()

`render`函数负责将`display`数组中的像素渲染到屏幕上。对于这个项目，它将每秒运行 60 次。

```
render() {
    // Clears the display every render cycle. Typical for a render loop.
    this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);

    // Loop through our display array
    for (let i = 0; i < this.cols * this.rows; i++) {
        // Grabs the x position of the pixel based off of `i`
        let x = (i % this.cols) * this.scale;

        // Grabs the y position of the pixel based off of `i`
        let y = Math.floor(i / this.cols) * this.scale;

        // If the value at this.display[i] == 1, then draw a pixel.
        if (this.display[i]) {
            // Set the pixel color to black
            this.ctx.fillStyle = '#000';

            // Place a pixel at position (x, y) with a width and height of scale
            this.ctx.fillRect(x, y, this.scale, this.scale);
        }
    }
} 
```

### testRender()

出于测试目的，让我们创建一个在屏幕上绘制几个像素的函数。

```
testRender() {
    this.setPixel(0, 0);
    this.setPixel(5, 2);
} 
```

[完整 renderer.js 代码](https://github.com/Erigitic/chip8-emulator/blob/master/scripts/renderer.js)

## chip8.js

现在我们有了渲染器，我们需要在我们的`chip8.js`文件中初始化它。

```
import Renderer from './renderer.js';

const renderer = new Renderer(10); 
```

根据技术参考，从这里我们需要创建一个以 60hz 或每秒 60 帧运行的循环。就像我们的渲染函数一样，这不是特定于 Chip-8 的，可以稍加修改以适用于几乎任何其他项目。

```
let loop;

let fps = 60, fpsInterval, startTime, now, then, elapsed;

function init() {
    fpsInterval = 1000 / fps;
    then = Date.now();
    startTime = then;

    // TESTING CODE. REMOVE WHEN DONE TESTING.
    renderer.testRender();
    renderer.render();
    // END TESTING CODE

    loop = requestAnimationFrame(step);
}

function step() {
    now = Date.now();
    elapsed = now - then;

    if (elapsed > fpsInterval) {
        // Cycle the CPU. We'll come back to this later and fill it out.
    }

    loop = requestAnimationFrame(step);
}

init(); 
```

如果您启动 web 服务器并在 web 浏览器中加载页面，您应该会看到屏幕上绘制了两个像素。如果你愿意，试试天平，找到最适合你的东西。

## 键盘. js

[键盘参考](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#2.3)

技术参考告诉我们，Chip-8 使用 16 键十六进制键盘，布局如下:

|  |  |  |  |
| --- | --- | --- | --- |
| one | Two | three | C |
| four | five | six | D |
| seven | eight | nine | E |
| A | Zero | B | F |

为了在现代系统上实现这一点，我们必须将键盘上的一个键映射到这些 Chip-8 键中的每一个。我们将在构造函数中这样做，还有其他一些事情。

### 构造函数()

```
class Keyboard {
    constructor() {
        this.KEYMAP = {
            49: 0x1, // 1
            50: 0x2, // 2
            51: 0x3, // 3
            52: 0xc, // 4
            81: 0x4, // Q
            87: 0x5, // W
            69: 0x6, // E
            82: 0xD, // R
            65: 0x7, // A
            83: 0x8, // S
            68: 0x9, // D
            70: 0xE, // F
            90: 0xA, // Z
            88: 0x0, // X
            67: 0xB, // C
            86: 0xF  // V
        }

        this.keysPressed = [];

        // Some Chip-8 instructions require waiting for the next keypress. We initialize this function elsewhere when needed.
        this.onNextKeyPress = null;

        window.addEventListener('keydown', this.onKeyDown.bind(this), false);
        window.addEventListener('keyup', this.onKeyUp.bind(this), false);
    }
}

export default Keyboard; 
```

在构造函数中，我们创建了一个键映射，将键盘上的键映射到 Chip-8 键盘上的键。除此之外，我们还有一个跟踪按键的数组、一个空变量(我们将在后面讨论)和几个处理键盘输入的事件监听器。

### iskeypressed(密钥代码)

我们需要一种方法来检查某个键是否被按下。这将简单地检查指定 Chip-8 `keyCode`的`keysPressed`数组。

```
isKeyPressed(keyCode) {
    return this.keysPressed[keyCode];
} 
```

### onKeyDown(事件)

在我们的构造函数中，我们添加了一个`keydown`事件监听器，它将在被触发时调用这个函数。

```
onKeyDown(event) {
    let key = this.KEYMAP[event.which];
    this.keysPressed[key] = true;

    // Make sure onNextKeyPress is initialized and the pressed key is actually mapped to a Chip-8 key
    if (this.onNextKeyPress !== null && key) {
        this.onNextKeyPress(parseInt(key));
        this.onNextKeyPress = null;
    }
} 
```

我们在这里所做的就是将按下的键添加到我们的`keysPressed`数组中，如果它被初始化并且有效的键被按下，就运行`onNextKeyPress`。

我们来谈谈 if 语句。Chip-8 指令之一(`Fx0A`)在继续执行之前等待按键。我们将让`Fx0A`指令初始化`onNextKeyPress`函数，这将允许我们模仿等待直到下一次按键的行为。一旦我们写了这个指令，我会更详细地解释它，因为当你看到它时，它会更有意义。

### onKeyUp(事件)

我们还有一个事件监听器来处理`keyup`事件，当该事件被触发时，这个函数将被调用。

```
onKeyUp(event) {
    let key = this.KEYMAP[event.which];
    this.keysPressed[key] = false;
} 
```

[全键盘. js 代码](https://github.com/Erigitic/chip8-emulator/blob/master/scripts/keyboard.js)

## chip8.js

创建了键盘类后，我们可以回到`chip8.js`并连接键盘。

```
import Renderer from './renderer.js';
import Keyboard from './keyboard.js'; // NEW

const renderer = new Renderer(10);
const keyboard = new Keyboard(); // NEW 
```

## 扬声器. js

现在让我们制造一些声音。这个文件相当简单，包括创建一个简单的声音和开始/停止它。

### 构造器

```
class Speaker {
    constructor() {
        const AudioContext = window.AudioContext || window.webkitAudioContext;

        this.audioCtx = new AudioContext();

        // Create a gain, which will allow us to control the volume
        this.gain = this.audioCtx.createGain();
        this.finish = this.audioCtx.destination;

        // Connect the gain to the audio context
        this.gain.connect(this.finish);
    }
}

export default Speaker; 
```

我们在这里所做的就是创建一个`AudioContext`并将一个增益连接到它，这样我们就可以控制音量。我不会在本教程中添加音量控制，但如果你想自己添加它，你只需使用以下:

```
// Mute the audio
this.gain.setValueAtTime(0, this.audioCtx.currentTime); 
```

```
// Unmute the audio
this.gain.setValueAtTime(1, this.audioCtx.currentTime); 
```

### 播放(频率)

该功能顾名思义:以所需的频率播放声音。

```
play(frequency) {
    if (this.audioCtx && !this.oscillator) {
        this.oscillator = this.audioCtx.createOscillator();

        // Set the frequency
        this.oscillator.frequency.setValueAtTime(frequency || 440, this.audioCtx.currentTime);

        // Square wave
        this.oscillator.type = 'square';

        // Connect the gain and start the sound
        this.oscillator.connect(this.gain);
        this.oscillator.start();
    }
} 
```

我们正在创造一个振荡器，它将播放我们的声音。我们设置它的频率，类型，连接到增益，然后最后播放声音。这里没有什么太疯狂的。

### 停止()

我们最终不得不停止声音，这样它就不会不停地播放。

```
stop() {
    if (this.oscillator) {
        this.oscillator.stop();
        this.oscillator.disconnect();
        this.oscillator = null;
    }
} 
```

所有这些都是停止声音，断开连接，并将其设置为空，以便可以在`play()`中重新初始化。

[完整的 speaker.js 代码](https://github.com/Erigitic/chip8-emulator/blob/master/scripts/speaker.js)

## chip8.js

我们现在可以将扬声器连接到我们的主`chip8.js`文件。

```
import Renderer from './renderer.js';
import Keyboard from './keyboard.js';
import Speaker from './speaker.js'; // NEW

const renderer = new Renderer(10);
const keyboard = new Keyboard();
const speaker = new Speaker(); // NEW 
```

## cpu.js

现在我们进入实际的 Chip-8 仿真器。这是事情变得有点疯狂的地方，但我会尽我所能，以一种希望能解释所有事情的方式来解释一切。

### 构造器(渲染器、键盘、扬声器)

我们需要在构造函数中初始化一些特定于 Chip-8 的变量，以及一些其他变量。我们将查看技术参考的第[节第 2](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#2.0) 部分，以确定我们的 Chip-8 仿真器的规格。

以下是芯片 8 的规格:

*   4KB (4096 字节)内存
*   16 个 8 位寄存器
*   存储存储器地址的 16 位寄存器(`this.i`)
*   两个定时器。一个是延时，一个是声音。
*   存储当前正在执行的地址的程序计数器
*   表示堆栈的数组

我们还有一个变量，存储模拟器是否暂停，以及模拟器的执行速度。

```
class CPU {
    constructor(renderer, keyboard, speaker) {
        this.renderer = renderer;
        this.keyboard = keyboard;
        this.speaker = speaker;

        // 4KB (4096 bytes) of memory
        this.memory = new Uint8Array(4096);

        // 16 8-bit registers
        this.v = new Uint8Array(16);

        // Stores memory addresses. Set this to 0 since we aren't storing anything at initialization.
        this.i = 0;

        // Timers
        this.delayTimer = 0;
        this.soundTimer = 0;

        // Program counter. Stores the currently executing address.
        this.pc = 0x200;

        // Don't initialize this with a size in order to avoid empty results.
        this.stack = new Array();

        // Some instructions require pausing, such as Fx0A.
        this.paused = false;

        this.speed = 10;
    }
}

export default CPU; 
```

### loadSpritesIntoMemory()

对于此功能，我们将参考技术参考的第 2.4 节中的[。](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#2.4)

Chip-8 使用 16 . 5 字节的子画面。这些精灵只是从 0 到 f 的十六进制数字。你可以在第 2.4 节看到所有的精灵，以及它们的二进制和十六进制值。

在我们的代码中，我们只是将技术参考提供的精灵的十六进制值存储在一个数组中。如果您不想手工输入它们，请随意将数组复制并粘贴到您的项目中。

该参考声明这些子画面存储在存储器的解释器部分(0x000 至 0x1FFF)。让我们来看看这个函数的代码，看看这是如何实现的。

```
loadSpritesIntoMemory() {
    // Array of hex values for each sprite. Each sprite is 5 bytes.
    // The technical reference provides us with each one of these values.
    const sprites = [
        0xF0, 0x90, 0x90, 0x90, 0xF0, // 0
        0x20, 0x60, 0x20, 0x20, 0x70, // 1
        0xF0, 0x10, 0xF0, 0x80, 0xF0, // 2
        0xF0, 0x10, 0xF0, 0x10, 0xF0, // 3
        0x90, 0x90, 0xF0, 0x10, 0x10, // 4
        0xF0, 0x80, 0xF0, 0x10, 0xF0, // 5
        0xF0, 0x80, 0xF0, 0x90, 0xF0, // 6
        0xF0, 0x10, 0x20, 0x40, 0x40, // 7
        0xF0, 0x90, 0xF0, 0x90, 0xF0, // 8
        0xF0, 0x90, 0xF0, 0x10, 0xF0, // 9
        0xF0, 0x90, 0xF0, 0x90, 0x90, // A
        0xE0, 0x90, 0xE0, 0x90, 0xE0, // B
        0xF0, 0x80, 0x80, 0x80, 0xF0, // C
        0xE0, 0x90, 0x90, 0x90, 0xE0, // D
        0xF0, 0x80, 0xF0, 0x80, 0xF0, // E
        0xF0, 0x80, 0xF0, 0x80, 0x80  // F
    ];

    // According to the technical reference, sprites are stored in the interpreter section of memory starting at hex 0x000
    for (let i = 0; i < sprites.length; i++) {
        this.memory[i] = sprites[i];
    }
} 
```

我们所做的就是遍历`sprites`数组中的每个字节，并从十六进制`0x000`开始将它存储在内存中。

### loadprogramintomemory(程式)

为了运行只读存储器，我们必须把它们装入内存。这比听起来容易多了。我们所要做的就是遍历 ROM/程序的内容，并将其存储在内存中。技术参考明确地告诉我们“大多数芯片 8 程序从位置 0x200 开始”。因此，当我们将 ROM 加载到内存中时，我们从`0x200`开始，并从那里递增。

```
loadProgramIntoMemory(program) {
    for (let loc = 0; loc < program.length; loc++) {
        this.memory[0x200 + loc] = program[loc];
    }
} 
```

### loadRom(romName)

现在我们有了一种将 ROM 加载到内存中的方法，但是我们必须先从文件系统中获取 ROM，然后才能将它加载到内存中。要做到这一点，你必须有一个只读存储器。我在 [GitHub repo](https://github.com/Erigitic/chip8-emulator/tree/master/roms) 中包含了一些，供你下载并放到你项目的`roms`文件夹中。

JavaScript 提供了一种发出 HTTP 请求和检索文件的方法。我在下面的代码中添加了注释来解释发生了什么:

```
loadRom(romName) {
    var request = new XMLHttpRequest;
    var self = this;

    // Handles the response received from sending (request.send()) our request
    request.onload = function() {
        // If the request response has content
        if (request.response) {
            // Store the contents of the response in an 8-bit array
            let program = new Uint8Array(request.response);

            // Load the ROM/program into memory
            self.loadProgramIntoMemory(program);
        }
    }

    // Initialize a GET request to retrieve the ROM from our roms folder
    request.open('GET', 'roms/' + romName);
    request.responseType = 'arraybuffer';

    // Send the GET request
    request.send();
} 
```

从这里，我们可以开始处理指令执行的 CPU 周期，以及其他一些事情。

### 周期()

我认为如果你能看到每次 CPU 循环时发生了什么，理解一切会更容易。这是我们将在`chip8.js`中的`step`函数中调用的函数，如果你还记得的话，这个函数每秒执行 60 次。我们将一点一点地研究这个函数。

此时，在`cycle`中被调用的函数还没有被创建。我们将很快创建它们。

我们的`cycle`函数中的第一段代码是一个处理指令执行的 for 循环。这就是我们的`speed`变量发挥作用的地方。该值越高，每个周期执行的指令越多。

```
cycle() {
    for (let i = 0; i < this.speed; i++) {

    }
} 
```

我们还想记住，指令应该只在模拟器运行时执行。

```
cycle() {
    for (let i = 0; i < this.speed; i++) {
        if (!this.paused) {

        }
    }
} 
```

如果你看一下[第 3.1 节](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#3.1)，你可以看到所有不同的指令及其操作码。举几个例子，它们看起来有点像`00E0`或`9xy0`。所以我们的工作是从内存中获取操作码，并将其传递给另一个处理该指令执行的函数。我们先来看看代码，然后我来解释一下:

```
cycle() {
    for (let i = 0; i < this.speed; i++) {
        if (!this.paused) {
            let opcode = (this.memory[this.pc] << 8 | this.memory[this.pc + 1]);
            this.executeInstruction(opcode);
        }
    }
} 
```

我们来具体看看这一行:`let opcode = (this.memory[this.pc] << 8 | this.memory[this.pc + 1]);`。对于那些不太熟悉位运算的人来说，这可能会很吓人。

首先，每条指令都是 16 位(2 字节)长( [3.0](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#3.0) ，但我们的内存是由 8 位(1 字节)片组成的。这意味着我们必须组合两块内存才能得到完整的操作码。这就是为什么我们在上面的代码行中有`this.pc`和`this.pc + 1`。我们只是抓住了操作码的两半。

但是你不能把两个 1 字节的值组合起来得到一个 2 字节的值。为了正确地做到这一点，我们需要将第一块内存`this.memory[this.pc]`左移 8 位，使其长度为 2 个字节。用最基本的术语来说，这将把两个零，或者更准确地说是十六进制值`0x00`添加到我们的 1 字节值的右边，使其成为 2 字节。

例如，将十六进制`0x11`左移 8 位将得到十六进制`0x1100`。从那里，我们将它与第二个内存块`this.memory[this.pc + 1])`按位“或”(`|`)。

这里有一个循序渐进的例子，可以帮助你更好地理解这一切意味着什么。

让我们假设几个值，每个值的大小为 1 字节:

`this.memory[this.pc] = PC = 0x10`


将`PC`左移 8 位(1 字节),使其变为 2 字节:

`PC = 0x1000`

按位 OR `PC`和`PC + 1`:

`PC | PC + 1 = 0x10F0`

或者

`0x1000 | 0xF0 = 0x10F0`

最后，我们希望在 are emulator 运行(而不是暂停)时更新计时器，播放声音，并在屏幕上呈现精灵:

```
cycle() {
    for (let i = 0; i < this.speed; i++) {
        if (!this.paused) {
            let opcode = (this.memory[this.pc] << 8 | this.memory[this.pc + 1]);
            this.executeInstruction(opcode);
        }
    }

    if (!this.paused) {
        this.updateTimers();
    }

    this.playSound();
    this.renderer.render();
} 
```

这个函数在某种程度上是我们仿真器的大脑。它处理指令的执行，更新计时器，播放声音，并在屏幕上呈现内容。

我们还没有创建任何这些函数，但是当我们创建它们的时候，看到 CPU 如何循环通过所有的东西将有希望使这些函数变得更有意义。

### 更新计时器()

让我们转到第 2.5 节的[部分，设置定时器和声音的逻辑。](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#2.5)

每个定时器、延迟和声音以 60Hz 的速率递减 1。换句话说，每隔 60 帧，我们的计时器就会递减 1。

```
updateTimers() {
    if (this.delayTimer > 0) {
        this.delayTimer -= 1;
    }

    if (this.soundTimer > 0) {
        this.soundTimer -= 1;
    }
} 
```

延迟计时器用于记录特定事件发生的时间。这个定时器只在两个指令中使用:一个用于设置它的值，另一个用于读取它的值，如果某个值存在，则转移到另一个指令。

声音计时器控制声音的长度。只要`this.soundTimer`的值大于零，声音就会继续播放。当声音计时器到达零时，声音将停止。这使我们进入下一个函数，我们将做的正是这一点。

### 播放声音()

重申一下，只要声音计时器大于零，我们就想播放一个声音。我们将使用我们之前创建的`Speaker`类中的`play`函数来播放频率为 440 的声音。

```
playSound() {
    if (this.soundTimer > 0) {
        this.speaker.play(440);
    } else {
        this.speaker.stop();
    }
} 
```

### 执行指令(操作码)

对于整个功能，我们将参考技术参考的第 3.0 节[和第 3.1 节](http://devernay.free.fr/hacks/chip8/C8TECH10.HTM#3.0)。

这是这个文件需要的最后一个函数，这个函数很长。我们必须写出所有 36 条 Chip-8 指令的逻辑。令人欣慰的是，大多数指令只需要几行代码。

需要注意的第一条信息是，所有指令的长度都是 2 字节。所以每次我们执行一条指令，或者运行这个函数，我们必须把程序计数器(`this.pc`)加 2，这样 CPU 就知道下一条指令在哪里了。

```
executeInstruction(opcode) {
    // Increment the program counter to prepare it for the next instruction.
    // Each instruction is 2 bytes long, so increment it by 2.
    this.pc += 2;
} 
```

现在让我们来看看 3.0 节的这一部分:

```
In these listings, the following variables are used:

nnn or addr - A 12-bit value, the lowest 12 bits of the instruction
n or nibble - A 4-bit value, the lowest 4 bits of the instruction
x - A 4-bit value, the lower 4 bits of the high byte of the instruction
y - A 4-bit value, the upper 4 bits of the low byte of the instruction
kk or byte - An 8-bit value, the lowest 8 bits of the instruction 
```

为了避免重复代码，我们应该为`x`和`y`值创建变量，因为它们几乎被每个指令使用。上面列出的其他变量没有被充分使用来保证每次都计算它们的值。

这两个值各为 4 位(又名。半字节或半字节)。`x`值位于高字节的低 4 位，而`y`位于低字节的高 4 位。

例如，如果我们有一个指令`0x5460`，高字节将是`0x54`，低字节将是`0x60`。高字节的低 4 位或半字节是`0x4`，低字节的高 4 位是`0x6`。所以，在这个例子中，`x = 0x4`和`y= 0x6`。

了解了所有这些之后，让我们编写获取`x`和`y`值的代码。

```
executeInstruction(opcode) {
    this.pc += 2;

    // We only need the 2nd nibble, so grab the value of the 2nd nibble
    // and shift it right 8 bits to get rid of everything but that 2nd nibble.
    let x = (opcode & 0x0F00) >> 8;

    // We only need the 3rd nibble, so grab the value of the 3rd nibble
    // and shift it right 4 bits to get rid of everything but that 3rd nibble.
    let y = (opcode & 0x00F0) >> 4;
} 
```

为了解释这一点，让我们再次假设我们有一个指令`0x5460`。如果我们用十六进制值`0x0F00`对指令进行`&`(位与)运算，我们将以`0x0400`结束。将这 8 位右移，我们得到`0x04`或`0x4`。`y`也是一样。我们`&`带有十六进制值`0x00F0`的指令，得到`0x0060`。将这 4 位右移，我们以`0x006`或`0x6`结束。

现在是有趣的部分，编写所有 36 条指令的逻辑。对于每一条指令，在你写代码之前，我强烈建议你阅读一下技术参考中的指令，因为你会更好地理解它。

我将为您提供您将使用的空 switch 语句，因为它很长。

```
switch (opcode & 0xF000) {
    case 0x0000:
        switch (opcode) {
            case 0x00E0:
                break;
            case 0x00EE:
                break;
        }

        break;
    case 0x1000:
        break;
    case 0x2000:
        break;
    case 0x3000:
        break;
    case 0x4000:
        break;
    case 0x5000:
        break;
    case 0x6000:
        break;
    case 0x7000:
        break;
    case 0x8000:
        switch (opcode & 0xF) {
            case 0x0:
                break;
            case 0x1:
                break;
            case 0x2:
                break;
            case 0x3:
                break;
            case 0x4:
                break;
            case 0x5:
                break;
            case 0x6:
                break;
            case 0x7:
                break;
            case 0xE:
                break;
        }

        break;
    case 0x9000:
        break;
    case 0xA000:
        break;
    case 0xB000:
        break;
    case 0xC000:
        break;
    case 0xD000:
        break;
    case 0xE000:
        switch (opcode & 0xFF) {
            case 0x9E:
                break;
            case 0xA1:
                break;
        }

        break;
    case 0xF000:
        switch (opcode & 0xFF) {
            case 0x07:
                break;
            case 0x0A:
                break;
            case 0x15:
                break;
            case 0x18:
                break;
            case 0x1E:
                break;
            case 0x29:
                break;
            case 0x33:
                break;
            case 0x55:
                break;
            case 0x65:
                break;
        }

        break;

    default:
        throw new Error('Unknown opcode ' + opcode);
} 
```

正如您从`switch (opcode & 0xF000)`中看到的，我们正在获取操作码最高有效字节的高 4 位。如果你看一下技术参考中的不同指令，你会注意到我们可以通过第一个半字节缩小不同操作码的范围。

#### 0nnn - SYS addr

这个操作码可以忽略。

#### 00E0 - CLS

清空显示屏。

```
case 0x00E0:
    this.renderer.clear();
    break; 
```

#### 00EE - RET

弹出`stack`数组中的最后一个元素并存储在`this.pc`中。这将使我们从子程序返回。

```
case 0x00EE:
    this.pc = this.stack.pop();
    break; 
```

《技术参考》指出，这条指令还“从堆栈指针中减去 1”。堆栈指针用于指向堆栈的最顶层。但是由于我们的`stack`数组，我们不需要担心栈顶在哪里，因为它是由数组处理的。所以对于其余的指令，如果它说了一些关于堆栈指针的东西，你可以放心地忽略它。

#### 1nnn - JP 地址

将程序计数器设置为存储在`nnn`中的值。

```
case 0x1000:
    this.pc = (opcode & 0xFFF);
    break; 
```

`0xFFF`抓取`nnn`的值。所以`0x1426 & 0xFFF`会给我们`0x426`，然后我们把它存储在`this.pc`。

#### 2nnn -呼叫地址

为此，技术参考说我们必须增加堆栈指针，使其指向`this.pc`的当前值。同样，我们在项目中没有使用堆栈指针，因为我们的`stack`数组会为我们处理它。因此，我们没有递增，而是将`this.pc`压入堆栈，这将给出相同的结果。就像使用操作码`1nnn`一样，我们获取`nnn`的值并将其存储在`this.pc`中。

```
case 0x2000:
    this.stack.push(this.pc);
    this.pc = (opcode & 0xFFF);
    break; 
```

#### 3xkk - SE Vx，字节

这就是我们上面计算的`x`值发挥作用的地方。

该指令将存储在`x`寄存器(`Vx`)中的值与`kk`的值进行比较。注意`V`表示一个寄存器，它后面的值，在本例中是`x`，是寄存器号。如果它们相等，我们将程序计数器加 2，有效地跳过下一条指令。

```
case 0x3000:
    if (this.v[x] === (opcode & 0xFF)) {
        this.pc += 2;
    }
    break; 
```

if 语句的`opcode & 0xFF`部分只是抓取操作码的最后一个字节。这是操作码的`kk`部分。

#### 4xkk - SNE Vx，字节

这条指令与`3xkk`非常相似，但是如果`Vx`和`kk`不相等，则跳过下一条指令。

```
case 0x4000:
    if (this.v[x] !== (opcode & 0xFF)) {
        this.pc += 2;
    }
    break; 
```

#### 5xy0 - SE Vx，Vy

现在我们同时使用了`x`和`y`。与前两条指令一样，如果满足条件，这条指令将跳过下一条指令。在这条指令的情况下，如果`Vx`等于`Vy`，我们跳过下一条指令。

```
case 0x5000:
    if (this.v[x] === this.v[y]) {
        this.pc += 2;
    }
    break; 
```

#### 6xkk - LD Vx，字节

该指令将把`Vx`的值设置为`kk`的值。

```
case 0x6000:
    this.v[x] = (opcode & 0xFF);
    break; 
```

#### 7xkk -添加 Vx，字节

该指令将`kk`加到`Vx`上。

```
case 0x7000:
    this.v[x] += (opcode & 0xFF);
    break; 
```

#### 8xy0 - LD Vx，Vy

在讨论这个指令之前，我想解释一下`switch (opcode & 0xF)`是怎么回事。为什么是开关中的开关？

这背后的原因是我们有一些属于`case 0x8000:`的不同指令。如果你看一下技术参考中的那些指令，你会注意到每条指令的最后半字节都以值`0-7`或`E`结束。

我们使用这个开关来获取最后一个半字节，然后为每个字节创建一个案例来正确处理它。在整个主开关语句中，我们还要重复几次。

解释完之后，让我们继续看说明书。这并不疯狂，只是将`Vx`的值设置为等于`Vy`的值。

```
case 0x0:
    this.v[x] = this.v[y];
    break; 
```

#### 8xy1 -或 Vx，Vy

将`Vx`设置为`Vx OR Vy`的值。

```
case 0x1:
    this.v[x] |= this.v[y];
    break; 
```

#### 8xy2 -和 Vx，Vy

将`Vx`设置为等于`Vx AND Vy`的值。

```
case 0x2:
    this.v[x] &= this.v[y];
    break; 
```

#### 8xy3 - XOR Vx，Vy

将`Vx`设置为等于`Vx XOR Vy`的值。

```
case 0x3:
    this.v[x] ^= this.v[y];
    break; 
```

#### 8xy4 -添加 Vx，Vy

该指令设置`Vx`至`Vx + Vy`。听起来很容易，但还有更多。如果我们阅读《技术参考》中对此说明的描述，会发现如下内容:

> 如果结果大于 8 位(即> 255)，VF 设置为 1，否则为 0。仅保留结果的最低 8 位，并存储在 Vx 中。

```
case 0x4:
    let sum = (this.v[x] += this.v[y]);

    this.v[0xF] = 0;

    if (sum > 0xFF) {
        this.v[0xF] = 1;
    }

    this.v[x] = sum;
    break; 
```

逐行执行，我们首先将`this.v[y]`与`this.v[x]`相加，并将该值存储在变量`sum`中。从那里我们将`this.v[0xF]`或`VF`设置为 0。我们这样做是为了避免在下一行使用 if-else 语句。如果总和大于 255 或十六进制数`0xFF`，我们将`VF`设置为 1。最后，我们将`this.v[x]`或`Vx`设置为总和。

你可能想知道我们如何确保“只保留结果的最低 8 位，并存储在 Vx 中”。由于`this.v`是一个`Uint8Array`，任何超过 8 位的值都会自动获取较低的、最右边的 8 位，并存储在数组中。因此，我们不需要对它做任何特殊的处理。

让我给你举个例子，让你更好地理解这一点。假设我们试图将十进制 257 放入`this.v`数组。在二进制中，该值是 9 位值`100000001`。当我们试图将 9 位值存储到数组中时，它将只取低 8 位。这意味着二进制的`00000001`，即十进制的 1，将被存储在`this.v`中。

#### 8xy5 - SUB Vx，Vy

这条指令从`Vx`中减去`Vy`。就像上一条指令中处理溢出一样，这次我们必须处理下溢。

```
case 0x5:
    this.v[0xF] = 0;

    if (this.v[x] > this.v[y]) {
        this.v[0xF] = 1;
    }

    this.v[x] -= this.v[y];
    break; 
```

同样，由于我们使用了`Uint8Array`，我们不需要做任何事情来处理下溢，因为它已经为我们处理好了。所以-1 会变成 255，-2 会变成 254，依此类推。

#### 8xy6 - SHR Vx {，Vy}

```
case 0x6:
    this.v[0xF] = (this.v[x] & 0x1);

    this.v[x] >>= 1;
    break; 
```

这条线`this.v[0xF] = (this.v[x] & 0x1);`将确定最低有效位，并相应地设置`VF`。

如果你看看它的二进制表示，这就更容易理解了。如果二进制中的`Vx`是`1001`，`VF`将被设置为 1，因为最低有效位是 1。如果`Vx`为`1000`，则`VF`将被设置为 0。

#### 8xy7 - SUBN Vx，Vy

```
case 0x7:
    this.v[0xF] = 0;

    if (this.v[y] > this.v[x]) {
        this.v[0xF] = 1;
    }

    this.v[x] = this.v[y] - this.v[x];
    break; 
```

该指令从`Vy`中减去`Vx`，并将结果存储在`Vx`中。如果`Vy`大于`Vx`，我们需要在`VF`中存储 1，否则存储 0。

#### 8 xy-SHL VX {，Vy}

该指令不仅将`Vx`左移 1，还根据条件是否满足将`VF`设置为 0 或 1。

```
case 0xE:
    this.v[0xF] = (this.v[x] & 0x80);
    this.v[x] <<= 1;
    break; 
```

第一行代码`this.v[0xF] = (this.v[x] & 0x80);`抓取`Vx`的最高有效位，并将其存储在`VF`中。为了解释这一点，我们有一个 8 位寄存器，`Vx`，我们想要得到最高有效位，或者最左边的位。为此我们需要用二进制的`10000000`或十六进制的`0x80`与`Vx`进行 AND 运算。这将完成将`VF`设置为正确的值。

之后，我们简单地将`Vx`向左移动 1，乘以 2。

#### 9xy0 - SNE Vx，Vy

如果`Vx`和`Vy`不相等，该指令简单地将程序计数器增加 2。

```
case 0x9000:
    if (this.v[x] !== this.v[y]) {
        this.pc += 2;
    }
    break; 
```

#### ann-id I，地址

将寄存器`i`的值设置为`nnn`。如果操作码是`0xA740`，那么`(opcode & 0xFFF)`将返回`0x740`。

```
case 0xA000:
    this.i = (opcode & 0xFFF);
    break; 
```

#### Bnnn - JP V0，地址

将程序计数器(`this.pc`)设置为`nnn`加上寄存器 0 的值(`V0`)。

```
case 0xB000:
    this.pc = (opcode & 0xFFF) + this.v[0];
    break; 
```

#### CXC-rnd VX，字节

```
case 0xC000:
    let rand = Math.floor(Math.random() * 0xFF);

    this.v[x] = rand & (opcode & 0xFF);
    break; 
```

生成一个 0-255 范围内的随机数，然后与操作码的最低字节相加。例如，如果操作码是`0xB849`，那么`(opcode & 0xFF)`将返回`0x49`。

#### Dxyn - DRW Vx，Vy，半字节

这是一个大的！该指令处理屏幕上像素的绘制和擦除。我将向您提供所有代码，并逐行解释。

```
case 0xD000:
    let width = 8;
    let height = (opcode & 0xF);

    this.v[0xF] = 0;

    for (let row = 0; row < height; row++) {
        let sprite = this.memory[this.i + row];

        for (let col = 0; col < width; col++) {
            // If the bit (sprite) is not 0, render/erase the pixel
            if ((sprite & 0x80) > 0) {
                // If setPixel returns 1, which means a pixel was erased, set VF to 1
                if (this.renderer.setPixel(this.v[x] + col, this.v[y] + row)) {
                    this.v[0xF] = 1;
                }
            }

            // Shift the sprite left 1\. This will move the next next col/bit of the sprite into the first position.
            // Ex. 10010000 << 1 will become 0010000
            sprite <<= 1;
        }
    }

    break; 
```

我们将`width`变量设置为 8，因为每个 sprite 都是 8 像素宽，所以硬编码这个值是安全的。接下来，我们将`height`设置为操作码的最后一个半字节(`n`)的值。如果我们的操作码是`0xD235`，`height`将被设置为 5。从那里我们将`VF`设置为 0，如果必要的话，稍后如果像素被擦除，它将被设置为 1。

现在进入 for 循环。请记住，精灵看起来像这样:

```
11110000
10010000
10010000
10010000
11110000 
```

我们的代码逐行(第一个`for`循环)，然后逐位或逐列(第二个`for`循环)通过 sprite。

这段代码，`let sprite = this.memory[this.i + row];`，占用了 8 位内存，或者一行 sprite，存储在`this.i + row`。技术参考声明，当我们从内存中读取精灵时，我们从存储在`I`或`this.i`中的地址开始。

在我们的第二个`for`循环中，我们有一个`if`语句，它抓取最左边的位并检查它是否大于 0。

值为 0 表示 sprite 在该位置没有像素，所以我们不需要担心绘制或擦除它。如果值为 1，我们继续执行另一个 If 语句，检查`setPixel`的返回值。让我们看看传递给该函数的值。

我们的`setPixel`调用看起来是这样的:`this.renderer.setPixel(this.v[x] + col, this.v[y] + row)`。根据技术参考，`x`和`y`位置分别位于`Vx`和`Vy`。将`col`的数字加到`Vx`上，将`row`的数字加到`Vy`上，就可以得到想要的位置来绘制/擦除一个像素。

如果`setPixel`返回 1，我们删除像素并将`VF`设置为 1。如果它返回 0，我们什么也不做，保持`VF`的值等于 0。

最后，我们将精灵左移 1 位。这让我们可以浏览精灵的每一个部分。

例如，`sprite`当前设置为`10010000`，左移后变为`0010000`。从那里，我们可以通过内部`for`循环的另一次迭代来决定是否绘制一个像素。继续这个过程，直到我们的精灵结束。

#### ex 9e-PSP VX

这个非常简单，如果按下存储在`Vx`中的键，就跳过下一条指令，将程序计数器加 2。

```
case 0x9E:
    if (this.keyboard.isKeyPressed(this.v[x])) {
        this.pc += 2;
    }
    break; 
```

#### ExA1 - SKNP Vx

这与前面的指令相反。如果未按下指定的键，则跳过下一条指令。

```
case 0xA1:
    if (!this.keyboard.isKeyPressed(this.v[x])) {
        this.pc += 2;
    }
    break; 
```

#### Fx07 - LD Vx，DT

另一个简单的。我们只是将`Vx`设置为存储在`delayTimer`中的值。

```
case 0x07:
    this.v[x] = this.delayTimer;
    break; 
```

#### Fx0A - LD Vx，K

看一下技术参考，这条指令暂停模拟器，直到按下一个键。下面是它的代码:

```
case 0x0A:
    this.paused = true;

    this.keyboard.onNextKeyPress = function(key) {
        this.v[x] = key;
        this.paused = false;
    }.bind(this);
    break; 
```

我们首先将`paused`设置为 true，以便暂停模拟器。然后，如果你记得在我们的`keyboard.js`文件中，我们将`onNextKeyPress`设置为空，这是我们初始化它的地方。随着`onNextKeyPress`函数的初始化，下一次`keydown`事件被触发时，我们的`keyboard.js`文件中的以下代码将会运行:

```
// keyboard.js
if (this.onNextKeyPress !== null && key) {
    this.onNextKeyPress(parseInt(key));
    this.onNextKeyPress = null;
} 
```

从那里，我们将`Vx`设置为按键的键码，最后通过将`paused`设置为 false 来启动模拟器。

### Fx15 - LD DT，Vx

该指令只是将延迟定时器的值设置为寄存器`Vx`中存储的值。

```
case 0x15:
    this.delayTimer = this.v[x];
    break; 
```

#### Fx18 - LD ST，Vx

该指令与 Fx15 非常相似，但将声音定时器设置为`Vx`而不是延迟定时器。

```
case 0x18:
    this.soundTimer = this.v[x];
    break; 
```

#### Fx1E -添加 I，Vx

将`Vx`加到`I`上。

```
case 0x1E:
    this.i += this.v[x];
    break; 
```

#### Fx29 - LD F，Vx - ADD I，Vx

对于这一个，我们将`I`设置为`Vx`处精灵的位置。它被乘以 5，因为每个精灵有 5 个字节长。

```
case 0x29:
    this.i = this.v[x] * 5;
    break; 
```

#### Fx33 - LD B，Vx

这条指令将从寄存器`Vx`中抓取百位数、十位数和一位数，并将它们分别存储在寄存器`I`、`I+1`和`I+2`中。

```
case 0x33:
    // Get the hundreds digit and place it in I.
    this.memory[this.i] = parseInt(this.v[x] / 100);

    // Get tens digit and place it in I+1\. Gets a value between 0 and 99,
    // then divides by 10 to give us a value between 0 and 9.
    this.memory[this.i + 1] = parseInt((this.v[x] % 100) / 10);

    // Get the value of the ones (last) digit and place it in I+2.
    this.memory[this.i + 2] = parseInt(this.v[x] % 10);
    break; 
```

#### Fx55 - LD [I]，Vx

在这条指令中，我们循环通过寄存器`V0`到`Vx`，并从`I`开始将其值存储在内存中。

```
case 0x55:
    for (let registerIndex = 0; registerIndex <= x; registerIndex++) {
        this.memory[this.i + registerIndex] = this.v[registerIndex];
    }
    break; 
```

#### Fx65 - LD Vx，[I]

现在进行最后一项指令。这个做的和`Fx55`相反。它从`I`开始从存储器中读取值，并将其存储在寄存器`V0`到`Vx`中。

```
case 0x65:
    for (let registerIndex = 0; registerIndex <= x; registerIndex++) {
        this.v[registerIndex] = this.memory[this.i + registerIndex];
    }
    break; 
```

## chip8.js

随着我们的 CPU 类的创建，让我们通过加载 ROM 并循环我们的 CPU 来完成我们的`chip8.js`文件。我们需要导入`cpu.js`并初始化一个 CPU 对象:

```
import Renderer from './renderer.js';
import Keyboard from './keyboard.js';
import Speaker from './speaker.js';
import CPU from './cpu.js'; // NEW

const renderer = new Renderer(10);
const keyboard = new Keyboard();
const speaker = new Speaker();
const cpu = new CPU(renderer, keyboard, speaker); // NEW 
```

我们的`init`功能变成了:

```
function init() {
    fpsInterval = 1000 / fps;
    then = Date.now();
    startTime = then;

    cpu.loadSpritesIntoMemory(); // NEW
    cpu.loadRom('BLITZ'); // NEW
    loop = requestAnimationFrame(step);
} 
```

当我们的模拟器初始化时，我们将把精灵加载到内存中，并加载 rom。现在我们只需要循环 CPU:

```
function step() {
    now = Date.now();
    elapsed = now - then;

    if (elapsed > fpsInterval) {
        cpu.cycle(); // NEW
    }

    loop = requestAnimationFrame(step);
} 
```

完成后，我们现在应该有一个工作的 Chip8 仿真器了。

## 结论

前阵子开始这个项目，被它迷住了。模拟器的创建总是让我感兴趣，但对我来说没有意义。直到我了解了 Chip-8 以及它与更先进的系统相比的简单性。

当我完成这个模拟器的时候，我知道我必须通过提供一个深入的，一步一步的指南来和其他人分享它。我获得的知识，希望你也获得了，将毫无疑问地在其他地方被证明是有用的。

总之，我希望你喜欢这篇文章，并学到了一些东西。我的目的是尽可能详细和简单地解释一切。

不管怎样，如果还有什么事情让你困惑，或者你有什么问题，请随时在 [Twitter](https://twitter.com/ericgrandt) 上告诉我，或者在 [GitHub repo](https://github.com/Erigitic/chip8-emulator/issues) 上发表问题，因为我很乐意帮助你。

我想告诉您一些可以添加到您的 Chip-8 仿真器中的功能:

*   音频控制(静音、改变频率、改变波形(正弦、三角波)等)
*   从用户界面改变渲染比例和模拟器速度的能力
*   暂停和取消暂停
*   保存和加载保存的能力
*   ROM 选择