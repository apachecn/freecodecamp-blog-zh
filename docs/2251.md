# 如何用 react-hook-form 创建 React 表单

> 原文：<https://www.freecodecamp.org/news/how-to-build-react-forms/>

包括 React 开发人员在内，没有人喜欢创建和重新创建带有验证的复杂表单。

在 React 中构建表单时，使用一个表单库是很重要的，它提供了许多方便的工具，并且不需要太多代码。

基于实用性和简单性这两个标准，您的应用程序使用的理想的 React 表单库是`react-hook-form`。

让我们看看如何在您自己的项目中使用 react-hook-form 来为您的 react 应用程序构建丰富、功能丰富的表单。

> 想要用 React 创建令人惊叹的项目的完整指南吗？看看 React 训练营。

## 如何安装 react-hook-form

让我们来看一个典型的用例:用户注册我们的应用程序。

对于我们的注册表单，我们将为任何新用户的用户名、密码和电子邮件提供三个输入:

```
import React from "react";

const styles = {
  container: {
    width: "80%",
    margin: "0 auto",
  },
  input: {
    width: "100%",
  },
};

export default function Signup() {
  return (
    <div style={styles.container}>
      <h4>Sign up</h4>
      <form>
        <input placeholder="Username" style={styles.input} />
        <input placeholder="Email" style={styles.input} />
        <input placeholder="Password" style={styles.input} />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
} 
```

一旦 React 项目启动并运行，我们将从安装`react-hook-form`库开始。

```
npm i react-hook-form 
```

## 如何使用 useForm 钩子

要开始使用`react-hook-form`，我们只需要调用`useForm`钩子。

当我们这样做时，我们将获得一个对象，我们将从该对象中析构`register`属性。

`register`是一个函数，我们需要将它作为 ref 连接到每个输入。

```
function App() {
  const { register } = useForm();

  return (
    <div style={styles.container}>
      <h4>Signup</h4>
      <form>
        <input ref={register} placeholder="Username" style={styles.input} />
        <input ref={register} placeholder="Email" style={styles.input} />
        <input ref={register} placeholder="Password" style={styles.input} />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
} 
```

register 函数将接受用户在每个输入中输入的值来验证它。Register 还会将每个值传递给一个函数，这个函数会在提交表单时被调用，我们将在接下来介绍这个函数。

为了让 register 正常工作，我们需要为每个输入提供一个适当名称属性。例如，对于用户名输入，它的名称为“username”。

这样做的原因是，当提交表单时，我们将获得单个对象的所有输入值。每个对象的属性都将根据我们指定的输入名称属性来命名。

```
function App() {
  const { register } = useForm();

  return (
    <div style={styles.container}>
      <h4>My Form</h4>
      <form>
        <input
          name="username"
          ref={register}
          placeholder="Username"
          style={styles.input}
        />
        <input
          name="email"
          ref={register}
          placeholder="Email"
          style={styles.input}
        />
        <input
          name="password"
          ref={register}
          placeholder="Password"
          style={styles.input}
        />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
} 
```

## 如何使用 handleSubmit 提交我们的表单

为了提交表单和接收输入数据，我们将向表单元素添加一个`onSubmit`,并将其连接到一个同名的本地函数:

```
function App() {
  const { register } = useForm();

  function onSubmit() {}

  return (
    <div style={styles.container}>
      <h4>My Form</h4>
      <form onSubmit={onSubmit}>
        <input
          name="username"
          ref={register}
          placeholder="Username"
          style={styles.input}
        />
        <input
          name="email"
          ref={register}
          placeholder="Email"
          style={styles.input}
        />
        <input
          name="password"
          ref={register}
          placeholder="Password"
          style={styles.input}
        />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
} 
```

从`useForm`开始，我们将获取一个名为`handleSubmit`的函数，并将其作为一个高阶函数包装在 onSubmit 上。

`handleSubmit`函数将负责收集我们输入到每个输入中的所有数据，我们将在`onSubmit`中接收这些数据，作为一个名为`data`的对象。

现在，如果我们`console.log(data)`可以看到我们在同名属性的每个输入中输入了什么:

```
function App() {
  const { register, handleSubmit } = useForm();

  function onSubmit(data) {
    console.log(data); 
    // { username: 'test', email: 'test', password: 'test' }
  }

  return (
    <div style={styles.container}>
      <h4>Signup</h4>
      <form onSubmit={handleSubmit(onSubmit)}>
        <input
          name="username"
          ref={register}
          placeholder="Username"
          style={styles.input}
        />
        <input
          name="email"
          ref={register}
          placeholder="Email"
          style={styles.input}
        />
        <input
          name="password"
          ref={register}
          placeholder="Password"
          style={styles.input}
        />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
} 
```

## 注册功能的验证选项

验证我们的表单并为每个输入值添加约束非常简单——我们只需要将信息传递给`register`函数。

`register`接受一个对象，该对象包含许多属性，这些属性将告诉 register 如何验证给定的输入。

第一个属性是`required`。默认情况下，它被设置为 false，但我们可以将其设置为 true，以确保表单在未填写的情况下不会被提交。

我们希望用户名值是必需的，我们希望我们的用户的用户名多于 6 个字符，但少于 24 个字符。

为了应用这个验证，我们可以将`minLength`的约束设置为 6，但是`maxLength`应该是 20:

