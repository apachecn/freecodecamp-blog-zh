# 如何在 React 中使用去抖和节流，并将它们抽象成钩子

> 原文：<https://www.freecodecamp.org/news/debounce-and-throttle-in-react-with-hooks/>

钩子是反应的一个极好的附加物。它们简化了许多逻辑，这些逻辑以前必须用`class`组件拆分成不同的生命周期。

然而，他们确实需要*不同的*、[心智模式，尤其是对第一次尝试的人](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)。

> 我还就这篇文章录制了一个简短的[视频系列](https://www.youtube.com/playlist?list=PLMV09mSPNaQlN92-1Dkz5NDlNgGQJEo75)，你可能会觉得有帮助。

## 谴责和压制

有很多关于去抖和节流的博客文章，所以我不会深入讨论如何写你自己的去抖和节流。为简洁起见，考虑来自洛达什的 [`debounce`](https://lodash.com/docs/4.17.15#debounce) 和 [`throttle`](https://lodash.com/docs/4.17.15#throttle) 。

如果您需要快速复习，两者都接受一个(回调)函数和一个以毫秒计的*延迟*(比如说`x`)，然后两者都返回另一个具有一些特殊行为的函数:

*   `debounce`:返回一个函数，这个函数可以被调用任意次(可能是连续调用)，但只会在从最后一次调用开始等待到`x`毫秒后调用回调**。**
*   `throttle`:返回一个函数，这个函数可以被调用任意次(可能是连续的)，但是每隔`x`毫秒最多调用**一次**

## 使用 case

我们有一个最小的博客编辑器(这里是 [GitHub repo](https://github.com/wtjs/react-debounce-throttle-hooks/) )，我们希望在用户停止输入 1 秒钟后将博客文章保存到数据库中。

> 如果你想看代码的最终版本，你也可以参考这个代码沙箱。

我们的编辑器的最小版本如下所示:

```
import React, { useState } from 'react';
import debounce from 'lodash.debounce';

function App() {
	const [value, setValue] = useState('');
	const [dbValue, saveToDb] = useState(''); // would be an API call normally

	const handleChange = event => {
		setValue(event.target.value);
	};

	return (
		<main>
			<h1>Blog</h1>
			<textarea value={value} onChange={handleChange} rows={5} cols={50} />
			<section className="panels">
				<div>
					<h2>Editor (Client)</h2>
					{value}
				</div>
				<div>
					<h2>Saved (DB)</h2>
					{dbValue}
				</div>
			</section>
		</main>
	);
} 
```

这里，`saveToDb`实际上是对后端的 API 调用。为了简单起见，我将它保存在 state 中，然后呈现为`dbValue`。

因为我们只想在用户停止输入后(1 秒钟后)执行保存操作，所以这应该是*去抖*。

[下面是](https://github.com/wtjs/react-debounce-throttle-hooks/tree/starter)回购和分支的启动代码。

## 创建去抖功能

首先，我们需要一个去抖函数来包装对`saveToDb`的调用:

```
import React, { useState } from 'react';
import debounce from 'lodash.debounce';

function App() {
	const [value, setValue] = useState('');
	const [dbValue, saveToDb] = useState(''); // would be an API call normally

	const handleChange = event => {
		const { value: nextValue } = event.target;
		setValue(nextValue);
		// highlight-starts
		const debouncedSave = debounce(() => saveToDb(nextValue), 1000);
		debouncedSave();
		// highlight-ends
	};

	return <main>{/* Same as before */}</main>;
} 
```

但是，这实际上并不起作用，因为函数`debouncedSave`是在每次调用`handleChange`时创建的。这将导致每次击键都去抖，而不是去抖整个输入值。

## 使用回调

[`useCallback`](https://reactjs.org/docs/hooks-reference.html#usecallback) 通常用于向子组件传递回调时的性能优化。但是我们可以使用它记忆回调函数的约束来确保`debouncedSave`在渲染中引用相同的去抖函数。

> 如果你想了解记忆化的基础，我也在 freeCodeCamp 上写了这篇文章。

这正如预期的那样工作:

```
import React, { useState, useCallback } from 'react';
import debounce from 'lodash.debounce';

function App() {
	const [value, setValue] = useState('');
	const [dbValue, saveToDb] = useState(''); // would be an API call normally

	// highlight-starts
	const debouncedSave = useCallback(
		debounce(nextValue => saveToDb(nextValue), 1000),
		[], // will be created only once initially
	);
	// highlight-ends

	const handleChange = event => {
		const { value: nextValue } = event.target;
		setValue(nextValue);
		// Even though handleChange is created on each render and executed
		// it references the same debouncedSave that was created initially
		debouncedSave(nextValue);
	};

	return <main>{/* Same as before */}</main>;
} 
```

## useRef

[`useRef`](https://reactjs.org/docs/hooks-reference.html#useref) 给出了一个可变对象，它的`current`属性引用了传递的初始值。如果我们不手动更改它，该值将在组件的整个生命周期内保持不变。

这类似于类实例属性(即在`this`上定义方法和属性)。

这也像预期的那样工作:

```
import React, { useState, useRef } from 'react';
import debounce from 'lodash.debounce';

function App() {
	const [value, setValue] = useState('');
	const [dbValue, saveToDb] = useState(''); // would be an API call normally

	// This remains same across renders
	// highlight-starts
	const debouncedSave = useRef(debounce(nextValue => saveToDb(nextValue), 1000))
		.current;
	// highlight-ends

	const handleChange = event => {
		const { value: nextValue } = event.target;
		setValue(nextValue);
		// Even though handleChange is created on each render and executed
		// it references the same debouncedSave that was created initially
		debouncedSave(nextValue);
	};

	return <main>{/* Same as before */}</main>;
} 
```

继续阅读[我的博客](https://divyanshu013.dev/blog/react-debounce-throttle-hooks/)，了解如何将这些概念抽象成定制挂钩，或者查看[视频系列](https://www.youtube.com/playlist?list=PLMV09mSPNaQlN92-1Dkz5NDlNgGQJEo75)。

你也可以在 [Twitter](https://twitter.com/divyanshu013) 上关注我，了解我的最新帖子。我希望这篇文章对你有所帮助。:)