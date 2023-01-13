# 如何将 Rails 实例变量传递给 Vue 组件

> 原文：<https://www.freecodecamp.org/news/how-to-pass-rails-instance-variables-into-vue-components-7fed2a14babf/>

作者加雷斯·富勒

![O8jLrVZ6GSHmqVATENFr3aMLxOaQdr8qfGs-](img/0f0f3291a5d03713cb2de56e8ec95625.png)

# 如何将 Rails 实例变量传递给 Vue 组件

我目前正在使用一个遗留的 Rails 应用程序。我们正在慢慢地将前端从 Rails 视图过渡到 Vue 应用程序。

作为这一过渡的一部分，我们希望将一些元素转变为全球 Vue 组件。我们希望能够在现有的 Rails 视图和 Vue 应用程序中使用这些组件，而不必定制每个组件来处理这两种情况。

在我分享我对此问题的解决方案之前，这里有一个我们希望能够在两种场景(Rails 视图和 Vue 应用程序)中使用的单文件 Vue 组件示例:

```
// Payments.vue
```

```
<template lang="html">  <div id="payments>    <div class="payment" v-for="payment in payments>      {{ payment.amount }}    </div>  </div></template>
```

```
<script>export default {  name: 'payments'
```

```
 props: {    payments: { type: Array }  }}</script>
```

```
<style lang="scss" scoped></style>
```

在 Vue 应用中，这是一个简单易用的组件，对吗？例如:

```
// app/javascript/App.vue
```

```
<template lang="html">  <div id="app>    <payments :payments="payments" />  </div></template>
```

```
<script>import Payments from './Payments.vue'
```

```
export default {  name: 'app',
```

```
 components: { Payments },
```

```
 data () {    return {      payments: [        { amount: 123.00 },        { amount: 124.00 }      ]    }  }</script>
```

```
<style lang="scss" scoped></style>
```

但是如何在 Rails 视图中使用它呢？

### 解决办法

因此，在 Rails 中使用 *Payments.vue* 组件的解决方案如下所示:

```
// app/views/payments/index.haml
```

```
.global-comp  = content_tag 'comp-wrapper', nil, data: { component: 'payments', props: { payments: @payments } }.to_json
```

让我们来分解这个元素。

`.global-comp`是一个 div(带有“global-comp”类)，用于挂载一个超级简单的 Vue 实例。这个 Vue 实例给了我们一个包装器组件来使用，名为 *CompWrapper.vue* (我们一会儿就会知道 CompWrapper 是做什么用的)。

下面是挂载到`.global-comp`的 Vue 实例:

```
// app/javascript/packs/global_comp.js
```

```
import Vue from 'vue/dist/vue.esm'import CompWrapper from './CompWrapper'
```

```
document.addEventListener('DOMContentLoaded', () => {  const app = new Vue({    components: { CompWrapper }  }).$mount('.global-comp')})
```

所有这些都是为了让组件( *CompWrapper.vue* )在一个 div 中对我们可用，div 的类是`global-comp`。

如果你在 Rails 上使用 [Webpacker](https://github.com/rails/webpacker) ,你需要在你的布局中在结束 body 标签之前的某个地方包含这个包。例如:

```
// app/views/layouts/application.haml
```

```
...
```

```
= javascript_pack_tag "global_comp"
```

#### 包装器.视图

这是有趣的部分。 *CompWrapper.vue* 允许我们通过:

1.  我们要使用的组件的名称，例如，“付款”
2.  我们要传递给它的道具

这个包装器组件的全部目的是允许我们将 Rails 实例变量像`@payments`一样作为道具传递到我们的组件中，而不必在每个组件中处理这些变量，像 *Payments.vue.*

所以这里是 *CompWrapper.vue* :

```
// app/javascript/CompWrapper.vue
```

```
<template lang="html">  <component :is="data.component" v-bind="data.props"></component></template>
```

```
<script>import * as components from './components'
```

```
export default {  name: 'comp-wrapper',
```

```
 components,
```

```
 data () {    return {      data: {}    }  },
```

```
 created() {    this.data = JSON.parse(this.$attrs.data)  }}</script>
```

```
<style lang="scss" scoped></style>
```

CompWrapper 组件做的第一件事是获取您在 Rails 视图中的元素上设置的数据属性，解析 JSON，并用解析的数据设置一个内部 Vue 数据属性:

```
created() {  this.data = JSON.parse(this.$attrs.data)}
```

设置好`this.data`之后，我们可以使用它来选择我们想要使用的组件，并使用 Vue 动态组件将我们在 Rails 视图中提供的道具传递给它:

```
<component :is="data.component" v-bind="data.props"></component>
```

就是这样！

我们现在可以使用 Vue 组件了，但是是在我们的 rails 视图中。？