```
<input
  name="username"
  ref={register({
    required: true,
    minLength: 6,
    maxLength: 20,
  })}
  style={styles.input}
  placeholder="Username"
/> 
```

如果我们使用数字进行输入(假设输入的是人的年龄)，我们将使用属性`min`和`max`，而不是`minLength`和`maxLength`。

接下来，如果愿意，我们可以提供一个正则表达式模式。

如果我们希望用户名只包含大写和小写字符，我们可以使用下面的正则表达式，它允许自定义验证:

```
<input
  name="username"
  ref={register({
    required: true,
    minLength: 6,
    maxLength: 20,
    pattern: /^[A-Za-z]+$/i,
  })}
  style={styles.input}
  placeholder="Username"
/> 
```

最后，还有`validate`，这是一个自定义函数，让我们可以访问输入到输入中的值。`validate`允许我们提供自己的逻辑来确定它是否有效(通过返回布尔值 true 或 false)。

对于此处的电子邮件，我们也希望它是必需的，并且是有效的电子邮件。为了检查这一点，我们可以将输入传递给库`validator`中名为`isEmail`的函数。

如果输入是电子邮件，则返回 true。否则，为假:

```
<input
  name="email"
  ref={register({
    required: true,
    validate: (input) => isEmail(input), // returns true if valid
  })}
  style={styles.input}
  placeholder="Email"
/> 
```

对于密码的`register`函数，我们将把 required 设置为 true，`minlength`设置为 6，并且不设置`maxLength`:

```
<input
  name="password"
  ref={register({
    required: true,
    minLength: 6
  })}
  style={styles.input}
  placeholder="Password"
/>
```

## 如何显示错误

现在，如果表单中的输入无效，我们不会告诉用户有什么问题。我们需要给他们反馈来修正他们提供的值。

当其中一个输入无效时，表单数据仅仅是不被提交(不调用`onSubmit`)。此外，第一个出现错误的输入会被自动聚焦，这不会向用户提供任何关于发生了什么的详细反馈。

我们可以从 useForm 获取一个`errors`对象，而不仅仅是不提交表单。

就像我们在`onSubmit`中得到的数据函数一样，`errors`包含对应于每个输入名称的属性，如果它有错误的话。

对于我们的例子，我们可以给每个输入添加一个条件，如果有错误，我们将把`borderColor`设置为红色。

```
function App() {
  const { register, handleSubmit, errors } = useForm();

  function onSubmit(data) {
    console.log(data);
  }

  return (
    <div style={styles.container}>
      <h4>My Form</h4>
      <form onSubmit={handleSubmit(onSubmit)}>
        <input
          name="username"
          ref={register({
            required: true,
            minLength: 6,
            maxLength: 20,
            pattern: /^[A-Za-z]+$/i,
          })}
          style={{ ...styles.input, borderColor: errors.username && "red" }}
          placeholder="Username"
        />
        <input
          name="email"
          ref={register({
            required: true,
            validate: (input) => isEmail(input),
          })}
          style={{ ...styles.input, borderColor: errors.email && "red" }}
          placeholder="Email"
        />
        <input
          name="password"
          ref={register({
            required: true,
            minLength: 6,
          })}
          style={{ ...styles.input, borderColor: errors.password && "red" }}
          placeholder="Password"
        />
        <button type="submit" disabled={formState.isSubmitting}>
          Submit
        </button>
      </form>
    </div>
  );
} 
```

## 验证模式

您会注意到，默认情况下，只有当我们提交表单时，errors 对象才会更新。默认验证仅在提交表单时执行。

我们可以通过向`useForm`传递一个对象来改变这一点，在这里我们可以将模式设置为我们希望执行验证的时间:`onBlur`、`onChange`或`onSubmit`。

`onBlur`将在用户“模糊”或点击离开输入时进行验证。`onChange`是每当用户输入时，而`onSubmit`是每当表单提交时。

对于我们的表单，让我们选择`onBlur`。

```
const { register, handleSubmit, errors } = useForm({
  mode: "onBlur",
}); 
```

注意，还有其他助手可以手动设置和清除错误(`setError`和`clearError`)。例如，在某些情况下，您希望它在`onSubmit`中创建一个不同的错误或自己清除一个错误，这时就会用到这些。

## 如何用 formState 禁用我们的表单

我们可以得到的最后一个`useForm`钩子的值是`formState`。

它给我们重要的信息，比如某些输入何时被触摸，以及表单何时被提交。

因此，如果您想禁用表单的按钮，以确保表单不会被提交过多的次数，我们可以将 disabled 设置为`formState.isSubmitting`。

每当我们提交表单时，它都会被禁用，直到完成验证并运行 onSubmit 函数。

## 结论

我希望这篇文章向您展示了如何在 React 应用程序中更容易地创建函数表单。

不值得一提的是，`react-hook-form`还有很多我没有在这里介绍的特性。[文档](https://react-hook-form.com)应该涵盖你能想到的任何用例。

如果你有兴趣看我们代码的最终版本，点击 CodeSandbox 链接[就在这里](https://codesandbox.io/s/crazy-leavitt-nf74y)。

## 喜欢这篇文章吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得数百名开发人员已经使用的内部信息，以掌握 React、找到他们梦想的工作并掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*