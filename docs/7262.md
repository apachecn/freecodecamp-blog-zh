# 使用 Vue.js 时要避免的常见错误

> 原文：<https://www.freecodecamp.org/news/common-mistakes-to-avoid-while-working-with-vue-js-10e0b130925b/>

想找个前端框架试试，从 React 开始，然后试了 Vue.js。

不幸的是**，**我一开始就遇到了很多 Vue.js 的问题。在这篇文章中，我想分享一些你在使用 Vue.js 时可能必须处理的常见问题。其中一些问题可能看起来很明显，但我认为分享我的经验可能会对某人有所帮助。

### 包括模板编译器

我的第一个问题是一个非常基本的问题。为了使用 Vue.js，首先要做的是导入它。如果你遵循官方指南并为你的组件使用内嵌模板，你将得到一个空白页。

```
import Vue from 'vue';
var vm = new Vue({
  el: '#vm',
  template: '<div>Hello World</div>',
});
```

请注意，当您使用 render 函数或 SFC ( [单个文件组件](https://vuejs.org/v2/guide/single-file-components.html#ad))定义模板时，这个问题不会发生。

其实还有很多[Vue build](https://vuejs.org/v2/guide/installation.html#Explanation-of-Different-Builds)。NPM 包导出的默认构建是仅运行时构建**。它没有带模板编译器。**

**对于一些背景信息，模板编译器的工作方式完全类似于 JSX 对反应的工作方式。它用函数调用替换模板字符串来创建虚拟 DOM 节点。**

```
`// #1: import full build in JavaScript file
import Vue from 'vue/dist/vue.js';

// OR #2: make an alias in webpack configuration
config.resolve: {
  alias: { vue: 'vue/dist/vue.js' }
}

// OR #3: use render function directly
var vm = new Vue({
  el: '#vm',
  render: function(createElement) {
    return createElement('div', 'Hello world');
  }
});`
```

**有了 sfc，这个问题就不会发生了。 **vue-loader** 和 **vueify** 都是用于处理 sfc 的工具。他们使用 render 函数定义模板来生成普通的 JavaScript 组件。**

**要在组件中使用字符串模板，请使用完整的 Vue.js 版本。**

**总之，要解决此问题，请在导入过程中指定正确的版本，或者在 bundler 配置中为 Vue 创建别名。**

**您应该注意，使用字符串模板会降低应用程序的性能，因为编译是在运行时进行的。**

**定义组件模板还有很多方法，所以[看看这篇文章](https://vuejsdevelopers.com/2017/03/24/vue-js-component-templates/)。另外，我推荐在 Vue 实例中使用[渲染功能。](https://vuejsdevelopers.com/2017/09/17/vue-js-avoid-dom-templates/)**

### **保持属性的反应性**

**如果您使用 React，您可能知道它的反应依赖于调用`setState`函数来更新属性值。**

**和 Vue.js 的反应性有点不一样。它基于代理组件属性。 [Getter 和 setter](https://vuejs.org/v2/guide/reactivity.html#How-Changes-Are-Tracked) 函数将被覆盖，并将通知对虚拟 DOM 的更新。**

```
`var vm = new Vue({
  el: '#vm',
  template: `<div>{{ item.count }}<input type="button" value="Click" @click="updateCount"/></div>`,
  data: {
    item: {}
  },
  beforeMount () {
    this.$data.item.count = 0;
  },
  methods: {
    updateCount () {
      // JavaScript object is updated but
      // the component template is not rendered again
      this.$data.item.count++;
    }
  }
});`
```

**在上面的代码片段中，Vue 实例有一个名为`item`的属性(在数据中定义)。此属性包含一个空的文本对象。**

**在组件初始化期间，Vue 在附加到`item`属性的`get`和`set`函数下创建一个代理。因此，框架将观察值的变化并最终做出反应。**

**然而，`count`属性不是反应性的，因为它没有在初始化时声明。**

**实际上，代理只发生在组件初始化时，而`beforeMount`生命周期函数稍后触发。**

**此外，在定义`count`期间不会调用`item` setter。因此代理不会被触发，并且`count`属性将没有监视。**

```
`beforeMount () {
  // #1: Call parent setter
  // item setter is called so proxifying is propaged
  this.$data.item = {
    count: 0
  };

  // OR #2: explicitly ask for watching
  // item.count got its getter and setter proxyfied
  this.$set(this.$data.item, 'count', 0);

  // "Short-hand" for:
  Vue.set(this.$data.item, 'count', 0);
}`
```

**如果你在`beforeMount`里保留了`item.count`的做作，以后再叫`Vue.set`就不会造出手表了。**

**当对未知索引使用直接影响时，数组也会出现完全相同的问题。在这种情况下，你应该首选[数组代理函数](https://vuejs.org/v2/guide/list.html#Array-Change-Detection)，比如`push`和`slice`。**

**还有，你可以从 Vue.js 开发者的网站上阅读[这篇文章](https://vuejsdevelopers.com/2017/03/05/vue-js-reactivity/)。**

### **小心 SFC 导出**

**可以在 JavaScript 文件中常规使用 Vue，但是也可以使用[单个文件组件](https://vuejs.org/v2/guide/single-file-components.html#ad)。它有助于将 JavaScript、HTML 和 CSS 代码收集到一个文件中。**

**对于 SFCs，组件代码是一个`.vue`文件的脚本标签。仍然用 JavaScript 编写，它必须导出组件。**

**有许多方法可以导出变量/组件。通常，我们使用直接导出、命名导出或默认导出。命名导出将阻止用户重命名组件。也将获得[摇树](https://en.wikipedia.org/wiki/Tree_shaking)的资格。**

```
`/* File: user.vue */
<template>
  <div>{{ user.name }}</div>
</template>

<script>
  const User = {
    data: () => ({
      user: {
        name: 'John Doe'
      }
    })
  };
  export User; // Not work
  export default User; // Works
</script>

/* File: app.js */
import {User} from 'user.vue'; // Not work
import User from 'user.vue'; // Works (".vue" is required)`
```

**使用命名导出与 sfc 不兼容，请注意这一点！**

**总之，默认情况下导出命名变量可能是个好主意。这样，您将获得更明确的调试消息。**

### **不要混合 SFC 组件**

**将代码、模板和样式放在一起是一个好主意。此外，根据您的约束和观点，您可能希望保持[关注点分离](https://en.wikipedia.org/wiki/Separation_of_concerns)。**

**如 [Vue 文档](https://vuejs.org/v2/guide/single-file-components.html#What-About-Separation-of-Concerns)中所述，它与 SFC 兼容。**

**后来，我想到了一个主意。使用相同的 JavaScript 代码，并将其包含在不同的模板中。你可能会指出这是一种不好的做法，但它让事情变得简单。**

**例如，一个组件可以同时具有只读和读写模式，并保持相同的实现。**

```
`/* File: user.js */
const User = {
  data: () => ({
    user: {
      name: 'John Doe'
    }
  })
};
export default User;

/* File: user-read-only.vue */
<template><div>{{ user.name }}</div></template>
<script src="./user.js"></script>

/* File: user-read-write.vue */
<template><input v-model="user.name"/></template>
<script src="./user.js"></script>`
```

**在这个代码片段中，只读和读写模板使用相同的 JavaScript 用户组件。**

**一旦您导入了这两个组件，您就会发现它并不像预期的那样工作。**

```
`// The last import wins
import UserReadWrite from './user-read-write.vue';
import UserReadOnly from './user-read-only.vue';

Vue.component('UserReadOnly', UserReadOnly);
Vue.component('UserReadWrite', UserReadWrite);

// Renders two times a UserReadOnly
var vm = new Vue({
  el: '#vm',
  template: `<div><UserReadOnly/><UserReadWrite/></div>`
});`
```

**在`user.js`中定义的组件只能在一个 SFC 中使用。否则，使用它的最后一个导入的 SFC 将覆盖先前的 SFC。**

> **sfc 允许将代码拆分到单独的文件中。但是你不能在多个 Vue 组件中导入这些文件。**

**简单来说，不要在多个 SFC 组件中重用 JavaScript 组件代码。独立代码特性是一种方便的语法，而不是一种设计模式。**

**感谢阅读，希望我的经历对你避免我犯的错误有用。**

****如果有用，请点击**？**按钮几下，让别人找到文章，表示你的支持！？****

****别忘了关注我，了解我即将发布的文章**？**

### **查看我的其他文章**

#### **JavaScript**

*   **[如何通过编写自己的 Web 开发框架来提高自己的 JavaScript 技能？](https://www.freecodecamp.org/news/how-to-improve-your-javascript-skills-by-writing-your-own-web-development-framework-eed2226f190/)**
*   **[停止痛苦的 JavaScript 调试，用源代码图拥抱 Intellij】](https://medium.com/dailyjs/stop-painful-javascript-debug-and-embrace-intellij-with-source-map-6fe68eda8555)**

#### **为初学者反应系列**

*   **[从第一篇文章开始](https://www.freecodecamp.org/news/a-quick-guide-to-learn-react-and-how-its-virtual-dom-works-c869d788cd44/)**
*   **[获取最佳实践指南](https://www.freecodecamp.org/news/the-beginners-collection-of-powerful-tips-and-tricks-for-react-f2e3833c6f12/)**