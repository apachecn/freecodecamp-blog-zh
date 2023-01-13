# JavaScript Array.some()教程——如何遍历数组中的元素

> 原文：<https://www.freecodecamp.org/news/javascript-array-some-tutorial-how-to-iterate-through-elements-in-an-array/>

当您在 JavaScript 中使用数组时，有时您可能只想检查数组中的**是否至少有一个**元素通过了测试。你可能不关心任何后续的比赛。

在这种情况下，您应该使用`some()` JavaScript 方法。所以让我们看看它是如何工作的。

## 如何使用 Array.some() JavaScript 方法

`some()`方法是一个`Array.propotype`(内置)方法，它接受一个回调函数，并将在每次迭代中针对当前项目测试该函数。

如果数组中的任何元素通过了回调中指定的测试，该方法将停止迭代并返回`true`。如果没有元素通过测试，该方法返回`false`。

该方法接受三个参数:

*   `currentItem`:这是当前正在迭代的数组内部的元素
*   `index`:这是数组内`currentItem`的索引位置
*   `array`:表示`some()`方法绑定到的数组集合

理解`Array.some()`方法背后主要思想的一个简单方法是考虑我们作为人类最大的倾向之一:**一般化**。人们倾向于仅仅基于一个单一的经历或感知来进行归纳。

例如，如果某个地方的某个人有某种行为方式，很多人会认为那个地方的每个人也有同样的行为方式。即使这样的假设仅仅基于一次经历。

`some()`方法实质上是**在找到匹配的那一刻就下定决心**，并返回`true`。

## 如何在 JavaScript 中使用 Array.some()

在下面的例子中，我们将实际看看如何在 JavaScript 中使用`some()`方法。

### 如何测试与`some()`的至少一项匹配

在这个例子中，我们将检查在`people`数组中是否至少有一个男性

```
let people = [{
    name: "Anna",
    sex: "Female"
  },

  {
    name: "Ben",
    sex: "Male"
  },

  {
    name: "Cara",
    sex: "Female"
  },

  {
    name: "Danny",
    sex: "Female"
  }

]

function isThereMale(person) {
	return person.sex === "Male"
}

console.log(people.some(person => isThereMale(person)) // true
```

由于男性确实存在，`some()`方法返回 true。

即使我们在数组中定义了两个男性，这个方法仍然会返回`true`。方法不关心有没有第二个雄性，只关心第一个。

```
let people = [{
    name: "Anna",
    sex: "Female"
  },

  {
    name: "Ben",
    sex: "Male"
  },

  {
    name: "Cara",
    sex: "Female"
  },

  {
    name: "Danny",
    sex: "Female"
  },

  {
    name: "Ethan",
    sex: "Male"
  }

]

function isThereMale(person) {
	return person.sex === "Male"
}

console.log(people.some(person => isThereMale(person)) // true
```

如果数组中的所有项都没有通过回调测试，`some()`方法将返回`false`。

在本例中，由于我们的 people 数组中没有男性，因此将返回`false`:

```
let people = [{
    name: "Anna",
    sex: "Female"
  },

  {
    name: "Bella",
    sex: "Female"
  },

  {
    name: "Cara",
    sex: "Female"
  },

  {
    name: "Danny",
    sex: "Female"
  },

  {
    name: "Ella",
    sex: "Female"
  }

]

function isThereMale(person) {
		return person.sex === "Male"
}

console.log(people.some(person => isThereMale(person))) // false 
```

### 如何通过`some()`使用索引参数

在`some()`中定义的回调函数可以访问被迭代的每一项的 index 属性。索引只是一个数值，唯一地标识数组中每个元素的位置。这样，您可以通过索引引用数组中的任何元素。

这里，我们使用索引值来构造一条消息，并将其登录到控制台:

```
let people = [{
    name: "Anna",
    sex: "Female"
  },

  {
    name: "Ben",
    sex: "Male"
  },

  {
    name: "Cara",
    sex: "Female"
  },

  {
    name: "Danny",
    sex: "Female"
  },

  {
    name: "Ethan",
    sex: "Male"
  }

]

function isThereMale(person, index) {
		if (person.sex === "Male") console.log(`No ${index+1}, which is ${person.name}, is a Male`)
		return person.sex === "Male"
}

console.log(people.some((person, index) => isThereMale(person, index)))

/* 
"No 2, which is Ben, is a Male"

true
*/
```

## 如何与`some()`一起使用上下文对象

除了回调函数，`some()`还可以接受一个上下文对象。

```
some(callbackFn, contextObj)
```

然后，我们可以在每次迭代中使用`this`作为引用，从 ****回调**** 函数中引用对象。这允许我们访问上下文对象中定义的任何属性或方法。

### 与`some()`一起使用上下文对象的示例

在本例中，我们要检查 people 数组中是否至少有一个人是三名成员。也就是说，这个人的年龄在 30 到 39 岁之间。

我们可以在`conditions`对象中定义规则，然后使用`this.property`符号从回调函数中访问它。然后，我们执行检查，以确定是否至少有一个人符合标准。

```
let people = [{
    name: "Anna",
    age: 20
  },

  {
    name: "Ben",
    age: 35
  },

  {
    name: "Cara",
    age: 8
  },

  {
    name: "Danny",
    age: 17
  },

  {
    name: "Ethan",
    age: 28
  }

]

let conditions = {
	lowerAge: 30,
  upperAge: 39
}

console.log(people.some(person => function(person) {
	return person.age >= this.lowerAge && person.age <= this.upperAge
}, conditions)) // true
```

由于有一个人(Ben)落在该范围内，`some()`将返回`true`。

**`some()`方法是一个`Array.prototype`方法，它接受一个回调函数，并为绑定数组中的每一项调用该函数。**

**当一个项目通过回调测试时，该方法将返回`true`并停止循环。否则返回`false`。**

**除了回调函数，`some()`方法还可以接受一个上下文对象作为第二个参数。这将使您能够使用`this`从回调函数中访问它的任何属性。**

**我希望你能从这篇文章中得到一些有用的东西。**

******************I****************如果你想了解更多关于 Web 开发的知识，可以随时访问我的****************[博客****************。****************](https://ubahthebuilder.tech/the-ultimate-tutorial-on-javascript-dom-js-dom-with-examples)******************

感谢您的阅读，再见。

> ********************************************【**************【**************************** :如果你是学 JavaScript，我就创造了一个教什么的电子书 [在这里签出](https://ubahthebuilder.gumroad.com/l/js-50)。**