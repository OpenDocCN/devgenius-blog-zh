# 面向对象的 JavaScript——惰性定义和私有变量

> 原文：<https://blog.devgenius.io/object-oriented-javascript-lazy-definition-and-private-variables-d7cf29ff27fd?source=collection_archive---------5----------------------->

![](img/ca84352e71466da9cd3a2e9dec7530d1.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[霍尔格连杆](https://unsplash.com/@photoholgic?utm_source=medium&utm_medium=referral)拍摄

JavaScript 部分是面向对象的语言。

要学习 JavaScript，我们必须学习 JavaScript 的面向对象部分。

在本文中，我们将研究一些基本的编码和设计模式。

# 懒惰定义

惰性定义模式是我们在运行函数时将它分配给一个属性。

例如，我们可以写:

```
const APP = {};
APP.click = {
  addListener(el, type, fn) {
    if (el.addEventListener) {
      APP.click.addListener = function(el, type, fn) {
        el.addEventListener(type, fn, false);
      };
    } else if (el.attachEvent) {
      APP.click.addListener = function(el, type, fn) {
        el.attachEvent(`on${type}`, fn);
      };
    } else {
      APP.click.addListener = function(el, type, fn) {
        el[`on${type}`] = fn;
      };
    }
    APP.click.addListener(el, type, fn);
  }
};
```

我们有在运行`APP.click.addListener`方法时运行的`addListener`方法。

我们可以根据哪个可用来设置功能。

# 配置对象

我们可以创建一个配置对象，让我们从那里获得配置。

对象比参数更好，因为它们的顺序无关紧要。

我们也可以很容易地跳过不想添加的参数。

添加可选配置很容易。

代码可读性更好，因为配置对象的属性在代码中。

例如，不要写:

```
APP.dom.CustomDiv = function(text, color) {
  const div = document.createElement('div');
  div.color = color|| 'green';
  div.textContent = text;
  return div;
};
```

我们写道:

```
APP.dom.CustomDiv = function({
  text,
  color = 'green'
}) {
  const div = document.createElement('div');
  div.color = color;
  div.textContent = text;
  return div;
};
```

我们解构了对象属性，这样我们就可以使用变量了。

它更短更干净。

属性的顺序并不重要。

我们只是传入一个对象作为参数来使用它:

```
new APP.dom.CustomDiv({ text: 'click me', color: 'blue' })
```

如果参数在一个对象中，很容易切换它们的顺序。

所有的属性都应该是独立的和可选的。

# 私有属性和方法

我们可以通过将属性和方法保存在一个对象中来用 JavaScript 保存它们。

例如，我们可以写:

```
APP.dom.CustomDiv = function({
  text,
  type = 'sumbit'
}) {
  const styles = {
    font: 'arial',
    border: '1px solid black',
    color: 'black',
    background: 'grey'
  }; const div = document.createElement('div');
  div.styles = {
    ...div.styles,
    ...styles
  };
  div.type = type;
  div.textContent = text;
  return div;
};
```

我们有了`styles`对象，它有我们想要应用到 div 的样式。

既然在函数内部，我们就不能从外部改变它。

我们还可以添加一个方法来设置方法内部的样式:

```
APP.dom.CustomDiv = function({
  text,
  type = 'sumbit'
}) {
  const styles = {
    font: 'arial',
    border: '1px solid black',
    color: 'black',
    background: 'grey'
  }; const div = document.createElement('div');
  const setStyles = (el) => {
    el.styles = {
      ...el.styles,
      ...styles
    };
  } div.type = type;
  div.textContent = text;
  setStyles(div);
  return div;
};
```

我们有只能在`CustomDiv`构造函数内部调用的`setStyles`私有函数。

![](img/453b7ef355996558b5676c3088c4586c.png)

照片由[戴恩·托普金](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

当我们调用函数时，我们可以把它们赋予一个属性。

此外，我们可以通过将值放入函数中来保持它们的私有性。