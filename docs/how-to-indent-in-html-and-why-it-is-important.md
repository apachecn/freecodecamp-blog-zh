# 如何缩进 HTML 代码——以及为什么它很重要

> 原文：<https://www.freecodecamp.org/news/how-to-indent-in-html-and-why-it-is-important/>

当你构建 HTML 文件时，缩进你的代码是非常重要的。但是如何在 HTML 中做到这一点，为什么它很重要？

在本文中，我将向您展示如何正确缩进 HTML 文件，并解释为什么正确格式化代码很重要。

## 如何在 HTML 中缩进代码

每当 HTML 元素嵌套在其他 HTML 元素中时，最好使用缩进。嵌套元素被称为其父元素的子元素。

在这个例子中，我有一个嵌套在`div`元素中的`p`元素。为了缩进`p`元素，我将把它向右移动两格。

```
<div>
  <p>This is what indentation looks like for HTML</p>
</div>
```

这被认为是最佳实践，将使您的代码更容易被其他开发人员阅读。现在我们可以看到`p`元素嵌套在它的父元素`div`中。

在下一个例子中，我将一个`h2`和`p`元素嵌套在一个没有缩进的`main`元素**中。**

```
<main>
<h2>Let's learn about indentation</h2>
<p>There is no indentation here</p>
</main>
```

但是如果我通过将`h2`和`p`元素向右移动两个空格来编辑代码，现在我们有了正确的缩进。

```
<main>
  <h2>Let's learn about indentation</h2>
  <p>This is indentation</p>
</main>
```

`h2`和`p`元素是`main`元素的子元素。

## HTML 中常用的缩进示例

### 无序列表

`li`元素向右缩进两个空格，并嵌套在`ul`元素中。`ul`元素是`li`元素的父元素。

```
<ul>
  <li>Cake</li>
  <li>Pizza</li>
  <li>Salad</li>
  <li>Apple</li>
</ul>
```

### 有序列表

`li`元素向右缩进两个空格，并嵌套在`ol`元素中。`ol`元素是`li`元素的父元素。

```
<ol>
  <li>Drive 1.2 miles and turn left on Cherry lane</li>
  <li>Drive 4.5 miles and turn right on Sycamore Rd.</li>
  <li>Drive 400 feet and stop at the light</li>
  <li>Turn left at the light</li>
  <li>Arrive at the destination on your right</li>
</ol>
```

## 为什么缩进很重要？

当您编写代码时，编写其他开发人员可读的代码是非常重要的。可读性的很大一部分是适当地缩进你的 HTML。

