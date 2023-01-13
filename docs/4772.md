# 如何使用 React & MomentJS 创建倒计时组件

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-countdown-component-using-react-momentjs-4717edc4ac3/>

最近我不得不为我的另一个项目创建一个倒计时，我认为它也可以成为一个很好的教程。所以在这篇文章中，我们将使用 React 和一点点`SVG`来创建这个组件。？

您可以在这个 [Codepen 示例](https://codepen.io/FlorinPop17/pen/YbpwyG)中找到最终结果:

[https://codepen.io/FlorinPop17/embed/preview/YbpwyG?height=300&slug-hash=YbpwyG&default-tabs=js,result&host=https://codepen.io](https://codepen.io/FlorinPop17/embed/preview/YbpwyG?height=300&slug-hash=YbpwyG&default-tabs=js,result&host=https://codepen.io)

首先我们将创建倒计时功能，然后我们将研究如何使用`SVG`和一些奇特的功能来创建动画弧线。？

### 创建倒计时功能

为此，我们将使用 [MomentJS](https://momentjs.com/) 库，它将帮助我们*解析、验证、操作、*和*显示*日期和时间。

基本上我们需要的是两次约会:

*   当前日期或`now`
*   最终日期还是`then`

当我们有这两个日期时，我们可以使用`moment`从`then`中减去`now`，我们将得到剩余的时间(或`countdown`值)。

对于`then`日期，我们需要传递两个字符串:

*   一个是包含我们想要计数的最终日期的`timeTillDate`字符串(例如: **05 26 2019，6:00 am** )
*   第二个是`moment`使用的`timeFormat`字符串，用于验证时间格式(在我们的例子中是: **MM DD YYYY，h:mm a**

您可以在[文档](https://momentjs.com/docs/#/parsing/string/)中找到更多关于解析字符串和格式化它们的信息。

让我们看看这在代码中是什么样子的:

```
import moment from 'moment';

const then = moment(timeTillDate, timeFormat);
const now = moment();
const countdown = moment(then - now);
```

**注意**:React 组件内部会提供`timeTillDate`、`timeFormat`值。现在我们用它们作为例子。

从`countdown`对象中，我们可以得到我们想要在组件中显示的所有值- `days`、`hours`、`minutes`和`seconds`，直到我们到达`then`时间。

```
import moment from 'moment';

const then = moment(timeTillDate, timeFormat);
const now = moment();
const countdown = moment(then - now);
const days = countdown.format('D');
const hours = countdown.format('HH');
const minutes = countdown.format('mm');
const seconds = countdown.format('ss');
```

稍后，我们将在 JS `interval`中添加这段代码，每秒钟都会调用这段代码，但在此之前，让我们为它设置 react 组件。

### 倒计时组件

为此，我们将创建一个基于*类*的组件，因为我们需要访问组件的`state`，因为我们将在其中保存这 4 个值(`days`、`hours`、`minutes`、`seconds`)。默认情况下，这些值为`undefined`。

```
import React from 'react';

class Countdown extends React.Component {
    state = {
        days: undefined,
        hours: undefined,
        minutes: undefined,
        seconds: undefined
    };

    render() {
        const { days, hours, minutes, seconds } = this.state;

        return (
            <div>
                <h1>Countdown</h1>
                <div className="countdown-wrapper">
                    <div className="countdown-item">
                        {days}
                        <span>days</span>
                    </div>
                    <div className="countdown-item">
                        {hours}
                        <span>hours</span>
                    </div>
                    <div className="countdown-item">
                        {minutes}
                        <span>minutes</span>
                    </div>
                    <div className="countdown-item">
                        {seconds}
                        <span>seconds</span>
                    </div>
                </div>
            </div>
        );
    }
}
```

接下来，让我们创建每秒运行一次的`interval`，并将值保存在组件的`state`中。我们将在`componentDidMount`生命周期方法中完成这个`interval`。我们将`clear`生命周期方法中的间隔，因为我们不想在组件从 DOM 中移除后保持它运行。

```
import React from 'react';
import moment from 'moment';

class Countdown extends React.Component {
    state = {
        days: undefined,
        hours: undefined,
        minutes: undefined,
        seconds: undefined
    };

    componentDidMount() {
        this.interval = setInterval(() => {
            const { timeTillDate, timeFormat } = this.props;
            const then = moment(timeTillDate, timeFormat);
            const now = moment();
            const countdown = moment(then - now);
            const days = countdown.format('D');
            const hours = countdown.format('HH');
            const minutes = countdown.format('mm');
            const seconds = countdown.format('ss');
            this.setState({ days, hours, minutes, seconds });
        }, 1000);
    }

    componentWillUnmount() {
        if (this.interval) {
            clearInterval(this.interval);
        }
    }

    render() {
        const { days, hours, minutes, seconds } = this.state;

        return (
            <div>
                <h1>Countdown</h1>
                <div className="countdown-wrapper">
                    <div className="countdown-item">
                        {days}
                        <span>days</span>
                    </div>
                    <div className="countdown-item">
                        {hours}
                        <span>hours</span>
                    </div>
                    <div className="countdown-item">
                        {minutes}
                        <span>minutes</span>
                    </div>
                    <div className="countdown-item">
                        {seconds}
                        <span>seconds</span>
                    </div>
                </div>
            </div>
        );
    }
}
```

### CSS

我们现在已经启动并运行了倒计时功能，所以让我们稍微设计一下:

```
@import url('https://fonts.googleapis.com/css?family=Lato');

* {
    box-sizing: border-box;
}

body {
    font-family: 'Lato', sans-serif;
}

h1 {
    letter-spacing: 2px;
    text-align: center;
    text-transform: uppercase;
}

.countdown-wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
}

.countdown-item {
    color: #111;
    font-size: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    line-height: 30px;
    margin: 10px;
    padding-top: 10px;
    position: relative;
    width: 100px;
    height: 100px;
}

.countdown-item span {
    color: #333;
    font-size: 12px;
    font-weight: 600;
    text-transform: uppercase;
}
```

CSS 中没有任何花哨的东西；我们使用`flexbox`来定位包装器中的项目。

最后，让我们创建一个`SVG`弧线，它将围绕着我们倒计时中的每一项。

### SVGCircle 组件

在此之前，我们需要几个函数来创建可定制的`SVG`弧。我在 [StackOverflow](https://stackoverflow.com/questions/5736398/how-to-calculate-the-svg-path-for-an-arc-of-a-circle) 上找到了这些。要了解更多信息，你应该去那里阅读详细的功能解释。

```
function polarToCartesian(centerX, centerY, radius, angleInDegrees) {
    var angleInRadians = ((angleInDegrees - 90) * Math.PI) / 180.0;

    return {
        x: centerX + radius * Math.cos(angleInRadians),
        y: centerY + radius * Math.sin(angleInRadians)
    };
}

function describeArc(x, y, radius, startAngle, endAngle) {
    var start = polarToCartesian(x, y, radius, endAngle);
    var end = polarToCartesian(x, y, radius, startAngle);
    var largeArcFlag = endAngle - startAngle <= 180 ? '0' : '1';
    var d = [
        'M',
        start.x,
        start.y,
        'A',
        radius,
        radius,
        0,
        largeArcFlag,
        0,
        end.x,
        end.y
    ].join(' ');

    return d;
}
```

基本上，上面的函数通过提供一组值来计算应该如何绘制圆弧:起点和终点，半径和角度。

回到我们的 React 组件:我们将创建`svg`并且我们将在它里面有一个`path`标签，它将通过给它一个`radius`属性来绘制弧线(`d`道具)。`describeArc`函数中的其他 4 个值是固定的，因为我们不想修改它，我们正在对它进行定制，以便在我们的例子中看起来更好。

```
const SVGCircle = ({ radius }) => (
    <svg className="countdown-svg">
        <path
            fill="none"
            stroke="#333"
            stroke-width="4"
            d={describeArc(50, 50, 48, 0, radius)}
        />
    </svg>
);
```

我们还需要一点 CSS 来将它放置在`.countdown-item`中(参见这个组件在最终结果部分的位置):

```
.countdown-svg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100px;
    height: 100px;
}
```

在将这个组件添加到`Countdown`组件之前，我们需要将我们拥有的值(`days`、`hours`、`minutes`和`seconds`)转换成它们对应的半径值。

为此，我们需要另一个简单的函数，将一个范围内的数字(在我们的例子中是日期值)映射到另一个范围内的数字(在我们的例子中是半径)。该功能也来自 [StackOverflow](https://stackoverflow.com/questions/10756313/javascript-jquery-map-a-range-of-numbers-to-another-range-of-numbers) :

```
function mapNumber(number, in_min, in_max, out_min, out_max) {
    return (
        ((number - in_min) * (out_max - out_min)) / (in_max - in_min) + out_min
    );
}
```

### 最后的结果

最后，让我们在每个`.countdown-item`中添加新的`SVGCircle`组件，并将所有内容放在一起:

```
import React from 'react';
import moment from 'moment';

class Countdown extends React.Component {
    state = {
        days: undefined,
        hours: undefined,
        minutes: undefined,
        seconds: undefined
    };

    componentDidMount() {
        this.interval = setInterval(() => {
            const { timeTillDate, timeFormat } = this.props;
            const then = moment(timeTillDate, timeFormat);
            const now = moment();
            const countdown = moment(then - now);
            const days = countdown.format('D');
            const hours = countdown.format('HH');
            const minutes = countdown.format('mm');
            const seconds = countdown.format('ss');
            this.setState({ days, hours, minutes, seconds });
        }, 1000);
    }

    componentWillUnmount() {
        if (this.interval) {
            clearInterval(this.interval);
        }
    }

    render() {
        const { days, hours, minutes, seconds } = this.state;

        // Mapping the date values to radius values
        const daysRadius = mapNumber(days, 30, 0, 0, 360);
        const hoursRadius = mapNumber(hours, 24, 0, 0, 360);
        const minutesRadius = mapNumber(minutes, 60, 0, 0, 360);
        const secondsRadius = mapNumber(seconds, 60, 0, 0, 360);

        if (!seconds) {
            return null;
        }

        return (
            <div>
                <h1>Countdown</h1>
                <div className="countdown-wrapper">
                    {days && (
                        <div className="countdown-item">
                            <SVGCircle radius={daysRadius} />
                            {days}
                            <span>days</span>
                        </div>
                    )}
                    {hours && (
                        <div className="countdown-item">
                            <SVGCircle radius={hoursRadius} />
                            {hours}
                            <span>hours</span>
                        </div>
                    )}
                    {minutes && (
                        <div className="countdown-item">
                            <SVGCircle radius={minutesRadius} />
                            {minutes}
                            <span>minutes</span>
                        </div>
                    )}
                    {seconds && (
                        <div className="countdown-item">
                            <SVGCircle radius={secondsRadius} />
                            {seconds}
                            <span>seconds</span>
                        </div>
                    )}
                </div>
            </div>
        );
    }
}

const SVGCircle = ({ radius }) => (
    <svg className="countdown-svg">
        <path
            fill="none"
            stroke="#333"
            stroke-width="4"
            d={describeArc(50, 50, 48, 0, radius)}
        />
    </svg>
);

// From StackOverflow: https://stackoverflow.com/questions/5736398/how-to-calculate-the-svg-path-for-an-arc-of-a-circle

function polarToCartesian(centerX, centerY, radius, angleInDegrees) {
    var angleInRadians = ((angleInDegrees - 90) * Math.PI) / 180.0;

    return {
        x: centerX + radius * Math.cos(angleInRadians),
        y: centerY + radius * Math.sin(angleInRadians)
    };
}

function describeArc(x, y, radius, startAngle, endAngle) {
    var start = polarToCartesian(x, y, radius, endAngle);
    var end = polarToCartesian(x, y, radius, startAngle);
    var largeArcFlag = endAngle - startAngle <= 180 ? '0' : '1';
    var d = [
        'M',
        start.x,
        start.y,
        'A',
        radius,
        radius,
        0,
        largeArcFlag,
        0,
        end.x,
        end.y
    ].join(' ');

    return d;
}

// From StackOverflow: https://stackoverflow.com/questions/10756313/javascript-jquery-map-a-range-of-numbers-to-another-range-of-numbers

function mapNumber(number, in_min, in_max, out_min, out_max) {
    return (
        ((number - in_min) * (out_max - out_min)) / (in_max - in_min) + out_min
    );
}
```

现在你所要做的就是使用`Countdown`组件，给它传递两个道具(`timeTillDate`和`timeFormat`)，你就成功了？：

```
<Countdown 
    timeTillDate="05 26 2019, 6:00 am" 
    timeFormat="MM DD YYYY, h:mm a" 
/>
```

### 结论

React 是一个有趣的小项目，不是吗？？

当我构建这个的时候，我学到了更多关于如何使用`momentjs`库和`svg` s 来绘制弧线的知识。

如果你对本教程有任何疑问，请告诉我。

编码快乐！？

*原贴于[www.florin-pop.com](https://www.florin-pop.com/blog/2019/05/countdown-built-with-react/)*