在这个例子中，我通过构建一个 Cat Photo App 项目复制了来自 [Learn HTML 的所有代码，并删除了所有缩进，以向您展示糟糕的代码格式是什么样子。](https://www.freecodecamp.org/learn/2022/responsive-web-design/#learn-html-by-building-a-cat-photo-app)

```
<html lang="en">
<head>
<title>CatPhotoApp</title>
</head>
<body>
<h1>CatPhotoApp</h1>
<main>
<section>
<h2>Cat Photos</h2>
<!-- TODO: Add link to cat photos -->
<p>
Click here to view more
<a target="_blank" href="https://freecatphotoapp.com">cat photos</a>.
</p>
<a href="https://freecatphotoapp.com"
><img
src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/relaxing-cat.jpg"
alt="A cute orange cat lying on its back."
/></a>
</section>
<section>
<h2>Cat Lists</h2>
<h3>Things cats love:</h3>
<ul>
<li>cat nip</li>
<li>laser pointers</li>
<li>lasagna</li>
</ul>
<figure>
<img
src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/lasagna.jpg"
alt="A slice of lasagna on a plate."
/>
<figcaption>Cats <em>love</em> lasagna.</figcaption>
</figure>
<h3>Top 3 things cats hate:</h3>
<ol>
<li>flea treatment</li>
<li>thunder</li>
<li>other cats</li>
</ol>
<figure>
<img
src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/cats.jpg"
alt="Five cats looking around a field."
/>
<figcaption>Cats <strong>hate</strong> other cats.</figcaption>
</figure>
</section>
<section>
<h2>Cat Form</h2>
<form action="https://freecatphotoapp.com/submit-cat-photo">
<fieldset>
<legend>Is your cat an indoor or outdoor cat?</legend>
<label
><input
id="indoor"
type="radio"
name="indoor-outdoor"
value="indoor"
checked
/>
Indoor</label
>
<label
><input
id="outdoor"
type="radio"
name="indoor-outdoor"
value="outdoor"
/>
Outdoor</label
>
</fieldset>
<fieldset>
<legend>What's your cat's personality?</legend>
<input
id="loving"
type="checkbox"
name="personality"
value="loving"
checked
/>
<label for="loving">Loving</label>
<input id="lazy" type="checkbox" name="personality" value="lazy" />
<label for="lazy">Lazy</label>
<input
id="energetic"
type="checkbox"
name="personality"
value="energetic"
/>
<label for="energetic">Energetic</label>
</fieldset>
<input
type="text"
name="catphotourl"
placeholder="cat photo URL"
required
/>
<button type="submit">Submit</button>
</form>
</section>
</main>
<footer>
<p>
No Copyright -
<a href="https://www.freecodecamp.org">freeCodeCamp.org</a>
</p>
</footer>
</body>
</html> 
```

这根本不是好的 HTML 实践，因为阅读和理解代码做什么真的很难。如果你试图在一个专业的开发环境中提交这样的东西，你的团队根本不会对你满意。

现在，我将采用完全相同的代码，并适当缩进，以向您展示不同之处。

```
<html lang="en">
  <head>
    <title>CatPhotoApp</title>
  </head>
  <body>
    <h1>CatPhotoApp</h1>
    <main>
      <section>
        <h2>Cat Photos</h2>
        <!-- TODO: Add link to cat photos -->
        <p>
          Click here to view more
          <a target="_blank" href="https://freecatphotoapp.com">cat photos</a>.
        </p>
        <a href="https://freecatphotoapp.com"
          ><img
            src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/relaxing-cat.jpg"
            alt="A cute orange cat lying on its back."
        /></a>
      </section>
      <section>
        <h2>Cat Lists</h2>
        <h3>Things cats love:</h3>
        <ul>
          <li>cat nip</li>
          <li>laser pointers</li>
          <li>lasagna</li>
        </ul>
        <figure>
          <img
            src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/lasagna.jpg"
            alt="A slice of lasagna on a plate."
          />
          <figcaption>Cats <em>love</em> lasagna.</figcaption>
        </figure>
        <h3>Top 3 things cats hate:</h3>
        <ol>
          <li>flea treatment</li>
          <li>thunder</li>
          <li>other cats</li>
        </ol>
        <figure>
          <img
            src="https://cdn.freecodecamp.org/curriculum/cat-photo-app/cats.jpg"
            alt="Five cats looking around a field."
          />
          <figcaption>Cats <strong>hate</strong> other cats.</figcaption>
        </figure>
      </section>
      <section>
        <h2>Cat Form</h2>
        <form action="https://freecatphotoapp.com/submit-cat-photo">
          <fieldset>
            <legend>Is your cat an indoor or outdoor cat?</legend>
            <label
              ><input
                id="indoor"
                type="radio"
                name="indoor-outdoor"
                value="indoor"
                checked
              />
              Indoor</label
            >
            <label
              ><input
                id="outdoor"
                type="radio"
                name="indoor-outdoor"
                value="outdoor"
              />
              Outdoor</label
            >
          </fieldset>
          <fieldset>
            <legend>What's your cat's personality?</legend>
            <input
              id="loving"
              type="checkbox"
              name="personality"
              value="loving"
              checked
            />
            <label for="loving">Loving</label>
            <input id="lazy" type="checkbox" name="personality" value="lazy" />
            <label for="lazy">Lazy</label>
            <input
              id="energetic"
              type="checkbox"
              name="personality"
              value="energetic"
            />
            <label for="energetic">Energetic</label>
          </fieldset>
          <input
            type="text"
            name="catphotourl"
            placeholder="cat photo URL"
            required
          />
          <button type="submit">Submit</button>
        </form>
      </section>
    </main>
    <footer>
      <p>
        No Copyright -
        <a href="https://www.freecodecamp.org">freeCodeCamp.org</a>
      </p>
    </footer>
  </body>
</html> 
```

这更容易阅读，现在我们可以看到所有嵌套的子元素在它们的父元素中，并且理解代码在做什么。

## 结论

编写 HTML 时，使用良好的缩进来正确格式化代码是很重要的。您可以通过将元素向右移动两个空格来缩进元素。

```
<main>
  <h2>Let's learn about indentation</h2>
  <p>This is indentation</p>
</main>
```

这将使您的代码更容易被其他开发人员阅读，并显示子 HTML 元素和父 HTML 元素之间的关系。

我希望您喜欢这篇文章，并祝您的开发者之旅好运。